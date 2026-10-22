---
name: sleet-kit-rhea-dclv2-ref-labs-near
description: Documents the dclv2.ref-labs.near concentrated-liquidity DEX contract - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions dclv2, ref-labs, the concentrated liquidity DEX, a DCL quote, add_liquidity or remove_liquidity by position, mint_v_liquidity, or burn_v_liquidity - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---

# dclv2.ref-labs.near

the ref-finance concentrated-liquidity dex v2 smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: dclv2.ref-labs.near
- TESTNET: dclv2.ref-dev.testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example token id, pool id, or amount as though it were the user's
> intent. Amounts here are yocto units, and copying one verbatim moves
> a real balance.

### quote - VIEW

`pool_ids` is the ordered route through pools.

```json
{
  "pool_ids": [1],
  "input_token": "wrap.near",
  "output_token": "usdc.tether-token.near",
  "input_amount": "1000000000000000000000000",
  "tag": "0"
}
```

`tag` is an optional opaque label echoed back in the response

- gas: none — a view call is read-only and free
- attached deposit: 0

### quote_by_output - VIEW

```json
{
  "pool_ids": [1],
  "input_token": "wrap.near",
  "output_token": "usdc.tether-token.near",
  "output_amount": "1000000"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_pool - VIEW

```json
{
  "pool_id": 1
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_pools - VIEW

```json
{
  "pool_ids": [1]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_pools - VIEW

```json
{
  "from_index": 0,
  "limit": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_liquidity - VIEW

```json
{
  "lpt_id": "1:0:100"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_liquidity_view - VIEW

```json
{
  "lpt_id": "1:0:100"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_liquidity_range - VIEW

```json
{
  "pool_id": 1,
  "left_point": 0,
  "right_point": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_pointorder_range - VIEW

```json
{
  "pool_id": 1,
  "left_point": 0,
  "right_point": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_marketdepth - VIEW

```json
{
  "pool_id": 1,
  "depth": 10
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_market_depth_list - VIEW

```json
{
  "pool_id_list": [1],
  "depth": 10
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_order - VIEW

```json
{
  "order_id": "1:0"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### find_order - VIEW

```json
{
  "account_id": "sleet.near",
  "pool_id": 1,
  "point": 0
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_active_orders - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_history_orders - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_liquidities - VIEW

```json
{
  "account_id": "sleet.near",
  "from_index": 0,
  "limit": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_liquidity_views - VIEW

```json
{
  "account_id": "sleet.near",
  "from_index": 0,
  "limit": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_user_asset - VIEW

```json
{
  "account_id": "sleet.near",
  "token_id": "wrap.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_user_assets - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_user_storage_detail - VIEW

```json
{
  "user_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### list_mft_supply - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### predict_add_liquidity - VIEW

```json
{
  "pool_id": 1,
  "left_point": 0,
  "right_point": 100,
  "amount_x": "0",
  "amount_y": "0"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### predict_remove_liquidity - VIEW

```json
{
  "lpt_id": "1:0:100",
  "liquidity": "1000"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### predict_hotzap - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_user_mft_asset - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### list_user_mft_assets - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_contract_storage_report - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_pool_whitelist - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### is_pool_account_allowed - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### is_valid_referral - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_frozenlist_tokens - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### list_vip_users - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_vip_user_discount - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_referral_whitelist - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_secure_sender_whitelist - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_client_echo_token_whitelist - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_client_echo_sender_whitelist - VIEW

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

### storage_balance_bounds - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### add_liquidity - CALL

```json
{
  "pool_id": 1,
  "left_point": 0,
  "right_point": 100,
  "amount_x": "0",
  "amount_y": "0",
  "min_amount_x": "0",
  "min_amount_y": "0"
}
```

`min_amount_x` and `min_amount_y` are the slippage bounds — set them
above zero before using this with value

- gas: 300 Tgas
- attached deposit: 0

### remove_liquidity - CALL

```json
{
  "lpt_id": "1:0:100",
  "amount": "1000",
  "min_amount_x": "0",
  "min_amount_y": "0"
}
```

- gas: 300 Tgas
- attached deposit: 0

### mint_v_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### burn_v_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### append_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### merge_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### batch_add_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### batch_remove_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### batch_update_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### batch_mint_v_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### batch_burn_v_liquidity - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### ft_transfer_call (helper: deposit) - CALL

deposits a token into the DEX — the helper is `dclv2_deposit`, and
`msg` is the JSON string `"Deposit"`, including its quotes

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000",
  "msg": "\"Deposit\""
}
```

- gas: 1000 Tgas
- attached deposit: 1 yoctoNEAR

### withdraw_asset - CALL

```json
{
  "token_id": "wrap.near",
  "amount": "1000000000000000000000000"
}
```

omit `amount` to withdraw the full balance of that token. Check
`get_user_asset` or `list_user_assets` first.

- gas: 300 Tgas
- attached deposit: 0

### cancel_order - CALL

```json
{
  "order_id": "1:0",
  "amount": "0"
}
```

omit `amount` for a full cancel. Pass `"0"` to claim the filled
(`unclaimed_amount`) without cancelling the remainder.

- gas: 300 Tgas
- attached deposit: 0

### storage_deposit - CALL

```json
{
  "registration_only": true
}
```

the minimum from `storage_balance_bounds` is **not** enough in practice
— 0.5 NEAR worked on `dclv2.ref-dev.testnet`

- gas: not documented yet
- attached deposit: 0.5 NEAR

### storage_withdraw - CALL

- no args necessary
- gas: 300 Tgas
- attached deposit: 1 yoctoNEAR

### storage_unregister - CALL

- no args necessary
- gas: 300 Tgas
- attached deposit: 1 yoctoNEAR

### ft_transfer_call (helper: swap) - CALL

performs a swap via `ft_transfer_call`

```json
{
  "receiver_id": "sleet.near",
  "amount": "1000000000000000000000000",
  "msg": ""
}
```

- gas: 180 Tgas
- attached deposit: 1 yoctoNEAR

### create_pool - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### clear_pool_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### claim_charged_fee - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### pause_contract - CALL

- no args necessary
- gas: not documented yet
- attached deposit: not documented yet

### resume_contract - CALL

- no args necessary
- gas: not documented yet
- attached deposit: not documented yet

### pause_pool - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### resume_pool - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_pool_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### extend_frozenlist_tokens - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_frozenlist_tokens - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### extend_operators - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_operators - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### modify_protocol_fee_rate - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### modify_storage_params - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_farming_contract_id - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_vip_user - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_vip_user - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### add_next_owner - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### grant_next_owner - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### confirm_next_owner - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### cancel_next_owner - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### upsert_referral_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_referral_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### upsert_secure_sender_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_secure_sender_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### upsert_client_echo_token_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_client_echo_token_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### upsert_client_echo_sender_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_client_echo_sender_whitelist - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### mft_transfer - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### mft_transfer_call - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### mft_transfer_all_call - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### mft_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### mft_total_supply - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### mft_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_transfer - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### nft_transfer_call - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### nft_approve - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### nft_revoke - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### nft_revoke_all - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### nft_is_approved - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_token - VIEW

```json
{
  "token_id": "1"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_tokens - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_tokens_for_owner - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_supply_for_owner - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_total_supply - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near