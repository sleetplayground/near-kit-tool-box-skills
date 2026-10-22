---
name: sleet-kit-pool-near-staking
description: Documents the NEAR validator staking-pool child contracts - the per-validator pool account ids such as everything.pool.near, and each method's call type (VIEW or CALL) and exact argument shape. Use this skill whenever the user mentions a staking pool, deposit_and_stake, get_pool_summary, staked or unstaked balances on a pool, or a pool account id like everything.pool.near - including when they want to call or inspect a staking pool and need the method list, argument names, or which pool id to use. For the pool.near factory contract that deploys these pools, use the factory skill instead.
---

# staking pools

The standard NEAR validator staking-pool child contracts. There is no
single canonical id — staking pools are deployed per-validator, and
the fun helpers take the concrete pool account id as a parameter.

On mainnet the validator's pool account id is typically
`<validator>.pool.near` (deployed by the v2 factory) or
`<validator>.poolv1.near` (deployed by the legacy v1 factory).

---

CONTRACT ID PER NETWORK

- MAINNET: per-validator — `<validator>.pool.near` or `<validator>.poolv1.near`
- TESTNET: (none)

KNOWN POOLS (v2 hash)

- everything.pool.near
- shitzu.pool.near
- shadowcorp.pool.near
- intear.pool.near

> Aurorapool (`<validator>.pool.near` for aurora) runs a **patched**
> v2 pool. Method surface matches but **always verify code_hash at the
> call site** before assuming parity.

### get_number_of_accounts - VIEW

- no args necessary

### get_accounts - VIEW

```json
{
  "from_index": 0,
  "limit": 100
}
```

### ping - CALL

```json
{
  "gas": "30 Tgas"
}
```

### deposit_and_stake - CALL

- no args necessary — the amount is the attached deposit

### deposit - CALL

- no args necessary — the amount is the attached deposit

### withdraw - CALL

- arg format not documented yet

### withdraw_all - CALL

- no args necessary

### unstake - CALL

- arg format not documented yet

### unstake_all - CALL

- no args necessary

### stake - CALL

- arg format not documented yet

### stake_all - CALL

- no args necessary

### burn - CALL

- arg format not documented yet

### unstake_burn - CALL

- arg format not documented yet

### claim - CALL

- arg format not documented yet

### get_account_staked_balance - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### get_account_unstaked_balance - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### get_account_total_balance - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### is_account_unstaked_balance_available - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### get_account - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### get_total_staked_balance - VIEW

- no args necessary

### get_pool_summary - VIEW

- no args necessary

### get_unclaimed_reward - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### get_owner_id - VIEW

- no args necessary

### get_factory_id - VIEW

- no args necessary

### get_version - VIEW

- no args necessary

### get_staking_key - VIEW

- no args necessary

### get_reward_fee_fraction - VIEW

- no args necessary

### is_staking_paused - VIEW

- no args necessary

### get_authorized_users - VIEW

- no args necessary

### get_authorized_farm_tokens - VIEW

- no args necessary

### get_active_farms - VIEW

- no args necessary

### get_farms - VIEW

- no args necessary

### get_farm - VIEW

- arg format not documented yet

### add_authorized_user - CALL

- arg format not documented yet

### remove_authorized_user - CALL

- arg format not documented yet

### add_authorized_farm_token - CALL

- arg format not documented yet

### remove_authorized_farm_token - CALL

- arg format not documented yet

### set_owner_id - CALL

- arg format not documented yet

### update_staking_key - CALL

- arg format not documented yet

### update_reward_fee_fraction - CALL

- arg format not documented yet

### decrease_burn_fee_fraction - CALL

- arg format not documented yet

### pause_staking - CALL

- arg format not documented yet

### resume_staking - CALL

- arg format not documented yet

### stop_farm - CALL

- arg format not documented yet

### update - CALL

- arg format not documented yet

### migrate - CALL

- arg format not documented yet

### upgrade - CALL

- arg format not documented yet

### on_stake_action - CALL

- private callback — not documented yet

### callback_post_get_owner - CALL

- private callback — not documented yet

### callback_post_withdraw_reward - CALL

- private callback — not documented yet

### ft_on_transfer - CALL

- NEP-141 callback — not documented yet

### new - CALL

- private initializer — not documented yet

==========================
<br/>
copyright 2026 by sleet.near