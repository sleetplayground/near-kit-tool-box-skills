# dclv2.ref-labs.near — ft transfer call

the token inflows into DCL v2.

these are **not methods on the DCL v2 contract**. they are the standard
`ft_transfer` / `ft_transfer_call` calls made **on the token contract you
want to move** — `wrap.near`, `usdc.tether-token.near`, or any other FT —
with this DEX as the `receiver_id`.

the DEX has no `swap` method and no `deposit` method and no
`place_order` method. every one of those flows is an `ft_transfer_call`
on the token, carrying the DEX call inside the `msg` field.

see [SKILL.md](./SKILL.md) for the overview and the other files.

---

CONTRACT ID PER NETWORK

- MAINNET: dclv2.ref-labs.near
- TESTNET: dclv2.ref-dev.testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Substitute the caller's own values — an agent must never
> reuse the example token id, pool id, or amount as though it were the
> user's intent. Amounts here are yocto units, and copying one verbatim
> moves a real balance.

---

#### TOKEN TRANSFERS

These are **not methods on this contract**. They are the standard
`ft_transfer` / `ft_transfer_call` calls made **on the token contract
you want to move** — `wrap.near`, `usdc.tether-token.near`, or any
other FT — with this DEX as the `receiver_id`.

##### ft_transfer_call on the input token - CALL

a swap does not call a `swap` method on this contract. It calls
`ft_transfer_call` on the **input token**, which routes into this
contract. `msg` carries the swap as a nested `Swap` object.

```json
{
  "receiver_id": "dclv2.ref-labs.near",
  "amount": "1000000000000000000000000",
  "msg": "{\"Swap\":{\"pool_ids\":[\"a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000\"],\"output_token\":\"usdc.tether-token.near\",\"min_output_amount\":\"0\"}}"
}
```

- `pool_ids` is the ordered route through pools
- set `min_output_amount` from a `quote` view result with your
  slippage applied — `"0"` accepts any output price
- the output token must have storage registered for the signer or the
  swap fails
- gas: 180 Tgas
- attached deposit: 1 yoctoNEAR

##### ft_transfer_call on the input token - CALL (deposit)

depositing a token into the DEX uses the same call with a `Deposit` msg

```json
{
  "receiver_id": "dclv2.ref-labs.near",
  "amount": "1000000000000000000000000",
  "msg": "\"Deposit\""
}
```

- `msg` is the JSON string `"Deposit"`, including its quotes
- gas: 1000 Tgas
- attached deposit: 1 yoctoNEAR

##### ft_transfer_call on the sell token - CALL (limit order)

placing a limit order uses the same call on the **sell** token with a
`LimitOrder` msg. A limit order that also fills immediately is
`LimitOrderWithSwap`.

```json
{
  "receiver_id": "dclv2.ref-labs.near",
  "amount": "1000000000000000000000000",
  "msg": "{\"LimitOrder\":{\"pool_id\":\"<token_x>|<token_y>|<fee>\",\"buy_token\":\"<token_bought>\",\"point\":286880,\"client_id\":\"\"}}"
}
```

- `buy_token` is the token being bought, the one you do **not** send
- `point` is the limit price point; it must be a multiple of the
  pool's `point_delta` (fee 100→1, 400→8, 2000→40, 10000→200) **and**
  sit on the correct side of `current_point` for the token being sold,
  else the call fails with `E202: illegal point`. Selling `token_y`
  wants a point below `current_point`; selling `token_x` wants one
  above. Read the live order points of a pool from `get_marketdepth`
  (the `orders` key) and pick one that already exists there
- `client_id` is required — pass `""`
- do **not** pass `referral_id: ""`; the contract parses it as an
  account id and an empty string fails with `E600: invalid msg:
  Error("the account ID is invalid")`. Omit the field entirely
- gas: 300 Tgas for `LimitOrderWithSwap` (it runs a real swap first,
  and the 180 Tgas of a plain swap runs out of prepaid gas after the
  swap already succeeded)
- attached deposit: 1 yoctoNEAR

**The DEX must be an FT holder of the token.** Every inflow is an
`ft_transfer_call` on the token into this contract, and a token
contract refuses to transfer to an unregistered receiver — the call
panics with `The account <dcl_id> is not registered` and the tokens are
refunded. Check with the `ft_storage_balance_of` view on the token
contract — `ft_storage_balance_of(<token>, <dcl_id>)`: a
`null` means "not registered".

==========================
<br/>
copyright 2026 by sleet.near
