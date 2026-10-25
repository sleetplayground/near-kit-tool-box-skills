# dex.intear.near (INTEAR DEX engine)

the INTEAR DEX engine — the host contract of every INTEAR DEX
sub-dex.

the engine runs each sub-dex as WASM inside itself, keeps the users'
and the dexes' inner balances, and exposes the sub-dexes' views
through `dex_view` and their calls through `dex_call`.

see [SKILL.md](./SKILL.md) for the overview and the other sub-dexes.

---

CONTRACT ID PER NETWORK

- MAINNET: dex.intear.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id, dex id, or asset id as though it were the user's
> intent. Withdrawing or swapping with a copied value moves real funds.
>
> #### NOT AUDITED
>
> The INTEAR DEX source repository states plainly: *"Not audited."*
> Treat every method here accordingly.

### SUB-DEXES

the sub-dexes are not separate accounts. they are WASM blobs the
engine runs, identified by a `<deployer>/<id>` dex id:

| dex id                | dex        | mainnet deployed |
| --------------------- | ---------- | ---------------- |
| `slimedragon.near/xyk`| xyk        | yes              |
| —                     | otc        | no               |
| —                     | simple-amm | no               |
| —                     | minimal    | no               |

each sub-dex has its own file in this skill — [xyk.md](./xyk.md),
[otc.md](./otc.md), [simple-amm.md](./simple-amm.md) and
[minimal.md](./minimal.md).

---

#### TYPES

`AssetId` — a string:

- `near` — native NEAR
- `nep141:<contract_id>` — NEP-141 fungible token, e.g.
  `nep141:wrap.near`
- `nep245:<contract_id>:<token_id>` — NEP-245 multi-token
- `nep171:<contract_id>:<token_id>` — NEP-171 NFT

`DexId` — a string, `<deployer>/<id>`, e.g. `slimedragon.near/xyk`.

`AccountOrDexId` — externally tagged:

```json
{ "Account": "alice.near" }
```

```json
{ "Dex": "slimedragon.near/xyk" }
```

all 128-bit amounts are decimal strings in yocto units.

---

### READING A SUB-DEX

`dex_view` is how a sub-dex's view is reached. it takes a JSON
envelope:

```json
{
  "dex_id": "slimedragon.near/xyk",
  "method": "get_pool_count",
  "args": "<base64 of the borsh-encoded arguments>"
}
```

the response is a base64 string holding the sub-dex's **borsh**
encoded result. the method name `swap` is reserved by the engine and
is rejected by `dex_view` — swaps go through `swap_simple` or
`dex_call`.

---

### is_paused - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns `true` when the engine rejects every state change. its views
keep working while it is paused.

### get_trusted_code_deployer - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the account that may deploy or upgrade a sub-dex's code. the
engine does not validate a sub-dex's code when it loads it, so only
this account's deployments are trustworthy.

### contract_source_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the nep330 metadata: `version`, `link` (the source commit
the deployed WASM was built from), `standards`, and `build_info`.

### untracked_near - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR the engine contract holds that no user or dex owns.

### total_in_custody - VIEW

```json
{
  "asset_id": "near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the total amount of an asset the engine holds for the dexes
running inside it, or `null` when that asset was never registered.

### asset_balance_of - VIEW

```json
{
  "of": { "Account": "alice.near" },
  "asset_id": "near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the engine's inner balance of one asset for one account or
one dex, or `null` when there is no such balance.

### registered_assets_of - VIEW

```json
{
  "account_id": "alice.near",
  "dex_id": null,
  "from_index": 0,
  "limit": 10
}
```

`account_id` and `dex_id` are **mutually exclusive** — pass exactly
one of them. `dex_id` is the `<deployer>/<id>` string form.

- gas: none — a view call is read-only and free
- attached deposit: 0

returns a page of `[asset_id, balance]` tuples — the registered assets
of an account or a dex and their inner balances.

### are_assets_registered - VIEW

```json
{
  "asset_ids": ["near"],
  "for": { "Account": "alice.near" }
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns whether every one of `asset_ids` is registered for an account
or a dex.

### storage_balance_of - VIEW

```json
{
  "account_id": "alice.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR storage deposit an account has with the engine, or
`null` when it has none. the shape is `{ "total": "...", "available":
"..." }`.

### storage_balance_bounds - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns `{ "min": "...", "max": null }` — the deposit bounds the
engine accepts for a user.

### dex_storage_balance_of - VIEW

```json
{
  "dex_id": "slimedragon.near/xyk"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR storage deposit a sub-dex has with the engine, or
`null` when it has none.

### dex_storage_balance_bounds - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the deposit bounds the engine accepts for a sub-dex.

### total_storage_balances - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns `{ "users": {...}, "dexes": {...} }` — the NEAR storage the
engine holds for users and for dexes.

### dex_code - VIEW

```json
{
  "dex_id": "slimedragon.near/xyk"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the sub-dex's WASM code, base64 encoded. the engine does not
validate the code when it loads it, so only the trusted code
deployer's deployments are trustworthy.

### dex_view - VIEW

```json
{
  "dex_id": "slimedragon.near/xyk",
  "method": "get_pool_count",
  "args": ""
}
```

`args` is the base64 of the sub-dex's **borsh**-encoded arguments —
an empty string for methods that take none. the response is a base64
string holding the sub-dex's borsh-encoded result.

- gas: none — a view call is read-only and free
- attached deposit: 0

`method` must not be `swap` — that name is reserved and the call is
rejected.

### simulate_swap_simple - VIEW

```json
{
  "dex_id": "slimedragon.near/xyk",
  "message": "<base64 of the borsh-encoded swap message>",
  "trader": "alice.near",
  "asset_in": "near",
  "asset_out": "nep141:wrap.near",
  "amount": { "ExactIn": "1000000000000000000000000" },
  "referrer": null,
  "referral_fees": []
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

simulates a swap without executing it and returns the expected
`[amount_in, amount_out]`. `amount` is `{ "ExactIn": "..." }` or
`{ "ExactOut": "..." }`. `referral_fees` is a list of
`[account_id, fee_fraction]` pairs, where `1000000` is 100% and all
of them together may charge at most 5%.

---

### swap_simple - CALL

```json
{
  "dex_id": "slimedragon.near/xyk",
  "message": "<base64 of the borsh-encoded swap message>",
  "asset_in": "near",
  "asset_out": "nep141:wrap.near",
  "amount": { "ExactIn": "1000000000000000000000000" },
  "referrer": null,
  "referral_fees": [],
  "constraint": null
}
```

`constraint` is the minimum amount out for an `ExactIn` swap, or the
maximum amount in for an `ExactOut` swap.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### dex_call - CALL

```json
{
  "dex_id": "slimedragon.near/xyk",
  "method": "add_liquidity",
  "args": "<base64 of the borsh-encoded arguments>",
  "attached_assets": { "near": "1000000000000000000000" },
  "referrer": null
}
```

an arbitrary call to a sub-dex method — adding liquidity, removing
liquidity, editing fees, and so on. `args` are borsh encoded.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### deploy_dex_code - CALL

```json
{
  "last_part_of_id": "xyk",
  "code_base64": "<base64 of the wasm>"
}
```

deploys or upgrades a sub-dex's code under the caller's dex id. only
the trusted code deployer may do this.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### register_assets - CALL

```json
{
  "asset_ids": ["near"],
  "for": { "Account": "alice.near" }
}
```

registers assets for an account or a dex, reserving storage for their
balances. unregistering is not possible. a no-op when the assets are
already registered.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### transfer_asset - CALL

```json
{
  "to": { "Account": "bob.near" },
  "asset_id": "near",
  "amount": "1000000000000000000000000"
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### withdraw - CALL

```json
{
  "asset_id": "near",
  "amount": { "Full": { "at_least": null } },
  "withdraw_to": null
}
```

withdraws an asset from the engine's inner balance. `amount` is
`{ "Full": { "at_least": "..." } }` or `{ "Exact": "..." }`. omit
`withdraw_to` to withdraw to the caller's own account.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### execute_operations - CALL

```json
{
  "operations": [],
  "referrer": null,
  "referral_fees": []
}
```

runs a batch of operations — registering assets, deploying dex code,
withdrawing, swapping, calling a dex, transferring. see the source
for the operation list.

- gas: not documented yet
- attached deposit: at least 1 yoctoNEAR

### deposit_near - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: the NEAR amount to deposit into the engine

### storage_deposit - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: the NEAR amount to reserve as storage

### storage_withdraw - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### storage_unregister - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### dex_storage_deposit - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: the NEAR amount to reserve as storage

### dex_storage_withdraw - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### new - CALL (init)

```json
{
  "trusted_code_deployer": "alice.near"
}
```

the constructor. sets the account that may deploy sub-dex code.

- gas: not documented yet
- attached deposit: not documented yet

### pause - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

only the accounts the engine's source authorizes may pause it.

### unpause - CALL

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### set_trusted_code_deployer - CALL

```json
{
  "account_id": "alice.near"
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### ft_on_transfer - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### mt_on_transfer - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### nft_on_transfer - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### rescue - CALL

```json
{
  "asset_id": "near",
  "amount": null,
  "to": "alice.near"
}
```

takes an asset out of the engine that no user or dex owns.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### after_rescue_balance_nep141_nep245_query - CALL (private)

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

a private callback of `rescue`. not callable from outside.

### after_rescue_nep171_query - CALL (private)

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

a private callback of `rescue`. not callable from outside.

### after_withdraw - CALL (private)

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

a private callback of `withdraw`. not callable from outside.

---

#### SOURCE

- https://github.com/INTEARnear/dex

the mainnet WASM was built from commit
`fd30f501ec27dbc3239e1529b4ce1d8cf5b5ecff`, version `0.1.0`. check
`contract_source_metadata` for the commit the *current* deployment was
built from.

```sh
# the authoritative method list
near contract inspect dex.intear.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
