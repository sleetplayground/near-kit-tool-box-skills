---
name: sleet-kit-intear-dex
description: Documents the whole INTEAR DEX - the engine contract dex.intear.near and every sub-dex running inside it (xyk, otc, simple-amm, minimal) - the contract ids and dex ids per network, the dex_view and dex_call envelopes, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions the INTEAR DEX, dex.intear.near, dex_view, dex_call, deploy_dex_code, swap_simple, simulate_swap_simple, execute_operations, register_assets, an INTEAR DEX inner balance, a dex id like slimedragon.near/xyk, an INTEAR DEX LP position or pool, an otc trade intent, or a no-op dex - including when they want to call or inspect these contracts and need the method list, argument names, the borsh encoding the dex_view envelope uses, or the network-specific contract id.
---

# INTEAR DEX

the INTEAR DEX — an on-chain DEX host and the trading venues that run
inside it.

the **engine** (`dex.intear.near`) is the only real account. it runs
each **sub-dex** as WASM inside itself, keeps the users' and the
dexes' inner balances, and exposes the sub-dexes' views through
`dex_view` and their calls through `dex_call`.

this skill covers the engine and all four sub-dexes. each component
has its own file:

| component   | file                                   | mainnet deployed            |
| ----------- | -------------------------------------- | --------------------------- |
| engine      | [engine.md](./engine.md)               | yes — `dex.intear.near`     |
| xyk         | [xyk.md](./xyk.md)                     | yes — `slimedragon.near/xyk` |
| otc         | [otc.md](./otc.md)                     | no                          |
| simple-amm  | [simple-amm.md](./simple-amm.md)       | no                          |
| minimal     | [minimal.md](./minimal.md)             | no                          |

> #### NOT AUDITED
>
> The INTEAR DEX source repository states plainly: *"Not audited."*
> Treat every method here accordingly.

---

CONTRACT ID PER NETWORK

- MAINNET: dex.intear.near (engine), `slimedragon.near/xyk` (xyk dex id)
- TESTNET: (none — mainnet only)

a **dex id** is `<deployer>/<id>` — the account that deployed the dex
code and the name it was deployed under. only the xyk sub-dex is
deployed on mainnet.

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

#### THE `dex_view` / `dex_call` ENVELOPES

the engine's own views are JSON. a **sub-dex** view is reached
through the engine's `dex_view`, which takes the sub-dex's
**borsh**-encoded arguments as base64 and answers with a base64 blob
of borsh-encoded bytes:

```json
{
  "dex_id": "slimedragon.near/xyk",
  "method": "get_pool_count",
  "args": "<base64 of the borsh-encoded arguments>"
}
```

`dex_call` is the same envelope for a call, plus the attached assets:

```json
{
  "dex_id": "slimedragon.near/xyk",
  "method": "add_liquidity",
  "args": "<base64 of the borsh-encoded arguments>",
  "attached_assets": { "near": "1000000000000000000000" },
  "referrer": null
}
```

the method name `swap` is reserved by the engine and is rejected by
`dex_view` — swaps go through `swap_simple` or `dex_call`.

> #### ARG EXAMPLES
>
> The JSON blocks in the component files are **worked examples**, not
> values to copy verbatim. Treat them as a template for argument
> *shape* only, and substitute the caller's own values — an agent
> must never reuse the example account id, dex id, pool id, or asset
> id as though it were the user's intent. Withdrawing, swapping, or
> adding liquidity with a copied value moves real funds.

---

### ENGINE — WHERE TO START

the engine's own method list, the inner balances it holds, the
storage deposits it takes, and the calls that reach into a sub-dex:

- [engine.md](./engine.md) — `dex_view`, `dex_call`, `swap_simple`,
  `simulate_swap_simple`, `deploy_dex_code`, `register_assets`,
  `withdraw`, `transfer_asset`, `execute_operations`, the storage
  methods, `pause` / `unpause`, and the transfer callbacks.

---

### SUB-DEXES — WHERE TO START

each sub-dex has its own methods, arguments, and fee rules. open the
file for the one the user is asking about:

- [xyk.md](./xyk.md) — the constant-product AMM. the deployed one.
  pool views, LP share views, fee views, pool creation, liquidity,
  fees, and the multi-route swap message.
- [otc.md](./otc.md) — the over-the-counter desk. trade intents,
  matching, the desk's internal balances, authorized keys, and
  nonces.
- [simple-amm.md](./simple-amm.md) — the minimal single-owner AMM.
  one pool view, and creating a pool, adding and removing liquidity.
- [minimal.md](./minimal.md) — the no-op dex. no views, and a `swap`
  that only succeeds when the input and output asset are the same.

---

#### SOURCE

- https://github.com/INTEARnear/dex

`src/lib.rs` is the engine, `dexes/xyk`, `dexes/otc`,
`dexes/simple-amm` and `dexes/minimal` are the sub-dexes, and
`intear-dex-types` holds the shared `AssetId`, `DexId` and
`AccountOrDexId` types.

the deployed mainnet WASM was built from commit
`fd30f501ec27dbc3239e1529b4ce1d8cf5b5ecff`, version `0.1.0`. check
the engine's `contract_source_metadata` view for the commit the
*current* deployment was built from.

```sh
# the authoritative method list of the engine
near contract inspect dex.intear.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
