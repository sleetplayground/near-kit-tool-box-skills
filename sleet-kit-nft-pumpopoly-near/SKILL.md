---
name: sleet-kit-nft-pumpopoly-near
description: Documents the nft.pumpopoly.near NFT contract - the mainnet contract id, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions nft.pumpopoly.near, a pumpopoly NFT, nft_token, nft_transfer, tokens for owner, or bond NFTs - including when they want to call or inspect this contract and need the method list, argument names, or contract id.
---

# nft.pumpopoly.near

the pumpopoly nft.pumpopoly.near NFT smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: nft.pumpopoly.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example receiver or token id as though it were the user's own.
> Token ids on this contract are human readable and contain spaces
> (e.g. `"Bond 10-849-1"`), so quote them when passing from a shell.

### nft_token - VIEW

```json
{
  "token_id": "Bond 10-849-1"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_tokens_for_owner - VIEW

`from_index` and `limit` are optional — the contract applies its own
defaults. Keep `limit` modest (~50) since each token carries a full
metadata blob and the view call has a gas budget.

```json
{
  "account_id": "sleet.near",
  "from_index": "0",
  "limit": 50
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_token_ids_for_owners - VIEW

not paginated, so a full 100+ token wallet comes back in one call

```json
{
  "account_ids": ["sleet.near", "narkmeta.near"]
}
```

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

### nft_tokens_by_ids - VIEW

```json
{
  "token_ids": ["Bond 10-849-1"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_tokens_ids - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### nft_tokens - VIEW

- arg format not documented yet
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

### nft_transfer - CALL

`approval_id` and `memo` are optional and serialize as `null` when
omitted.

```json
{
  "receiver_id": "sleet.near",
  "token_id": "Bond 10-849-1",
  "approval_id": null,
  "memo": null
}
```

- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

### nft_transfer_call - CALL

```json
{
  "receiver_id": "sleet.near",
  "token_id": "Bond 10-849-1",
  "approval_id": null,
  "memo": null,
  "msg": ""
}
```

- gas: 100 Tgas
- attached deposit: 1 yoctoNEAR

### nft_transfer_payout - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

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

### nft_mint - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### nft_mint_bond - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### nft_sync - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### nft_from_land - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### nft_payout - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

==========================
<br/>
copyright 2026 by sleet.near