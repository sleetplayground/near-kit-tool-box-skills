---
name: sleet-kit-wrap-near
description: Documents the wrap.near wNEAR contract - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL) and exact argument shape. Use this skill whenever the user mentions wrap.near, wrap.testnet, wNEAR, wrapping or unwrapping NEAR, near_deposit, or near_withdraw, or asks what arguments a wrapping contract method takes - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---

# wrap.near

the wrap.near wNEAR (NEP-141 wrapped NEAR) smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: wrap.near
- TESTNET: wrap.testnet

### storage_minimum_balance - VIEW

- no args necessary

### near_deposit - CALL

- no args necessary — the amount is the attached deposit

### near_withdraw - CALL

```json
{
  "amount": "1000000000000000000000000"
}
```

### ft_transfer - CALL

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000"
}
```

### ft_transfer_call - CALL

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000",
  "msg": ""
}
```

### ft_total_supply - VIEW

- no args necessary

### ft_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### ft_resolve_transfer - CALL

- private / callback method — not documented yet

### storage_deposit - CALL

- no args necessary

### storage_withdraw - CALL

- no args necessary

### storage_unregister - CALL

- no args necessary

### storage_balance_bounds - VIEW

- no args necessary

### storage_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### ft_metadata - VIEW

- no args necessary

### new - CALL

- private initializer — not documented yet

==========================
<br/>
copyright 2026 by sleet.near