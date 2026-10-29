# dclv2.ref-labs.near — contract calls

the **CALL** methods of the DCL v2 contract — the ones that change state
and are sent as a transaction.

this file covers the methods of the contract itself. the token inflows
that are *not* contract methods live in
[ft_transfer_call.md](./ft_transfer_call.md), and the read methods live in
[views.md](./views.md).

see [SKILL.md](./SKILL.md) for the overview and the other files.

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
> <token_x_account_id>|<token_y_account_id>|<fee>
> ```
>
> Both segments are the tokens' NEAR account ids; `fee` is in basis points.
>
> An `lpt_id` is the pool id followed by the owner's liquidity index:
>
> ```
> <pool_id>#<index>
> ```

---

#### CALLS

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
