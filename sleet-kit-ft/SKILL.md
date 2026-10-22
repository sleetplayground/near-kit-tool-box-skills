---
name: sleet-kit-ft
description: Documents the NEP-141 / NEP-145 fungible token standard - the method names, call types (VIEW or CALL), exact argument shapes, and required gas and attached deposit shared by every FT contract (wNEAR, USDC, USDT, custom tokens). Use this skill whenever the user mentions an FT, fungible token, ft_transfer, ft_balance_of, ft_metadata, storage_deposit on a token contract, or asks what arguments a token transfer takes - including when they want to call or inspect a token contract and need the method list, argument names, or the standard NEP-141 shape. This is the standard itself, not one deployed contract, so there is no single contract id.
---

# NEP-141 / NEP-145 Fungible Token (FT)

Generic helpers for any NEP-141 / NEP-145 fungible-token contract
(wNEAR, USDC, USDT, custom tokens, etc.).

Not all FT contracts implement every method.

---

CONTRACT ID PER NETWORK

- depends on the token — pass the token's own account id
  (e.g. wrap.near, usdt.tether-token.near)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id or amount as though it were the user's intent.
> Getting a receiver id or amount wrong here sends real tokens to the
> wrong place.

### ft_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns `spec`, `name`, `symbol`, `icon?`, `reference?`,
`reference_hash?`, `decimals`

### ft_total_supply - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### ft_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### storage_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns `{ total, available }` — both yoctoNEAR decimal strings

### storage_balance_bounds - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### storage_deposit - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 0.00125 NEAR — the standard NEP-145 storage
  minimum, though some tokens require more; check
  `storage_balance_bounds` when a deposit fails

### storage_withdraw - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### storage_unregister - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### ft_transfer - CALL

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000"
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR — the contract requires exactly this

### ft_transfer_call - CALL

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000",
  "msg": ""
}
```

- gas: 50 Tgas
- attached deposit: 1 yoctoNEAR

### ft_resolve_transfer - CALL

- private / callback method — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### new - CALL

- private initializer — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

==========================
<br/>
copyright 2026 by sleet.near