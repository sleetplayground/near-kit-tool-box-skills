# slimedragon.near/xyk (INTEAR DEX xyk)

the INTEAR DEX `xyk` sub-dex — a constant-product (x\*y=k) AMM with
two assets per pool.

it runs as WASM **inside** the INTEAR DEX engine (`dex.intear.near`),
not as its own account. its views are read through the engine's
`dex_view`, and its calls go through the engine's `dex_call` or
`swap_simple`.

see [SKILL.md](./SKILL.md) for the overview and the other sub-dexes.

---

CONTRACT ID PER NETWORK

- MAINNET: dex id `slimedragon.near/xyk`
- TESTNET: (none — mainnet only)

the dex id is `<deployer>/<id>` — the account that deployed the dex
code and the name it was deployed under.

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example pool id, account id, or asset id as though it were the
> user's intent. Removing liquidity or withdrawing fees with a copied
> value moves real funds.
>
> #### BORSH, NOT JSON
>
> the xyk dex's arguments and responses are **borsh** encoded, not
> JSON. they travel through the engine's `dex_view` / `dex_call` as
> base64 blobs. the JSON blocks below show the *logical* shape of
> each argument, not the bytes on the wire.
>
> #### NOT AUDITED
>
> The INTEAR DEX source repository states plainly: *"Not audited."*

---

#### THE `dex_view` ENVELOPE

```json
{
  "dex_id": "slimedragon.near/xyk",
  "method": "get_pool",
  "args": "<base64 of the borsh-encoded arguments>"
}
```

the response is a base64 string holding the dex's borsh-encoded
result.

---

### get_pool_count - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns how many pools the dex holds. the count is the id of the next
pool to be created, so valid pool ids run `0 .. count - 1`.

### get_pool - VIEW

```json
{
  "pool_id": 3
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the pool at `pool_id`, or `null` when there is no such pool.

the pool's shape depends on its kind:

- **`Private`** — a single-owner pool
  - `assets`: two `{ asset_id, balance }` entries
  - `fees`: the pool's fees at the current block
  - `fee_configuration`: the fee rules
  - `owner_id`: the pool's owner
  - `locked`: whether the pool is locked
- **`Public`** — a shared pool with LP shares
  - `assets`, `fees`, `fee_configuration` as above
  - `total_shares`: the pool's total LP shares, or `null`
- **`Launch`** — a token-launch pool quoted in NEAR
  - `near_amount`, `launched_asset`, `fees`, `fee_configuration`,
    `phantom_liquidity_near`
- **`LaunchV2`** — a token-launch pool with an arbitrary quote asset
  - `quote_asset`, `launched_asset`, `fees`, `fee_configuration`,
    `phantom_liquidity`

an `asset_id` is `near`, `nep141:<contract_id>`,
`nep245:<contract_id>:<token_id>` or
`nep171:<contract_id>:<token_id>`.

a fee receiver is `{"Account": "alice.near"}`, `"Pool"`, or
`{"Community": "alice.near"}`.

a fee fraction is **parts per million** — `1000000` is 100%.

`fee_configuration` is either:

- `{"V1": {"receivers": [[receiver, fraction], ...]}}` — a fixed fee
  per receiver
- `{"V2": {"receivers": [[receiver, amount], ...]}}` — a per-receiver
  fee rule, where `amount` is one of:
  - `{"Fixed": 30000}` — a fixed fraction
  - `{"Scheduled": {"start": ["1771599694000000000", 60000], "end": ["1771603294000000000", 15000], "curve": "Linear"}}`
    — a fee that falls linearly between the two timestamps. the
    timestamps are nanoseconds since the epoch, as decimal strings —
    they do not fit a 64-bit JSON number.
  - `{"Dynamic": {"min": 1000, "max": 30000}}` — not implemented yet

the protocol fee is added on top of the pool's own fees, and is
charged to `plach.intear.near`.

### get_pools - VIEW

```json
{
  "start_index": 0,
  "limit": 50
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns a page of pools, at most `limit` of them, starting at
`start_index`. each pool has the shape `get_pool` returns.

to page through every pool, repeat the call with `start_index` moving
on by `limit`.

### get_pool_shares - VIEW

```json
{
  "pool_ids": [3, 13],
  "account_id": "alice.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the LP shares `account_id` holds in each of `pool_ids`, in the
order the pools were asked for.

each entry has two states:

- `null` — the pool is **not a public pool**, so it has no per-account
  share map at all. a private or launch pool always answers `null`.
- `"123456"` — the account holds that many shares in that pool. the
  count is `"0"` when the account has no entry in the pool's share
  map.

a share is worth its pro-rata part of the pool's two assets.

### get_pending_fees - VIEW

```json
{
  "account_id": "alice.near",
  "asset_ids": ["near", "nep141:wrap.near"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the fees `account_id` has collected but not withdrawn yet, as
a map from asset id to amount. an asset the account has never
collected a fee for is absent from the map.

### get_fee_discount_signer - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the public key that signs fee discounts for this dex, or
`null` when fee discounts are disabled.

a fee discount lowers the pool and protocol fees of a swap. the
discount itself is signed off-chain and handed to the dex as part of
the swap message — this view only tells you which key to trust.

### get_community_owned_fees - VIEW

```json
{
  "account_id": "alice.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR (yocto) a community account has collected from
launch pools, in total. community fee receivers are only supported in
launch pools, and the amount is NEAR even when the pool trades another
quote asset.

### pool_needs_upgrade - VIEW

```json
{
  "pool_id": 3
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns whether the pool at `pool_id` still runs an old pool layout
and has to be migrated. the view fails when there is no pool with
that id.

---

### new - CALL (init)

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

the constructor.

### swap - CALL

the swap message is borsh encoded and depends on the dex version:

- a single pool, no fee discount — the pool id as a `u32`
- otherwise — `{ routes: [{ fraction, pool_ids }], fee_discount:
  null }`, where `fraction` is parts per million of the requested
  amount that route takes and the route fractions must add up to
  `1000000`

the swap is executed through the engine's `swap_simple`, not through
`dex_view` — the engine reserves the name `swap`.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### create_pool - CALL

```json
{
  "assets": ["near", "nep141:wrap.near"],
  "fees": { "V1": { "receivers": [] } },
  "pool_type": "PublicLatest",
  "initial_liquidity": ["1000000000000000000000000", "2000000000000000000000000"]
}
```

- gas: not documented yet
- attached deposit: the NEAR for storage, plus the initial liquidity

the assets are attached to the call. `pool_type` is one of
`PrivateLatest`, `PublicLatest`, `LaunchLatest`, `LaunchV1`,
`LaunchV2`, `PrivateV1`, `PublicV1`, `PrivateV2`, `PublicV2`. public
pools must be created with an initial liquidity; launch pools take the
launched asset from the attached assets instead.

### register_liquidity - CALL

```json
{
  "pool_id": 3
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### add_liquidity - CALL

```json
{
  "pool_id": 3,
  "min_shares_received": null
}
```

- gas: not documented yet
- attached deposit: the NEAR for storage, plus the two assets

### remove_liquidity - CALL

```json
{
  "pool_id": 3,
  "shares_to_remove": null,
  "min_assets_received": null
}
```

omit `shares_to_remove` to remove the caller's whole position. set
`min_assets_received` to guard against slippage.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### edit_fees - CALL

```json
{
  "pool_id": 3,
  "fees": { "V1": { "receivers": [] } }
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### withdraw_fees - CALL

```json
{
  "assets": ["near"]
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### upgrade_pool - CALL

```json
{
  "pool_id": 3
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### lock_pool - CALL

```json
{
  "pool_id": 3
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### register_fee_assets - CALL

```json
{
  "asset_ids": ["near"]
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### withdraw_community_fee - CALL

```json
{
  "account_id": "alice.near"
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### set_fee_discount_signer - CALL

```json
{
  "fee_discount_signer_public_key": null
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

only the dex's owner may set the signer.

---

#### SOURCE

- https://github.com/INTEARnear/dex/tree/main/dexes/xyk
- the argument and response types live in
  `dexes/xyk/types/src/lib.rs`

```sh
# the engine's method list — the dex's own methods are not on the
# account, they run inside it
near contract inspect dex.intear.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
