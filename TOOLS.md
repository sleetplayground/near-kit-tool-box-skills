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
> inspect` before use. Anything with neither is recorded as **not
> documented yet** rather than guessed.

---

#### NEAR CLI

inspect a live contract for its authoritative method list and
signatures. This is the fallback whenever a skill says an argument
format is not documented yet.

```sh
# mainnet
near contract inspect v2.ref-finance.near network-config mainnet now
near contract inspect wrap.near network-config mainnet now

# testnet
near contract inspect ref-finance-101.testnet network-config testnet now
near contract inspect wrap.testnet network-config testnet now
```

substitute the contract id for the one you need. for per-validator
staking pools and per-user lockups, substitute the concrete account id
— `<validator>.pool.near` or `<user>.lockup.near`.

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

---

#### DOCS

- NEAR: https://docs.near.org/
- FASTNEAR: https://docs.fastnear.com/
- INTEAR: https://docs.intear.tech/

=====================
<br/>
copyright 2026 by sleet.near