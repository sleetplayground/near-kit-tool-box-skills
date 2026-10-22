---
name: sleet-kit-hos-lockup
description: Documents the house-of-stake per-user lockup contract - how to resolve a user's lockup sub-account, and each method's call type (VIEW or CALL). Use this skill whenever the user mentions a venear lockup, a lock.near account, lock_near, begin_unlock_near, deposit_to_staking_pool, or a user's locked NEAR - including when they want to call or inspect a lockup account and need the method list or which account id to use. For the venear.dao contract that deploys these lockups, use the hos-venear-dao skill instead.
---

# house-of-stake lockup

the house-of-stake per-user lockup contract

---

CONTRACT ID PER NETWORK

- MAINNET: per-user — deployed by `venear.dao` as e.g. `<user>.lockup.near`
- TESTNET: (none)

There is no global lockup account id. Each user interacts with their
own lockup sub-account. Resolve the exact id with `get_lockup_account_id`
on `venear.dao` before calling anything here.

> #### ARG AND GAS
>
> No argument format, gas, or attached deposit has been confirmed for
> this contract. Every method below is listed as callable or read-only
> by name only — treat the list as a starting point, not a
> specification. Get the authoritative signatures before using any of
> them:
>
> ```sh
> near contract inspect <lockup_account> network-config mainnet now
> ```

### get_owner_account_id - VIEW

- no args necessary

### get_staking_pool_account_id - VIEW

- no args necessary

### get_balance - VIEW

- no args necessary

### get_owners_balance - VIEW

- no args necessary

### get_known_deposited_balance - VIEW

- no args necessary

### get_liquid_owners_balance - VIEW

- no args necessary

### get_venear_liquid_balance - VIEW

- no args necessary

### get_venear_locked_balance - VIEW

- no args necessary

### get_venear_pending_balance - VIEW

- no args necessary

### get_venear_unlock_timestamp - VIEW

- no args necessary

### get_lockup_update_nonce - VIEW

- no args necessary

### get_version - VIEW

- no args necessary

### contract_source_metadata - VIEW

- no args necessary

### lock_near - CALL

- arg format not documented yet

### lock_pending_near - CALL

- arg format not documented yet

### begin_unlock_near - CALL

- arg format not documented yet

### end_unlock_near - CALL

- arg format not documented yet

### transfer - CALL

- arg format not documented yet

### delete_lockup - CALL

- arg format not documented yet

### select_staking_pool - CALL

- arg format not documented yet

### unselect_staking_pool - CALL

- arg format not documented yet

### deposit_and_stake - CALL

- no args necessary — the amount is the attached deposit

### deposit_to_staking_pool - CALL

- arg format not documented yet

### stake - CALL

- arg format not documented yet

### unstake - CALL

- arg format not documented yet

### unstake_all - CALL

- no args necessary

### withdraw_from_staking_pool - CALL

- arg format not documented yet

### withdraw_all_from_staking_pool - CALL

- no args necessary

### refresh_staking_pool_balance - CALL

- arg format not documented yet

==========================
<br/>
copyright 2026 by sleet.near