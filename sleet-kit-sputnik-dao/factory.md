# sputnik-dao.near (the Sputnik DAO factory)

the factory that deploys the Sputnik DAO daos and keeps the list of
them.

the factory has no dao state of its own — it deploys daos and lists
them. calling it as if it were a dao fails with a misleading
`MethodResolveError(MethodNotFound)` for every method.

see [SKILL.md](./SKILL.md) for the overview and the dao contract.

---

CONTRACT ID PER NETWORK

- MAINNET: sputnik-dao.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values.
>
> #### SOURCE
>
> the factory's `contract_source_metadata` view reports version
> `0.2.1`, built from commit
> `2b0f34a3860460f285307ec2ccf36345acbe05a2` of
> `near-daos/sputnik-dao-contract`.
>
> ```sh
> near contract inspect sputnik-dao.near network-config mainnet now
> ```

### get_dao_list - VIEW

- no args necessary
- answers every dao's account id, as a plain array of strings

```json
["genesis.sputnik-dao.near", "codame.sputnik-dao.near", "pulse.sputnik-dao.near"]
```

### get_daos - VIEW

- `from_index` — the dao to start at, a JSON number
- `limit` — how many daos to return, a JSON number

```json
{
  "from_index": 0,
  "limit": 10
}
```

> the numbers are JSON numbers. sending them as strings panics with
> `invalid type: string "0", expected u64`.

### get_number_daos - VIEW

- no args necessary
- answers how many daos the factory has deployed, as a number

### get_default_version - VIEW

- no args necessary
- answers the version the factory deploys new daos with, as a
  `[major, minor]` pair

```json
[3, 2]
```

### get_default_code_hash - VIEW

- no args necessary
- answers the code hash the factory deploys new daos with

```json
"DtHZA1mUmGYeScEaUoUZYQGGFHSMpnjMCvkr5U9hJXuF"
```

### get_contracts_metadata - VIEW

- no args necessary
- answers the versions of the dao contract the factory knows about,
  as `[code_hash, metadata]` pairs

```json
[
  ["8RMeZ5cXDap6TENxaJKtigRYf3n139iHmTRe8ZUNey6N", { "version": [2, 0], "commit_id": "c2cf1553b070d04eed8f659571440b27d398c588", "changelog_url": null }],
  ["DtHZA1mUmGYeScEaUoUZYQGGFHSMpnjMCvkr5U9hJXuF", { "version": [3, 2], "commit_id": "2b0f34a3860460f285307ec2ccf36345acbe05a2", "changelog_url": null }]
]
```

> a dao can be pinned to an older version than the factory's default,
> so the factory's defaults and a dao's own `version` are worth
> reading together.

### get_owner - VIEW

- no args necessary
- answers the account that owns the factory

### contract_source_metadata - VIEW

- no args necessary

### set_owner - CALL

- arg format not documented yet

### set_default_code_hash - CALL

- arg format not documented yet

### get_code - CALL

- arg format not documented yet

### delete_contract - CALL

- arg format not documented yet

### store_contract_metadata - CALL

- arg format not documented yet

### on_create - CALL

- arg format not documented yet
- the callback the factory runs when it deploys a dao

==========================
<br/>
copyright 2026 by sleet.near
