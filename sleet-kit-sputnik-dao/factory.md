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
- TESTNET: sputnikv2.testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument _shape_ only, and
> substitute the caller's own values.
>
> #### SOURCE
>
> the factory's `get_contracts_metadata` reports each stored DAO build's
> version, source commit, and changelog URL.
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

### set_owner - CALL

```json
{
  "owner_id": "alice.near"
}
```

- owner only
- attached deposit: 0

### set_default_code_hash - CALL

```json
{
  "code_hash": "11111111111111111111111111111111"
}
```

- owner only
- the hash must already be stored
- attached deposit: 0

### get_code - VIEW

```json
{
  "code_hash": "11111111111111111111111111111111"
}
```

- returns raw WASM bytes, not JSON

### delete_contract - CALL

```json
{
  "code_hash": "11111111111111111111111111111111"
}
```

- owner only
- the default contract cannot be deleted
- attached deposit: 0

### store - CALL

- the input is the raw DAO contract WASM, not JSON
- owner only
- attached deposit: at least 11 times the contract's storage cost

### store_contract_metadata - CALL

```json
{
  "code_hash": "11111111111111111111111111111111",
  "metadata": {
    "version": [3, 2],
    "commit_id": "2b0f34a3860460f285307ec2ccf36345acbe05a2",
    "changelog_url": null
  },
  "set_default": true
}
```

- owner only
- the code hash must already be stored
- attached deposit: 0

### delete_contract_metadata - CALL

```json
{
  "code_hash": "11111111111111111111111111111111"
}
```

- owner only
- attached deposit: 0

### create - CALL

```json
{
  "name": "my-dao",
  "args": "<base64-encoded DAO new() JSON>"
}
```

- attached deposit: at least 10 mNEAR; more is needed to fund the new DAO
- the decoded `args` are `{ "config": { ... }, "policy": [ ... ] }`
- creation is asynchronous; the DAO is listed after the `on_create` callback succeeds

### update - CALL

```json
{
  "account_id": "my-dao.sputnik-dao.near",
  "code_hash": "11111111111111111111111111111111"
}
```

- callable by the factory owner or the DAO itself
- the DAO must have been created by this factory
- attached deposit: 0

### on_create - CALL

- private callback the factory runs when it deploys a DAO
- not intended to be called by a user

==========================
<br/>
copyright 2026 by sleet.near
