---
name: sleet-kit-ft
description: Documents the NEP-141 / NEP-145 fungible token standard - the method names, call types (VIEW or CALL), and exact argument shapes shared by every FT contract (wNEAR, USDC, USDT, custom tokens). Use this skill whenever the user mentions an FT, fungible token, ft_transfer, ft_balance_of, ft_metadata, storage_deposit on a token contract, or asks what arguments a token transfer takes - including when they want to call or inspect a token contract and need the method list, argument names, or the standard NEP-141 shape. This is the standard itself, not one deployed contract, so there is no single contract id.
---

# NEP-141 / NEP-145 Fungible Token (FT)

Generic helpers for any NEP-141 / NEP-145 fungible-token contract
(wNEAR, USDC, USDT, custom tokens, etc.).

Not all FT contracts implement every method.

---

CONTRACT ID PER NETWORK

- depends on the token — pass the token's own account id
  (e.g. wrap.near, usdt.tether-token.near)

### ft_metadata - VIEW

- no args necessary

returns `spec`, `name`, `symbol`, `icon?`, `reference?`,
`reference_hash?`, `decimals`

### ft_total_supply - VIEW

- no args necessary

### ft_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### storage_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

returns `{ total, available }` — both yoctoNEAR decimal strings

### storage_balance_bounds - VIEW

- no args necessary

### storage_deposit - CALL

- no args necessary — the standard storage deposit is 0.00125 NEAR

### storage_withdraw - CALL

- no args necessary

### storage_unregister - CALL

- no args necessary

### ft_transfer - CALL

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000"
}
```

requires exactly 1 yoctoNEAR attached as deposit

### ft_transfer_call - CALL

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000",
  "msg": ""
}
```

requires exactly 1 yoctoNEAR attached as deposit

### ft_resolve_transfer - CALL

- private / callback method — not documented yet

### new - CALL

- private initializer — not documented yet

==========================
<br/>
copyright 2026 by sleet.near