---
name: sleet-kit-pool-near-factory
description: Documents the pool.near staking-pool factory contract - the mainnet contract id that deploys validator staking-pool child contracts, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions pool.near as a factory, create_staking_pool, the staking pool factory, is_contract_allowed, or get_number_of_staking_pools_created - including when they want to call or inspect the factory and need the method list, argument names, or contract id. For the per-validator staking pool child contracts themselves (deposit_and_stake, get_pool_summary, claim), use the staking skill instead.
---

# pool.near (staking pool factory)

the `pool.near` staking-pool factory contract — deploys v2 validator
staking-pool child contracts (mainnet only).

---

CONTRACT ID PER NETWORK

- MAINNET: pool.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example contract id as though it were the user's own.

### get_min_attached_balance - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_number_of_staking_pools_created - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### is_contract_allowed - VIEW

```json
{
  "contract_id": "sleet.pool.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### create_staking_pool - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet — view
  `get_min_attached_balance` first, the factory requires at least that
  much to cover the child contract's storage

### allow_contract - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### disallow_contract - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### get_code - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### on_staking_pool_create - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### store - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### new - CALL

- private initializer — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

==========================
<br/>
copyright 2026 by sleet.near