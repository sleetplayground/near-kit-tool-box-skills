# INTEAR DEX simple-amm sub-dex

the INTEAR DEX `simple-amm` sub-dex — the smallest working AMM.

one liquidity provider per pool, constant-product math, no LP shares.
the source describes it as the demonstration of the engine's basic
functionality: swaps, adding and withdrawing liquidity, storage
management, and event emission.

it runs as WASM **inside** the INTEAR DEX engine (`dex.intear.near`),
not as its own account. its views are read through the engine's
`dex_view`, and its calls go through the engine's `dex_call` or
`swap_simple`.

see [SKILL.md](./SKILL.md) for the overview and the other sub-dexes.

---

CONTRACT ID PER NETWORK

- MAINNET: **not deployed** — no dex id is live
- TESTNET: (none)

the dex id is `<deployer>/<id>`, but no simple-amm dex has been
deployed on mainnet. the methods below are the ones the source
defines; check that the dex exists before reading anything.

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example pool id, account id, or asset id as though it were the
> user's intent. Adding or withdrawing liquidity with a copied value
> moves real funds.
>
> #### BORSH, NOT JSON
>
> the simple-amm dex's arguments and responses are **borsh** encoded,
> not JSON. they travel through the engine's `dex_view` / `dex_call`
> as base64 blobs. the JSON blocks below show the *logical* shape of
> each argument, not the bytes on the wire.
>
> #### NOT AUDITED
>
> The INTEAR DEX source repository states plainly: *"Not audited."*

---

#### THE `dex_view` ENVELOPE

```json
{
  "dex_id": "<deployer>/<id>",
  "method": "get_pool",
  "args": "<base64 of the borsh-encoded arguments>"
}
```

the response is a base64 string holding the dex's borsh-encoded
result.

---

### get_pool - VIEW

```json
{
  "pool_id": 0
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the pool at `pool_id`, or `null` when there is no such pool.

the pool holds:

- `assets`: two `{ asset_id, balance }` entries
- `owner_id`: the pool's single liquidity provider

an `asset_id` is `near`, `nep141:<contract_id>`,
`nep245:<contract_id>:<token_id>` or
`nep171:<contract_id>:<token_id>`.

---

### new - CALL (init)

- no args necessary
- gas: not documented yet
- attached deposit: 1 yoctoNEAR

the constructor.

### swap - CALL

the swap message is borsh encoded: `{ "pool_id": 0 }`.

the swap is executed through the engine's `swap_simple`, not through
`dex_view` — the engine reserves the name `swap`.

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

### create_pool - CALL

```json
{
  "assets": ["near", "nep141:wrap.near"]
}
```

- gas: not documented yet
- attached deposit: the NEAR for storage

creates a pool with the two assets. the creator becomes its owner.

### add_liquidity - CALL

```json
{
  "pool_id": 0
}
```

- gas: not documented yet
- attached deposit: the NEAR for storage, plus the two assets

only the pool's owner may add liquidity.

### remove_liquidity - CALL

```json
{
  "pool_id": 0,
  "assets_to_remove": ["1000000000000000000000000", "2000000000000000000000000"]
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

only the pool's owner may remove liquidity. the two amounts are the
amounts to take out of the pool's first and second asset.

---

#### SOURCE

- https://github.com/INTEARnear/dex/tree/main/dexes/simple-amm

```sh
# the engine's method list — the dex's own methods are not on the
# account, they run inside it
near contract inspect dex.intear.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
