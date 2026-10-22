---
name: sleet-kit-v2-ref-finance-near
description: Documents the v2.ref-finance.near AMM DEX contract - the deployed contract id on mainnet and testnet, the core methods you will actually call (swap, add_liquidity, remove_liquidity, get_pool, get_pools, get_return, withdraw, storage_deposit) with their exact argument shapes and required gas and attached deposit, and a pointer to the full 162-method surface for everything else. Use this skill whenever the user mentions ref-finance, rhea, the AMM or DEX, a swap, adding or removing liquidity, get_pool, get_return, or v2.ref-finance.near - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---

# v2.ref-finance.near

the rhea / ref-finance v2 amm dex smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: v2.ref-finance.near
- TESTNET: ref-finance-101.testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example token id, pool id, or amount as though it were the user's
> intent. A swap built from these numbers verbatim would trade someone
> else's chosen amounts on someone else's pool.

### swap - CALL

`actions` is an array of swap actions, each with a pool id, an input
token, an optional input amount, an output token, and a minimum
acceptable output.

```json
{
  "actions": [
    {
      "pool_id": 1,
      "token_in": "wrap.near",
      "amount_in": "1000000000000000000000000",
      "token_out": "usdc.tether-token.near",
      "min_amount_out": "0"
    }
  ],
  "referral_id": "0"
}
```

- gas: not documented yet
- attached deposit: 0

> `min_amount_out` of `"0"` in the example is a placeholder that
> accepts any output price. Always set it to a real slippage bound
> before using this for value.

### add_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### add_simple_pool - CALL

```json
{
  "fee": 30,
  "tokens": ["wrap.near", "usdc.tether-token.near"]
}
```

- gas: not documented yet
- attached deposit: 0

### remove_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### withdraw - CALL

```json
{
  "amount": "1000000000000000000000000",
  "token_id": "wrap.near",
  "unregister": false,
  "skip_unwrap_near": false
}
```

- gas: not documented yet
- attached deposit: 0

### get_pool - VIEW

```json
{
  "pool_id": 1
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns `pool_kind`, `token_account_ids`, `amounts`, `total_fee`,
`shares_total_supply`, `amp`

### get_pools - VIEW

```json
{
  "from_index": 0,
  "limit": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_number_of_pools - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_return - VIEW

```json
{
  "pool_id": 1,
  "token_in": "wrap.near",
  "amount_in": "1000000000000000000000000",
  "token_out": "usdc.tether-token.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_deposits - VIEW

- this reads the **internal token balances the contract holds for an
  account** — it is not that account's LP position, staked balance, or
  share count. For LP positions use `get_deposit` (single pool),
  `get_pool_total_shares`, `get_pool_shares`, or
  `get_pool_share_price` — those appear in the method surface below and
  their arg formats are not documented yet.

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns a `{ token_id: amount }` map

### storage_deposit - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 0.00125 NEAR — the standard NEP-145 storage
  minimum, though some pools require more; check
  `storage_balance_bounds` when a deposit fails

### storage_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### predict_swap_actions - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### batch_views - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### execute_actions - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### swap_by_output - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 0

### execute_actions_in_va - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

---

#### FULL METHOD SURFACE

This contract exposes **162 methods** as reported by
`near contract inspect`. Only the methods documented above have a
verified argument format in the tool box.

For any method not listed above, the arg format is **not documented
yet**. To get the authoritative list and signatures for your network:

```sh
near contract inspect v2.ref-finance.near network-config mainnet now
near contract inspect ref-finance-101.testnet network-config testnet now
```

Every method in the surface below is a view unless listed as a call
elsewhere in this document; unlisted methods default to the client's
gas and a 0 deposit unless the contract requires otherwise.

The method surface groups into these areas, all present unless noted
otherwise:

- **core swap / liquidity** — `swap`, `swap_by_output`, `add_liquidity`,
  `add_stable_liquidity`, `remove_liquidity`, `remove_liquidity_by_tokens`,
  `execute_actions`, `execute_actions_in_va`
- **pool reads** — `get_pool`, `get_pools`, `get_pool_by_ids`,
  `get_pool_detail_info`, `get_pool_detail_infos`,
  `get_pool_detail_info_by_ids`, `get_pool_fee`, `get_pool_volumes`,
  `get_pool_volumes_by_ids`, `list_pool_volumes`, `get_pool_share_price`,
  `get_pool_shares`, `get_pool_shares_batch`, `get_pool_total_shares`,
  `get_stable_pool`, `get_rated_pool`, `get_degen_pool`,
  `get_number_of_pools`, `get_unit_lpt_assets`
- **account reads** — `get_deposits`, `get_deposit`, `get_account_basic_info`,
  `get_user_storage_state`, `get_user_whitelisted_tokens`
- **prediction / return quoting** — `predict_add_simple_liquidity`,
  `predict_add_stable_liquidity`, `predict_remove_liquidity`,
  `predict_remove_liquidity_by_tokens`, `predict_add_rated_liquidity`,
  `predict_add_degen_liquidity`, `predict_remove_rated_liquidity_by_tokens`,
  `predict_remove_degen_liquidity_by_tokens`, `predict_swap_actions`,
  `batch_predict_swap_actions`, `predict_hot_zap`, `get_return`,
  `get_return_by_output`, `get_rated_return`, `get_degen_return`
- **pool creation** — `add_simple_pool`, `add_stable_swap_pool`,
  `add_rated_swap_pool`, `add_degen_swap_pool`, `new`
- **MFT (multi FT)** — `mft_balance_of`, `mft_total_supply`,
  `mft_has_registered`, `mft_register`, `mft_unregister`, `mft_transfer`,
  `mft_transfer_call`, `mft_transfer_all_call`, `mft_resolve_transfer`,
  `mft_metadata`
- **NEP-141 / NEP-145 storage** — `storage_deposit`, `storage_withdraw`,
  `storage_unregister`, `storage_balance_bounds`, `storage_balance_of`,
  `ft_on_transfer`, `metadata`
- **token registration / allowlists** — `register_tokens`, `unregister_tokens`,
  `token_register_of`, `extend_whitelisted_tokens`, `remove_whitelisted_tokens`,
  `extend_frozenlist_tokens`, `remove_frozenlist_tokens`,
  `get_frozenlist_tokens`, `get_whitelisted_tokens`, `register_rated_token`,
  `unregister_rated_token`, `update_rated_token_extra_info`,
  `register_degen_token`, `update_degen_token`, `unregister_degen_token`
- **degen token oracles** — `register_degen_oracle_config`,
  `unregister_degen_oracle_config`, `update_degen_oracle_config`,
  `list_degen_oracle_configs`, `batch_get_degen_tokens`, `list_degen_tokens`,
  `update_degen_token_price`, `batch_update_degen_token_price`
- **owner / admin** — `upgrade`, `set_owner`, `get_owner`, `get_guardians`,
  `extend_guardians`, `remove_guardians`, `change_state`, `migrate`,
  `modify_admin_fee`, `modify_total_fee`, `modify_boost_farm_id`,
  `modify_burrowland_id`, `modify_wnear_id`,
  `remove_exchange_fee_liquidity`, `withdraw_owner_token`,
  `retrieve_unmanaged_token`, `add_degen_pool_limit`, `update_degen_pool_limit`,
  `remove_pool_limit`, `get_pool_limit_by_pool_id`, `get_pool_limit_paged`,
  `get_degen_pool_tvl`
- **referrals** — `insert_referral`, `update_referral`, `remove_referral`,
  `list_referrals`
- **auto-whitelist / postfix** — `extend_auto_whitelisted_postfix`,
  `remove_auto_whitelisted_postfix`
- **TWAP records** — `register_pool_twap_record`, `unregister_pool_twap_record`,
  `sync_pool_twap_record`, `modify_cumulative_info_record_interval_sec`,
  `get_pool_twap_info_view`, `list_pool_twap_info_view`,
  `get_unit_share_twap_token_amounts`, `get_unit_share_token_amounts`
- **echo / secure sender whitelists** — `extend_client_echo_token_id_whitelist`,
  `remove_client_echo_token_id_whitelist`, `get_client_echo_token_id_whitelist`,
  `extend_client_echo_sender_id_whitelist`, `remove_client_echo_sender_id_whitelist`,
  `get_client_echo_sender_id_whitelist`, `extend_secure_sender_whitelist`,
  `remove_secure_sender_whitelist`, `get_secure_sender_whitelist`
- **donations** — `donation_share`, `donation_token`
- **rate updates** — `update_token_rate`,
  `stable_swap_ramp_amp`, `stable_swap_stop_ramp_amp`
- **lostfound** — `get_lostfound_token`, `list_lostfound_tokens`, `claim_lostfound`
- **shadow records** — `get_shadow_records`
- **misc reads** — `version`, `get_tokens_paged`, `list_rated_tokens`,
  `batch_views`

Internal callback, initializer, upgrade, and cross-contract plumbing is
omitted here — those are never called directly.

==========================
<br/>
copyright 2026 by sleet.near