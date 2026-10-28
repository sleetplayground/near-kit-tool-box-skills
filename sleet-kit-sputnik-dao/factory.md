# sputnik-dao.near (the Sputnik DAO factory)

the factory that deploys the Sputnik DAO daos and keeps the list of
them.

the factory has no dao state of its own — it deploys daos and lists
them. calling it as if it were a dao fails with
`MethodResolveError(MethodNotFound)` for every method.

see [SKILL.md](./SKILL.md) for the shared types.

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
> the factory's `contract_source_metadata` reports version `0.2.1`,
> built from commit `2b0f34a3860460f285307ec2ccf36345acbe05a2` of
> `near-daos/sputnik-dao-contract`.
>
> ```sh
> near contract inspect sputnik-dao.near network-config mainnet now
> ```

### get_dao_list - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns every dao's account id, as an array of strings

### get_daos - VIEW

```json
{
  "from_index": 0,
  "limit": 10
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns a page of daos' account ids

> the numbers are JSON numbers. sending them as strings panics with
> `invalid type: string "0", expected u64`.

### get_number_daos - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns how many daos the factory has deployed

### get_default_version - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the `[major, minor]` version the factory deploys new daos with

> a dao can be pinned to an older version than this one, so the
> factory's default and a dao's own `version` are worth reading
> together.

### get_default_code_hash - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the code hash the factory deploys new daos with

### get_contracts_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the versions of the dao contract the factory knows about, as
`[code_hash, metadata]` pairs — the code hash, the `version`, the
source `commit_id` and the `changelog_url` of each

### get_owner - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the account that owns the factory

### contract_source_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### set_owner - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_default_code_hash - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### get_code - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### delete_contract - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### store_contract_metadata - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_create - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet
- the callback the factory runs when it deploys a dao

==========================
<br/>
copyright 2026 by sleet.near
