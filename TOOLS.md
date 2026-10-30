# TOOLS

tools used when working with the contracts in this repo.

---

#### NEAR KIT TOOL BOXES

the near kit tool boxes are the source for every argument shape,
gas value, and attached deposit documented in these skills.

**RUST**

-🐱 GIT: https://github.com/sleet-rs/near-kit-tool-box-rs

`src/fun` holds the reusable functions, `src/bin` the example bins,
`src/lib/contract` the typed contracts, `src/lib/methods` the method
name constants, `src/lib/const_id` the contract id constants, and
`src/lib/types` the shared response and data shapes.

**TS**

-🐱 GIT: https://github.com/sleet-js/near-kit-tool-box-ts

`packages/fun` holds the reusable functions, with the method
constants, contract id constants, and shared types alongside them.

> #### SOURING
>
> A skill documents an argument format when a fun helper builds and
> sends that exact JSON. A typed contract is weaker evidence — it is
> hand-written and has been wrong once — so anything taken from one is
> marked as unconfirmed and should be checked against `near contract
inspect` before use. Anything with neither is recorded as **not
> documented yet** rather than guessed.
>
> > A fun helper proves the **shape** of the arguments, not that the
> > method exists. near-sdk only exports methods inside a `#[near]` impl
> > block — a plain `pub fn` on the contract is not callable, so check
> > the contract's own method list before documenting a method as
> > callable.

---

#### INTEAR DEX SOURCE

the INTEAR DEX skill is sourced from the contract's own source
repository, because the sub-dexes have no account of their own to
inspect — they run as wasm inside the engine.

-🐱 GIT: https://github.com/INTEARnear/dex

`src/lib.rs` is the engine, `dexes/xyk`, `dexes/otc`,
`dexes/simple-amm` and `dexes/minimal` are the sub-dexes, and
`intear-dex-types` holds the shared `AssetId`, `DexId` and
`AccountOrDexId` types.

> #### BORSH
>
> the engine's own views are JSON. a **sub-dex** view is reached
> through the engine's `dex_view`, which takes the sub-dex's
> **borsh**-encoded arguments as base64 and answers with a base64
> blob of borsh-encoded bytes. the `dex_view` envelope is
> `{ "dex_id": "<deployer>/<id>", "method": "<name>", "args": "<base64>" }`.
>
> the deployed mainnet wasm was built from commit
> `fd30f501ec27dbc3239e1529b4ce1d8cf5b5ecff`. check the engine's
> `contract_source_metadata` view for the commit the current
> deployment was built from.

---

#### NEAR CLI

inspect a live contract for its authoritative method list and
signatures. This is the fallback whenever a skill says an argument
format is not documented yet.

```sh
# mainnet
near contract inspect v2.ref-finance.near network-config mainnet now
near contract inspect wrap.near network-config mainnet now
near contract inspect sputnik-dao.near network-config mainnet now
near contract inspect huggies.sputnik-dao.near network-config mainnet now

# testnet
near contract inspect ref-finance-101.testnet network-config testnet now
near contract inspect wrap.testnet network-config testnet now
near contract inspect sputnikovv2.testnet network-config testnet now
near contract inspect genesis.sputnikv2.testnet network-config testnet now
```

substitute the contract id for the one you need. for per-validator
staking pools and per-user lockups, substitute the concrete account id
— `<validator>.pool.near` or `<user>.lockup.near`.

> `near contract inspect` reports the **build that account runs**, not
> the newest build. daos deployed from the same factory can sit on
> different builds, so check each account you intend to call.

> some methods take their input as **raw bytes**, not JSON — the
> Sputnik factory's `store`, the DAO's `store_blob`, and the DAO's
> `update`. they do not appear in the contract's ABI, so
> `near contract inspect` shows no argument shape for them.

---

#### FASTNEAR TRANSACTIONS API SERVER

a read-only JSON API over near transaction, block, and receipt data.
use it to check whether a call actually landed and what happened to it
after submission.

- mainnet: https://tx.main.fastnear.com/
- testnet: https://tx.test.fastnear.com/

the root of either host serves the full endpoint reference with
request bodies and response shapes — read that rather than guessing.

source: https://github.com/fastnear/explorer-api

#### INTEAR API

the base url is `https://api.intea.rs`, no api key. every page of the
docs is also markdown, at its url with `.md` in place of the trailing
slash — <https://docs.intear.tech/api/llms-full.txt> has all of them in
one file.

---

#### DOCS

- NEAR: https://docs.near.org/
- FASTNEAR: https://docs.fastnear.com/
- INTEAR: https://docs.intear.tech/

=====================
<br/>
copyright 2026 by sleet.near
