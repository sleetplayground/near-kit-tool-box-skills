---
name: sleet-kit-rhea-dclv2-ref-labs-near
description: Documents the dclv2.ref-labs.near concentrated-liquidity DEX contract - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL), arguments, and required gas and attached deposit. Use this skill whenever the user mentions dclv2, ref-labs, the concentrated liquidity DEX, a DCL quote, add_liquidity or remove_liquidity by position, mint_v_liquidity, or burn_v_liquidity - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
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
> verbatim. Substitute the caller's own values — an agent must never
> reuse the example token id, pool id, or amount as though it were the
> user's intent. Amounts here are yocto units, and copying one verbatim
> moves a real balance.

> #### ID FORMATS
>
> A `pool_id` is a composite STRING, not a number:
>
> ```
> <token_x_hash>|<token_y_hash_or_account>|<fee>
> ```
>
> for example
> `a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000`.
> The pool id's first segment is the token x **hash**, not its account id.
>
> An `lpt_id` is the pool id followed by the owner's liquidity index:
>
> ```
> <pool_id>#<index>
> ```
>
> for example
> `nearvember-337.meme-cooking.near|wrap.near|10000#17677`.
>
> `list_pools` and `list_liquidities` are the views that expose real ids.
> The website's own form (`token<>token@fee`) returns `null`.

### quote - VIEW

`pool_ids` is the ordered route through pools.

```json
{
  "pool_ids": ["a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000"],
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
  "pool_ids": ["a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000"],
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
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000"
}
```

answers with 23 fields: `pool_id`, `token_x`, `token_y`, `fee`,
`point_delta`, `current_point`, `liquidity`, `liquidity_x`,
`max_liquidity_per_point`, `total_x`, `total_y`, `total_liquidity`,
`total_order_x`, `total_order_y`, `volume_x_in`, `volume_x_out`,
`volume_y_in`, `volume_y_out`, `total_fee_x_charged`,
`total_fee_y_charged`, `state`, `whitelist`, and `tax`

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_pools - VIEW

```json
{
  "pool_ids": ["a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000"]
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

answers with an array of the same 23-field pool objects

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_liquidity - VIEW

```json
{
  "lpt_id": "nearvember-337.meme-cooking.near|wrap.near|10000#17677"
}
```

answers with 10 fields: `lpt_id`, `owner_id`, `pool_id`, `left_point`,
`right_point`, `amount`, `mft_id`, `v_liquidity`, `unclaimed_fee_x`, and
`unclaimed_fee_y`

`left_point` and `right_point` are NEGATIVE for a position below the
current point — the live answer for `sleet.near` is `-9400` and `-9200`

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_liquidity_view - VIEW

```json
{
  "lpt_id": "nearvember-337.meme-cooking.near|wrap.near|10000#17677"
}
```

answers with the same shape as `get_liquidity` but with `token_x`,
`token_y`, `fee_x`, and `fee_y` in place of `unclaimed_fee_x` and
`unclaimed_fee_y`

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_liquidity_range - VIEW

```json
{
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000",
  "left_point": 0,
  "right_point": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_pointorder_range - VIEW

```json
{
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000",
  "left_point": 0,
  "right_point": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_marketdepth - VIEW

```json
{
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000",
  "depth": 10
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_market_depth_list - VIEW

```json
{
  "pool_id_list": ["a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000"],
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
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000",
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

answers with an array of the same 10-field liquidity objects

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

answers with an array of the liquidity-view objects — the `get_liquidity_view`
shape

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_user_asset - VIEW

```json
{
  "account_id": "sleet.near",
  "token_id": "wrap.near"
}
```

answers with a BARE decimal string — the live answer on
`dclv2.ref-labs.near` is `"0"` — not an object. The account and token are
both in the request, so the response repeats neither

- gas: none — a view call is read-only and free
- attached deposit: 0

### list_user_assets - VIEW

```json
{
  "account_id": "sleet.near"
}
```

answers with a `{ token_id: balance }` map, the balances as decimal strings
in each token's smallest unit

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
- answers with 12 fields: `version`, `owner_id`, `farming_contract_id`,
  `next_owner_id`, `next_owner_accept_deadline`, `wnear_id`, `state`,
  `operators`, `protocol_fee_rate`, `pool_count`, and `user_count`.
  `pool_count` and `user_count` are JSON strings; `protocol_fee_rate` is a
  JSON number
- gas: none — a view call is read-only and free
- attached deposit: 0

### list_mft_supply - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### predict_add_liquidity - VIEW

```json
{
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000",
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
  "lpt_id": "nearvember-337.meme-cooking.near|wrap.near|10000#17677",
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
  "pool_id": "a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000",
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
  "lpt_id": "nearvember-337.meme-cooking.near|wrap.near|10000#17677",
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

---

#### TOKEN TRANSFERS

These are **not methods on this contract**. They are the standard
`ft_transfer` / `ft_transfer_call` calls made **on the token contract
you want to move** — `wrap.near`, `usdc.tether-token.near`, or any
other FT — with this DEX as the `receiver_id`.

##### ft_transfer_call on the input token - CALL

a swap does not call a `swap` method on this contract. It calls
`ft_transfer_call` on the **input token**, which routes into this
contract. `msg` carries the swap as a nested `Swap` object.

```json
{
  "receiver_id": "dclv2.ref-labs.near",
  "amount": "1000000000000000000000000",
  "msg": "{\"Swap\":{\"pool_ids\":[\"a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000\"],\"output_token\":\"usdc.tether-token.near\",\"min_output_amount\":\"0\"}}"
}
```

- `pool_ids` is the ordered route through pools
- set `min_output_amount` from a `quote` view result with your
  slippage applied — `"0"` accepts any output price
- the output token must have storage registered for the signer or the
  swap fails
- gas: 180 Tgas
- attached deposit: 1 yoctoNEAR

##### ft_transfer_call on the input token - CALL (deposit)

depositing a token into the DEX uses the same call with a `Deposit` msg

```json
{
  "receiver_id": "dclv2.ref-labs.near",
  "amount": "1000000000000000000000000",
  "msg": "\"Deposit\""
}
```

- `msg` is the JSON string `"Deposit"`, including its quotes
- gas: 1000 Tgas
- attached deposit: 1 yoctoNEAR

==========================
<br/>
copyright 2026 by sleet.near