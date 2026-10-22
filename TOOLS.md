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

-🐱 GIT: https://github.com/sleet-js/near-kit-tool-box

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

a **JSON API, not a web page** — there is nothing to paste a hash into.
every endpoint is `POST`, takes a JSON body, and lives under `/v0`.

- mainnet: https://tx.main.fastnear.com/
- testnet: https://tx.test.fastnear.com/

source: https://github.com/fastnear/explorer-api

this is how an agent checks what actually happened on-chain instead of
guessing. when a call returns a transaction hash, resolve it here to
confirm the transaction completed, read its receipts and logs, and see
whether it succeeded.

> #### USAGE
>
> **check a transaction** — up to 20 hashes per request
>
> ```sh
> curl -X POST https://tx.test.fastnear.com/v0/transactions \
>  -H 'Content-Type: application/json' \
>  -d '{
>   "tx_hashes": [
>    "HV42hanyDVK3MYoW8c17Ufxw83htNEQGk93bzyQRocvS",
>    "D2pt9ceGUyiGdJeeZLDd3uBGHF11DmPe4aJFMaoWMmDd"
>   ]
> }'
> ```
>
> returns `{ "transactions": [ /* raw tx data */ ] }` — check
> `is_success` and read the logs.
>
> **find the transactions of an account** — this is the one to reach for
> when you have an account id but no hash. it is cursor paginated, so
> follow `resume_token` until it is absent.
>
> ```sh
> curl -X POST https://tx.test.fastnear.com/v0/account \
>  -H 'Content-Type: application/json' \
>  -d '{ "account_id": "hello.sleet.testnet", "limit": 50 }'
> ```
>
> filters worth knowing: `is_success`, `is_function_call`,
> `is_event_log`, `is_real_signer`, `is_receiver`,
> `from_tx_block_height` / `to_tx_block_height`, `desc`, `limit`
> (max 200).
>
> **inspect a block** — by height or hash, optionally pulling its
> transactions and receipts
>
> ```sh
> curl -X POST https://tx.test.fastnear.com/v0/block \
>  -H 'Content-Type: application/json' \
>  -d '{ "block_id": 130000000, "with_transactions": true, "with_receipts": true }'
> ```
>
> **other endpoints** — `POST /v0/blocks` for a range of blocks,
> `POST /v0/receipt` to resolve a receipt id back to its transaction.
>
> swap `tx.test.fastnear.com` for `tx.main.fastnear.com` for mainnet.

---

#### DOCS

- NEAR: https://docs.near.org/
- FASTNEAR: https://docs.fastnear.com/
- INTEAR: https://docs.intear.tech/

=====================
<br/>
copyright 2026 by sleet.near