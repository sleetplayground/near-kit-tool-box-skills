---
name: sleet-kit-hos-venear-dao
description: Documents the venear.dao house-of-stake governance token contract - the mainnet contract id, and each method's call type (VIEW or CALL). Use this skill whenever the user mentions venear, venear.dao, the house-of-stake governance token, deploy_lockup, or get_lockup_account_id - including when they want to call or inspect this contract and need the method list or contract id. For a user's own per-user lockup sub-account, use the hos-lockup skill instead.
---

# venear.dao

the house-of-stake venear governance token contract

---

CONTRACT ID PER NETWORK

- MAINNET: venear.dao
- TESTNET: (none — mainnet only)

This contract deploys each user's lockup sub-account (e.g.
`<user>.lockup.near`) — see the `hos-lockup` skill for those methods.

> #### ARG AND GAS
>
> No argument format, gas, or attached deposit has been confirmed for
> this contract. Every method below is listed as callable or read-only
> by name only — treat the list as a starting point, not a
> specification. Get the authoritative signatures before using any of
> them:
>
> ```sh
> near contract inspect venear.dao network-config mainnet now
> ```

### get_lockup_account_id - VIEW

- arg format not documented yet

### get_lockup_deployment_cost - VIEW

- arg format not documented yet

### get_config - VIEW

- no args necessary

### get_version - VIEW

- no args necessary

### get_num_accounts - VIEW

- no args necessary

### get_accounts - VIEW

- arg format not documented yet

### get_accounts_raw - VIEW

- arg format not documented yet

### get_account_by_index - VIEW

- arg format not documented yet

### get_account_info - VIEW

- arg format not documented yet

### get_snapshot - VIEW

- arg format not documented yet

### get_proof - VIEW

- arg format not documented yet

### is_paused - VIEW

- no args necessary

### ft_metadata - VIEW

- no args necessary

### ft_total_supply - VIEW

- no args necessary

### ft_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### storage_balance_of - VIEW

- arg format not documented yet

### storage_balance_bounds - VIEW

- no args necessary

### deploy_lockup - CALL

- arg format not documented yet

### set_local_deposit - CALL

- arg format not documented yet

### delegate_all - CALL

- arg format not documented yet

### undelegate - CALL

- arg format not documented yet

### ft_transfer - CALL

- arg format not documented yet

### ft_transfer_call - CALL

- arg format not documented yet

### storage_deposit - CALL

- no args necessary

### storage_withdraw - CALL

- arg format not documented yet

### pause - CALL

- no args necessary

### unpause - CALL

- no args necessary

### accept_ownership - CALL

- arg format not documented yet

### propose_new_owner_account_id - CALL

- arg format not documented yet

### set_guardians - CALL

- arg format not documented yet

### set_lockup_code_deployers - CALL

- arg format not documented yet

### set_lockup_contract - CALL

- arg format not documented yet

### set_staking_pool_whitelist_account_id - CALL

- arg format not documented yet

### set_unlock_duration_sec - CALL

- arg format not documented yet

### migrate_state - CALL

- arg format not documented yet

### contract_source_metadata - VIEW

- no args necessary

==========================
<br/>
copyright 2026 by sleet.near