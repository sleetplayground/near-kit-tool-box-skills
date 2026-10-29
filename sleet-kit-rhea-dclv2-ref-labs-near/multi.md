# dclv2.ref-labs.near — multi ft and nft

the DCL v2 contract also carries a **multi ft** (`mft_*`) and an **nft**
(`nft_*`) interface.

these are separate from the core liquidity and swap methods. their
argument shapes are **not documented yet** — verify them with
`near contract inspect` before calling them with value.

see [SKILL.md](./SKILL.md) for the overview and the other files.

---

CONTRACT ID PER NETWORK

- MAINNET: dclv2.ref-labs.near
- TESTNET: dclv2.ref-dev.testnet

---

#### MULTI FT AND NFT

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

==========================
<br/>
copyright 2026 by sleet.near
