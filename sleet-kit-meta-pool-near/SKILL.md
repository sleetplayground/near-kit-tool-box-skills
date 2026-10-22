---
name: sleet-kit-meta-pool-near
description: Documents the meta-pool.near liquid-staking contract - the mainnet contract id, and each method's call type (VIEW or CALL). Use this skill whenever the user mentions meta-pool, stNEAR, staking pool lists, distribute_rewards, or nslp liquidity on meta-pool - including when they want to call or inspect this contract and need the method list or contract id.
---

# meta-pool.near

the meta-pool liquid-staking smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: meta-pool.near
- TESTNET: (none — mainnet only)

> #### ARG AND GAS
>
> No fun helper exists for this contract yet, so no argument format,
> gas, or attached deposit has been verified. Every method below is
> listed as callable or read-only by name only. Get the authoritative
> signatures before using any of them:
>
> ```sh
near contract inspect meta-pool.near network-config mainnet now
> ```


### VIEWS

### get_staking_pool_list - VIEW

- arg format not documented yet

### get_operator_account_id - VIEW

- arg format not documented yet

### get_treasury_account_id - VIEW

- arg format not documented yet

### get_known_deposited_balance - VIEW

- arg format not documented yet

### get_account_info - VIEW

- arg format not documented yet

### get_contract_info - VIEW

- arg format not documented yet

### get_st_near_price - VIEW

- arg format not documented yet

### get_contract_state - VIEW

- arg format not documented yet

### get_contract_params - VIEW

- arg format not documented yet

### get_sp_info - VIEW

- arg format not documented yet

### get_staking_pool_requiring_unstake - VIEW

- arg format not documented yet

### storage_balance_bounds - VIEW

- arg format not documented yet

### storage_balance_of - VIEW

- arg format not documented yet

### ft_total_supply - VIEW

- arg format not documented yet

### ft_balance_of - VIEW

- arg format not documented yet

### ft_metadata - VIEW

- arg format not documented yet

### get_account_unstaked_balance - VIEW

- arg format not documented yet

### get_account_staked_balance - VIEW

- arg format not documented yet

### get_account_total_balance - VIEW

- arg format not documented yet

### get_account_available_balance - VIEW

- arg format not documented yet

### is_account_unstaked_balance_available - VIEW

- arg format not documented yet

### get_reward_fee_fraction - VIEW

- arg format not documented yet

### get_owner_id - VIEW

- arg format not documented yet

### get_staking_key - VIEW

- arg format not documented yet

### is_staking_paused - VIEW

- arg format not documented yet

### get_account - VIEW

- arg format not documented yet

### get_number_of_accounts - VIEW

- arg format not documented yet

### get_accounts - VIEW

- arg format not documented yet

### get_accounts_info - VIEW

- arg format not documented yet

### get_near_amount_sell_stnear - VIEW

- arg format not documented yet

### nslp_get_discount_basis_points - VIEW

- arg format not documented yet

### ping - VIEW

- arg format not documented yet


### CALLS

### distribute_staking - CALL

- arg format not documented yet

### manual_stake - CALL

- arg format not documented yet

### force_rebalance_unstake - CALL

- arg format not documented yet

### rebalance_unstake_sp - CALL

- arg format not documented yet

### do_rebalance_unstake - CALL

- arg format not documented yet

### distribute_unstaking - CALL

- arg format not documented yet

### set_busy - CALL

- arg format not documented yet

### sp_busy - CALL

- arg format not documented yet

### extra_balance_accumulated - CALL

- arg format not documented yet

### transfer_extra_balance_accumulated - CALL

- arg format not documented yet

### sync_unstaked_balance - CALL

- arg format not documented yet

### distribute_rewards - CALL

- arg format not documented yet

### retrieve_funds_from_a_pool - CALL

- arg format not documented yet

### end_of_epoch_clearing - CALL

- arg format not documented yet

### migrate - CALL

- arg format not documented yet

### pause_staking - CALL

- arg format not documented yet

### un_pause_staking - CALL

- arg format not documented yet

### remove_staking_pool - CALL

- arg format not documented yet

### add_staking_pool - CALL

- arg format not documented yet

### set_staking_pools - CALL

- arg format not documented yet

### compute_current_unstaking_delay - CALL

- arg format not documented yet

### set_operator_account_id - CALL

- arg format not documented yet

### set_treasury_account_id - CALL

- arg format not documented yet

### set_owner_id - CALL

- arg format not documented yet

### set_contract_info - CALL

- arg format not documented yet

### set_contract_params - CALL

- arg format not documented yet

### set_reward_multipliers - CALL

- arg format not documented yet

### get_staking_pool_requiring_retrieve - CALL

- arg format not documented yet

### storage_deposit - CALL

- arg format not documented yet

### storage_withdraw - CALL

- arg format not documented yet

### storage_unregister - CALL

- arg format not documented yet

### ft_transfer - CALL

- arg format not documented yet

### ft_transfer_call - CALL

- arg format not documented yet

### set_max_meta_rewards - CALL

- arg format not documented yet

### ft_metadata_set - CALL

- arg format not documented yet

### deposit - CALL

- no args necessary — the amount is the attached deposit

### withdraw - CALL

- arg format not documented yet

### withdraw_all - CALL

- arg format not documented yet

### withdraw_unstaked - CALL

- arg format not documented yet

### deposit_and_stake - CALL

- arg format not documented yet

### stake_all - CALL

- arg format not documented yet

### stake - CALL

- arg format not documented yet

### unstake_all - CALL

- arg format not documented yet

### unstake - CALL

- arg format not documented yet

### stake_for_lockup - CALL

- arg format not documented yet

### unstake_from_lockup_shares - CALL

- arg format not documented yet

### withdraw_to_lockup - CALL

- arg format not documented yet

### get_reward_fee_bp - CALL

- arg format not documented yet

### set_reward_fee - CALL

- arg format not documented yet

### liquid_unstake - CALL

- arg format not documented yet

### nslp_add_liquidity - CALL

- arg format not documented yet

### nslp_remove_liquidity - CALL

- arg format not documented yet

### stake_from_nslp - CALL

- arg format not documented yet

### realize_meta - CALL

- arg format not documented yet

### harvest_meta - CALL

- arg format not documented yet

### new - CALL

- arg format not documented yet


==========================
<br/>
copyright 2026 by sleet.near