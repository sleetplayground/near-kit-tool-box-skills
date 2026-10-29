---
name: sleet-kit-rhea-dclv2-ref-labs-near
description: Documents the dclv2.ref-labs.near concentrated-liquidity DEX contract - the deployed contract id on mainnet and testnet, the three kinds of call it takes (a view, a contract method, or an ft_transfer_call on the token), each method's call type (VIEW or CALL), arguments, and required gas and attached deposit. Use this skill whenever the user mentions dclv2, ref-labs, the concentrated liquidity DEX, a DCL quote, add_liquidity or remove_liquidity by position, a DCL limit order, mint_v_liquidity or burn_v_liquidity, the DCL internal balance - including when they want to call or inspect this contract and need the method list, argument names, pool id and lpt id formats, the gas each flow needs, or the network-specific contract id.
---

# dclv2.ref-labs.near

the ref-finance concentrated-liquidity dex v2 smart contract

this skill is split into files. this page is the overview and the index.

| file                                     | what it holds                                             |
| ---------------------------------------- | --------------------------------------------------------- |
| [views.md](./views.md)                   | the 40 read methods of the contract                        |
| [calls.md](./calls.md)                   | the 45 state-changing methods of the contract              |
| [ft_transfer_call.md](./ft_transfer_call.md) | the token inflows — swaps, deposits, limit orders       |
| [multi.md](./multi.md)                   | the multi ft (`mft_*`) and nft (`nft_*`) interface         |
| [recipes.md](./recipes.md)               | task shaped how-tos with the functions and bins to use     |

---

CONTRACT ID PER NETWORK

- MAINNET: dclv2.ref-labs.near
- TESTNET: dclv2.ref-dev.testnet

> #### ID FORMATS
>
> A `pool_id` is a composite STRING, not a number:
>
> ```
> <token_x_account_id>|<token_y_account_id>|<fee>
> ```
>
> for example
> `a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000`.
> Both segments are the tokens' NEAR account ids; `fee` is in basis points.
>
> An `lpt_id` is the pool id followed by the owner's liquidity index:
>
> ```
> <pool_id>#<index>
> ```
>
> for example
> `nearvember-337.meme-cooking.near|wrap.near|10000#17677`.
>
> `list_pools` and `list_liquidities` are the views that expose real ids.
> The website's own form (`token<>token@fee`) returns `null`.

---

#### THE THREE KINDS OF CALL

the DEX is reached in three different ways, and mixing them up is the
most common mistake:

1. **a view** — a read, free, no signer needed. `quote`, `get_pool`,
   `list_liquidities` and the rest live in [views.md](./views.md).
2. **a contract method** — a call to a method *on the DEX*. `add_liquidity`,
   `remove_liquidity`, `withdraw_asset`, `cancel_order`, `storage_deposit`
   and the rest live in [calls.md](./calls.md).
3. **an `ft_transfer_call` on the token** — a call on the **token contract**,
   with the DEX as `receiver_id` and the DEX call carried inside `msg`.
   swaps, internal-balance deposits and limit orders all work this way, and
   they live in [ft_transfer_call.md](./ft_transfer_call.md).

beware of taxed tokens — some tokens take a cut on transfer, so a taxed
token arrives at the DEX with less than you sent.

the DEX has **no `swap` method, no `deposit` method and no `place_order`
method**. those three flows are all `ft_transfer_call` on the token.

---

#### THE INTERNAL BALANCE

the DEX keeps a per-user **internal balance** of every token it has seen.
`add_liquidity` pulls from it, and `withdraw_asset` pays out of it.

so the LP flow is always three steps, never two:

1. `ft_transfer_call` on the token with `msg = "\"Deposit\""` — credit the
   internal balance
2. `add_liquidity` — a direct contract method, pulling from the internal
   balance
3. `withdraw_asset` — a direct contract method, paying out of it

a swap does **not** touch the internal balance. it goes straight in and
straight out to the wallet.

---

#### GAS AND DEPOSITS

- every **view** is free and takes no deposit
- a **contract method** takes no deposit, except the storage methods:
  `storage_deposit` needs 0.5 NEAR, and `storage_withdraw` /
  `storage_unregister` attach exactly 1 yoctoNEAR
- an **`ft_transfer_call`** attaches exactly 1 yoctoNEAR

the `ft_transfer_call` gas is the one to watch:

| flow                     | gas    |
| ------------------------ | ------ |
| deposit                  | 1000   |
| swap                     | 180    |
| limit order with a swap  | 300    |

a limit order that swaps first needs more than the gas of a plain swap —
180 Tgas runs out of prepaid gas *after* the swap already succeeded, so
the swap goes through and the order placement fails.

---

#### THE DEX MUST HOLD THE TOKEN

every token inflow is an `ft_transfer_call` on the token into the DEX, and
a token contract refuses to transfer to a receiver it does not know. if
the DEX is not registered on that token the call panics with
`The account <dcl_id> is not registered` and the tokens are refunded.

check first with the `ft_storage_balance_of` view on the token contract —
`ft_storage_balance_of(<token>, <dcl_id>)` — a `null` means "not
registered", and that token cannot be deposited, swapped or traded through
the DEX.

---

#### POINTS

a pool's `point_delta` comes from its fee: 100→1, 400→8, 2000→40,
10000→200.

- an **LP range** (`left_point`, `right_point`) must sit on multiples of
  `point_delta`, else `E200: invalid endpoint`
- a **limit order** `point` must also sit on the correct side of
  `current_point` for the token being sold, else `E202: illegal point`.
  selling `token_y` wants a point below `current_point`, selling `token_x`
  wants one above

`current_point` is generally not a multiple of `point_delta`, so round to
it. read the live order points of a pool from `get_marketdepth` (the
`orders` key) and pick one that already exists there.

---

#### CAN A LIMITED ACCESS KEY CALL THIS CONTRACT?

yes — a function-call key with `receiver_id` set to the DEX and an empty
method list can call **every method of the DEX directly**: `add_liquidity`,
`remove_liquidity`, `withdraw_asset`, `cancel_order`, the storage methods,
and the rest.

it cannot do the **token inflows** — `deposit`, `swap` and limit orders are
`ft_transfer_call` on the **token** contract, and a function-call key is
pinned to one receiver and cannot attach a deposit. those fail with
`ReceiverMismatch` and `DepositWithFunctionCall`.

a **method list** on the key narrows it further — a key restricted to
`[quote, get_pool]` fails on any other method with `MethodNameMismatch`.

the allowance is the limit on what a key can spend, so a key that has to
attach a deposit needs an allowance above it. `storage_deposit` needs
0.5 NEAR, and the allowance also has to cover the gas.

adding a key with the same public key but a different method list does
**not** replace the existing key — the account keeps the old permission. use
a fresh key pair, or delete the old key first.

---

#### WHERE THE FUNCTIONS AND BINS LIVE

in the `near-kit-tool-box-rs` crate:

| flow                | function                                                             | bin                                          |
| ------------------- | -------------------------------------------------------------------- | -------------------------------------------- |
| quote               | `fun::rhea_dclv2::dclv2_quote_fun_json::quote`                        | `rhea_dclv2_quote_bin_json`                    |
| quote by output     | `fun::rhea_dclv2::dclv2_quote_by_output_fun_json::quote_by_output`    | `rhea_dclv2_quote_by_output_bin_json`          |
| get pool            | `fun::rhea_dclv2::dclv2_get_pool_fun_json::get_pool`                  | `rhea_dclv2_get_pool_bin_json`                 |
| get market depth    | `fun::rhea_dclv2::dclv2_get_marketdepth_fun_json::get_marketdepth`    | `rhea_dclv2_get_marketdepth_bin_json`          |
| list liquidities    | `fun::rhea_dclv2::dclv2_list_liquidities_fun_json::list_liquidities`  | `rhea_dclv2_list_liquidities_bin_json`         |
| list user assets    | `fun::rhea_dclv2::dclv2_list_user_assets_fun_json::list_user_assets`  | `rhea_dclv2_list_user_assets_bin_json`         |
| add liquidity       | `fun::rhea_dclv2::dclv2_add_liquidity_fun_json::add_liquidity`        | `rhea_dclv2_add_liquidity_bin_json`            |
| remove liquidity    | `fun::rhea_dclv2::dclv2_remove_liquidity_fun_json::remove_liquidity`  | `rhea_dclv2_remove_liquidity_bin_json`         |
| predict remove      | `fun::rhea_dclv2::dclv2_predict_remove_liquidity_fun_json::predict_remove_liquidity` | `rhea_dclv2_predict_remove_liquidity_bin_json` |
| withdraw            | `fun::rhea_dclv2::dclv2_withdraw_asset_fun_json::withdraw_asset`      | `rhea_dclv2_withdraw_asset_bin_json`           |
| cancel order        | `fun::rhea_dclv2::dclv2_cancel_order_fun_json::cancel_order`          | `rhea_dclv2_cancel_order_bin_json`             |
| storage deposit     | `fun::rhea_dclv2::dclv2_storage_deposit_fun_json::storage_deposit`    | `rhea_dclv2_storage_deposit_bin_json`          |
| storage withdraw    | `fun::rhea_dclv2::dclv2_storage_withdraw_fun_json::storage_withdraw`  | `rhea_dclv2_storage_withdraw_bin_json`         |
| storage unregister  | `fun::rhea_dclv2::dclv2_storage_unregister_fun_json::storage_unregister` | `rhea_dclv2_storage_unregister_bin_json`    |
| deposit             | `fun::rhea_dclv2::ft_transfer_call::dclv2_deposit_fun_json::deposit`  | `rhea_dclv2_deposit_bin_json`                  |
| swap                | `fun::rhea_dclv2::ft_transfer_call::dclv2_swap_ft_transfer_call_fun_json::swap` | `rhea_dclv2_swap_bin_json`          |
| limit order         | `fun::rhea_dclv2::ft_transfer_call::dclv2_limit_order_fun_json::limit_order` | `rhea_dclv2_limit_order_bin_json`       |

---

#### READY-MADE RECIPES

[recipes.md](./recipes.md) has the task shaped how-tos — add liquidity,
swap, place a limit order, unwind a position — each one naming the exact
functions and bins in order.

==========================
<br/>
copyright 2026 by sleet.near
