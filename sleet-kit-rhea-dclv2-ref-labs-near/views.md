# dclv2.ref-labs.near

the ref-finance concentrated-liquidity dex v2 smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: dclv2.ref-labs.near
- TESTNET: dclv2.ref-dev.testnet

> #### ID FORMATS
>
> A `pool_id` is a composite STRING, not a number:
>
> ```
> <token_x_account_id>|<token_y_account_id>|<fee>
> ```
>
> for example
> `a0b86991c6218b36c1d19d4a2e9eb0ce3606eb48.factory.bridge.near|wrap.near|2000`.
> Both segments are the tokens' NEAR account ids; `fee` is in basis points.
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

---

#### VIEWS

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Substitute the caller's own values — an agent must never
> reuse the example token id, pool id, or amount as though it were the
> user's intent. Amounts here are yocto units, and copying one verbatim
> moves a real balance.

see [SKILL.md](./SKILL.md) for the overview and the other files.

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

==========================
<br/>
copyright 2026 by sleet.near
