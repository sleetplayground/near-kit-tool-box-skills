---
name: sleet-kit-wrap-near
description: Documents the wrap.near wNEAR contract - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions wrap.near, wrap.testnet, wNEAR, wrapping or unwrapping NEAR, near_deposit, or near_withdraw, or asks what arguments a wrapping contract method takes - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---


# wrap.near

the wrap.near wNEAR (NEP-141 wrapped NEAR) smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: wrap.near
- TESTNET: wrap.testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id or amount as though it were the user's intent.

### storage_minimum_balance - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### near_deposit - CALL

- no args necessary — the amount is the attached deposit
- gas: not documented yet
- attached deposit: the amount of NEAR to wrap, e.g. `1 NEAR`

### near_withdraw - CALL

```json
{
  "amount": "1000000000000000000000000"
}
```

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
- attached deposit: 1 yoctoNEAR

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

### storage_deposit - CALL

- no args necessary
- gas: not documented yet
- attached deposit: the NEP-145 storage minimum — view
  `storage_minimum_balance` to read the current value

### storage_withdraw - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### storage_balance_bounds - VIEW

- no args necessary
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

### ft_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near