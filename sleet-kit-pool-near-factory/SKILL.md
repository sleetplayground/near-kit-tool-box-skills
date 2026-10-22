---
name: sleet-kit-pool-near-factory
description: Documents the pool.near staking-pool factory contract - the mainnet contract id that deploys validator staking-pool child contracts, and each method's call type (VIEW or CALL) and exact argument shape. Use this skill whenever the user mentions pool.near as a factory, create_staking_pool, the staking pool factory, is_contract_allowed, or get_number_of_staking_pools_created - including when they want to call or inspect the factory and need the method list, argument names, or contract id. For the per-validator staking pool child contracts themselves (deposit_and_stake, get_pool_summary, claim), use the staking skill instead.
---

# pool.near (staking pool factory)

the `pool.near` staking-pool factory contract — deploys v2 validator
staking-pool child contracts (mainnet only).

---

CONTRACT ID PER NETWORK

- MAINNET: pool.near
- TESTNET: (none — mainnet only)

### get_min_attached_balance - VIEW

- no args necessary

### get_number_of_staking_pools_created - VIEW

- no args necessary

### is_contract_allowed - VIEW

```json
{
  "contract_id": "sleet.pool.near"
}
```

### create_staking_pool - CALL

- arg format not documented yet

### allow_contract - CALL

- arg format not documented yet

### disallow_contract - CALL

- arg format not documented yet

### get_code - VIEW

- arg format not documented yet

### on_staking_pool_create - CALL

- private callback — not documented yet

### store - VIEW

- arg format not documented yet

### new - CALL

- private initializer — not documented yet

==========================
<br/>
copyright 2026 by sleet.near