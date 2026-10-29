# dclv2.ref-labs.near — recipes

task shaped how-tos for the DCL v2 DEX. each one names the functions and
the bins to use, in order.

every recipe here was **run end to end** against `dclv2.ref-dev.testnet` on
2026-10-29 and the outcome was confirmed on chain. the unverified flows
are collected at the bottom.

see [SKILL.md](./SKILL.md) for the overview and the method reference.

---

#### BEFORE ANYTHING

**is the DEX registered on my token?**

```bash
ft_storage_balance_of_bin_json <token> <dcl_id>
```

a `null` means the DEX is not an FT holder of that token — every inflow
of it fails with `The account <dcl_id> is not registered`. there is no
way around this from the caller side.

**do I have storage on the DEX?**

```bash
rhea_dclv2_storage_balance_of_bin_json <my_account> <dcl_id>
```

`None` means no storage. `add_liquidity`, `remove_liquidity` and limit
orders all need it. register it with:

```bash
rhea_dclv2_storage_deposit_bin_json <dcl_id>
```

that attaches 0.5 NEAR. the bare `storage_balance_bounds` minimum is
rejected with `E102`.

**does a pool exist for my token?**

```bash
rhea_dclv2_list_pools_bin_json <from_index> <limit> <dcl_id>
```

the pool id is `<token_x>|<token_y>|<fee>` — both slots are plain account
ids. a token with no pool can still be deposited into the internal
balance, but it cannot be swapped or LP'd.

---

### how do I swap one token for another?

three steps, and the first one is optional but recommended.

**1. quote it**

```bash
rhea_dclv2_quote_bin_json "<pool_ids_csv>" <input_token> <output_token> <input_amount> <tag> <dcl_id>
```

```
rhea_dclv2_quote_bin_json "usdc.fakes.testnet|wrap.testnet|2000" wrap.testnet usdc.fakes.testnet 100000000000000000000000 Auto dclv2.ref-dev.testnet
-> { amount: "28691277889", tag: "Auto" }
```

the `pool_ids_csv` is comma separated, it is the **route** through the
pools. `tag` is optional and is echoed back.

**2. swap it**

```bash
rhea_dclv2_swap_bin_json <token_in> <amount_in> <pool_ids_csv> <output_token> <min_output> <dcl_id>
```

```
rhea_dclv2_swap_bin_json wrap.testnet 100000000000000000000000 "usdc.fakes.testnet|wrap.testnet|2000" usdc.fakes.testnet 28000000000 dclv2.ref-dev.testnet
```

`min_output` is the slippage floor — take the quote amount and shave
your tolerance off it. `"0"` accepts any output price.

the output lands **in the wallet**, not the internal balance. the swap
does not need storage on the DEX, but the *output* token must have
storage registered for the signer.

**swap to an exact output amount instead**

```bash
rhea_dclv2_quote_by_output_bin_json "<pool_ids_csv>" <input_token> <output_token> <output_amount> <dcl_id>
```

```
rhea_dclv2_quote_by_output_bin_json "usdc.fakes.testnet|wrap.testnet|2000" wrap.testnet usdc.fakes.testnet 1000000000 dclv2.ref-dev.testnet
-> { amount: "3483498336658883468384", tag: null }
```

the `amount` is the **input** needed for that exact output.

---

### how do I add liquidity?

four steps.

**1. make sure storage is registered**

```bash
rhea_dclv2_storage_deposit_bin_json <dcl_id>
```

**2. fund the internal balance**

`add_liquidity` pulls from the internal balance, so the tokens have to be
in there first.

```bash
rhea_dclv2_deposit_bin_json <token_id> <amount> <dcl_id>
```

```
rhea_dclv2_deposit_bin_json wrap.testnet 100000000000000000000000 dclv2.ref-dev.testnet
```

deposit each token you plan to use. a range entirely above
`current_point` only needs `amount_x`, one entirely below only needs
`amount_y`.

**3. pick the points**

read the pool first:

```bash
rhea_dclv2_get_pool_bin_json "<pool_id>" <dcl_id>
```

```
-> point_delta: 40
-> current_point: 288785
```

the points must be multiples of `point_delta` (40 here), else `E200`.
`current_point` is 288785, which is *not* a multiple of 40, so the
aligned points around it are 288760 and 288800.

**4. add the liquidity**

```bash
rhea_dclv2_add_liquidity_bin_json <pool_id> <left_point> <right_point> <amount_x> <amount_y> <dcl_id>
```

pass `"0"` for the side the range does not use. the position is now
known by its `lpt_id`.

#### THE THREE RANGE SHAPES

the shape of the range against `current_point` decides which token is
needed, and how much of it. with `current_point` 288785 and `point_delta`
40, the aligned range edges around it are 288760 (below) and 288800 (above).

**a range entirely BELOW current point needs only `amount_y`** (the second
token of the pool id):

```bash
rhea_dclv2_add_liquidity_bin_json "usdc.fakes.testnet|wrap.testnet|2000" 288560 288760 0 1000000000000000000000 dclv2.ref-dev.testnet
-> liquidity_added, lpt_id "usdc.fakes.testnet|wrap.testnet|2000#2544"
-> paid_token_x "0", paid_token_y "999999999999807010505"
```

**a range entirely ABOVE current point needs only `amount_x`** (the first
token of the pool id):

```bash
rhea_dclv2_add_liquidity_bin_json "usdc.fakes.testnet|wrap.testnet|2000" 288800 288880 5000000 0 dclv2.ref-dev.testnet
-> liquidity_added, lpt_id "usdc.fakes.testnet|wrap.testnet|2000#2546"
-> paid_token_x "5000000", paid_token_y "0"
```

**a range that STRADDLES current point needs both**, and it takes much
more of the second token than a single-sided range of the same width:

```bash
rhea_dclv2_add_liquidity_bin_json "usdc.fakes.testnet|wrap.testnet|2000" 288760 288800 5000000 40000000000000000000000 dclv2.ref-dev.testnet
-> liquidity_added, lpt_id "usdc.fakes.testnet|wrap.testnet|2000#2547"
-> paid_token_x "5000000", paid_token_y "32274009234898315497"
```

the straddling range took 0.0323 wrap for 5 usdc, where a single-sided
range of the same width took a comparable amount of one side only. it
only takes what it needs — the 0.04 wrap deposited was more than the
0.0323 it used, and the surplus stayed in the internal balance.

**deposit enough of both tokens before a two-sided add.** a two-sided
range that is short on the second token fails with
`E101: insufficient balance`, and the contract reports the amount it
wanted in the `liquidity_added` event.

> `predict_add_liquidity` is the read-only way to check what a range
> needs, but it currently panics with
> `contracts/dcl/src/dcl/pool.rs:1138:33: attempt to subtract with
> overflow`. `add_liquidity` itself works.

---

### how do I remove liquidity?

**1. find the position**

```bash
rhea_dclv2_list_liquidities_bin_json <my_account> <from_index> <limit> <dcl_id>
```

that gives the `lpt_id` and the `amount` of liquidity in the position.

**2. remove it**

```bash
rhea_dclv2_remove_liquidity_bin_json <lpt_id> <amount> <dcl_id>
```

```
rhea_dclv2_remove_liquidity_bin_json "usdc.fakes.testnet|wrap.testnet|2000#2544" 2698486080695 dclv2.ref-dev.testnet
```

`amount` is the **liquidity amount**, not a token amount. the tokens land
in the **internal balance**, so pull them out with `withdraw_asset`.

the refund mirrors the deposit — a single-sided position gives back only
the side it took, and a two-sided position gives back both:

```
# above-current, single sided on amount_x
-> refund_token_x "4999999", refund_token_y "0"

# below-current, single sided on amount_y
-> refund_token_x "0", refund_token_y "999999999999807010504"
```

**check what you would get back first**

```bash
rhea_dclv2_predict_remove_liquidity_bin_json <lpt_id> <amount> <dcl_id>
```

```
rhea_dclv2_predict_remove_liquidity_bin_json "usdc.fakes.testnet|wrap.testnet|2000#2545" 2698486080695 dclv2.ref-dev.testnet
-> ["0", "999999999999807010504"]
```

the two values are the `amount_x` and `amount_y` you get back.

---

### how do I get my tokens back out?

**1. check the internal balance**

```bash
rhea_dclv2_get_user_asset_bin_json <my_account> <token_id> <dcl_id>
```

or for every token at once:

```bash
rhea_dclv2_list_user_assets_bin_json <my_account> <dcl_id>
```

**2. withdraw**

```bash
rhea_dclv2_withdraw_asset_bin_json <token_id> [amount] [dcl_id]
```

the bin disambiguates its arguments by shape: a second argument that
contains a `.` is read as the dcl id and the amount comes from the third
argument. so both of these work:

```bash
rhea_dclv2_withdraw_asset_bin_json wrap.testnet 99000000000000000000000 dclv2.ref-dev.testnet   # partial
rhea_dclv2_withdraw_asset_bin_json wrap.testnet dclv2.ref-dev.testnet                             # the whole balance
```

withdrawing more than the internal balance holds fails with
`E101: insufficient balance`.

---

### how do I place a limit order?

**1. read the pool's live order points**

```bash
rhea_dclv2_get_marketdepth_bin_json <pool_id> <depth> <dcl_id>
```

the `orders` key holds the points that already have orders on them.
**2. place the order**

```bash
rhea_dclv2_limit_order_bin_json <sell_token> <amount> <pool_id> <buy_token> <point> [with_swap] [dcl_id]
```

```
rhea_dclv2_limit_order_bin_json wrap.testnet 1000000000000000000000 "usdc.fakes.testnet|wrap.testnet|2000" usdc.fakes.testnet 286880 false dclv2.ref-dev.testnet
-> order "usdc.fakes.testnet|wrap.testnet|2000#409"
```

`buy_token` is the token being **bought** — the one you do not send. you
send the **sell** token.

`with_swap` is `true` for `LimitOrderWithSwap` (fills what it can right
away, then rests the remainder) and `false` for the plain `LimitOrder`
variant (rests entirely).

the `point` must be on the correct side of `current_point` for the token
you are selling. selling `token_y` wants a point **below**, selling
`token_x` wants one **above**. a point that is aligned but on the wrong
side fails with `E202: illegal point`.

---

### how do I cancel a limit order?

**1. read the order**

```bash
rhea_dclv2_get_order_bin_json <order_id> <dcl_id>
rhea_dclv2_list_active_orders_bin_json <my_account> <dcl_id>
```

`remain_amount` is what is left to cancel, `unclaimed_amount` is what a
fill already earned.

**2. cancel**

```bash
rhea_dclv2_cancel_order_bin_json <order_id> [amount] [dcl_id]
```

```
rhea_dclv2_cancel_order_bin_json "usdc.fakes.testnet|wrap.testnet|2000#409" 500000000000000000000 dclv2.ref-dev.testnet
```

- an **amount** cancels that much of the order
- `"0"` claims the fills without cancelling the rest
- to cancel the whole thing, pass the full `remain_amount` **and** the
  dcl id in the third slot — the second slot is always read as the
  amount, so putting the dcl id there sends it as the amount and the
  contract rejects it with `invalid digit found in string`

---

### how do I release my storage?

**1. withdraw the excess**

```bash
rhea_dclv2_storage_withdraw_bin_json <dcl_id>
```

this returns everything above the required minimum — 0.4 NEAR out of
the 0.5 that was deposited, leaving 0.1 locked.

**2. unregister**

```bash
rhea_dclv2_storage_unregister_bin_json <dcl_id>
```

this returns the locked 0.1 NEAR and the account's storage on the DEX
goes back to `None`.

---

### how do I check a transaction actually worked?

every write bin prints `✅ ... complete` **even when the on-chain call
failed** — they print the transaction hash without checking the receipt
outcome. always confirm the outcome yourself:

```sh
curl -s -X POST https://test.rpc.fastnear.com \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"EXPERIMENTAL_tx_status","params":["<tx_hash>","<signer_account_id>"]}'
```

look at `result.status` — a `Failure` there means the call did not go
through, whatever the bin printed. the second parameter, the signer
account id, is required.

---

#### NOT YET VERIFIED

these flows are documented by the contract but have **not** been run
end to end yet. verify before using them with value, then move them up
into a recipe above.

| flow                                | how                                              |
| ----------------------------------- | ------------------------------------------------ |
| `LimitOrderWithSwap`                | `rhea_dclv2_limit_order_bin_json ... true`        |
| `SwapByOutput` (exact-output swap)  | `ft_transfer_call` on the token with that msg     |
| `SwapByStopPoint`                   | `ft_transfer_call` on the token with that msg     |
| `HotZap`                            | `ft_transfer_call` on the token with that msg     |
| `predict_hotzap`                    | `ft_transfer_call` on the token with that msg     |
| `mint_v_liquidity` / `burn_v_liquidity` | the v liquidity pair                          |
| `append_liquidity` / `merge_liquidity` | extending an existing position                |
| `batch_*` liquidity methods         | the batched liquidity calls                      |
| `create_pool`                       | creating a pool                                  |
| `mft_*` and `nft_*` methods         | the multi ft and nft interface                   |

==========================
<br/>
copyright 2026 by sleet.near
