# <name>.sputnik-dao.near (a Sputnik DAO)

a single Sputnik DAO — the config, the policy, the proposals and the
bounties of one dao.

a dao's account id is `<name>.sputnik-dao.near`, e.g.
`huggies.sputnik-dao.near`, `onsocial.sputnik-dao.near`,
`sleet.sputnik-dao.near`.

> #### THE TRAP
>
> the factory `sputnik-dao.near` is **not** a dao. calling it as if it
> were one fails with a misleading `MethodResolveError(MethodNotFound)`
> for every method, because none of these methods exist there.
>
> see [SKILL.md](./SKILL.md) for the overview and the factory.

see [SKILL.md](./SKILL.md) for the shared shapes and gotchas.

---

CONTRACT ID PER NETWORK

- MAINNET: `<name>.sputnik-dao.near`
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values. A `Transfer` proposal moves real
> funds — never reuse the example values as though they were the
> user's intent.
>
> #### SOURCE
>
> the dao contract is `sputnikdao2`. its source is
> `near-daos/sputnik-dao-contract`. a dao answers `version` with the
> build it runs — `2.3.1` for the current one — and the factory
> publishes five builds: `[2,0]`, `[2,1]`, `[3,0]`, `[3,1]` and
> `[3,2]`.
>
> ```sh
> near contract inspect huggies.sputnik-dao.near network-config mainnet now
> ```

### CONFIG AND POLICY

#### get_config - VIEW

- no args necessary
- answers the dao's name, purpose and metadata

```json
{ "name": "HUGGIES", "purpose": "managing digital assets", "metadata": "<base64 json>" }
```

#### get_policy - VIEW

- no args necessary
- answers the dao's roles, who is in them, and what they may do

```json
{
  "roles": [
    {
      "name": "Requestor",
      "kind": { "Group": ["sleet.near", "huggies.near"] },
      "permissions": ["call:VoteRemove", "transfer:VoteRemove", "call:AddProposal"]
    }
  ]
}
```

#### version - VIEW

- no args necessary
- answers the dao contract's version, e.g. `2.3.1`

#### contract_source_metadata - VIEW

- no args necessary

### PROPOSALS

#### get_proposals - VIEW

- `from_index` — the proposal to start at, a JSON number
- `limit` — how many proposals to return, a JSON number

```json
{
  "from_index": 0,
  "limit": 100
}
```

> a dao with no proposals answers with an empty list. on some rpc
> endpoints it answers with a wasm failure that carries no result
> field instead — read "no result" as "no proposals", not as a
> failure.

#### get_proposal - VIEW

- `id` — the proposal's id, a JSON number

```json
{
  "id": 0
}
```

> the arg is `id`. calling it with `proposal_id` panics with
> `missing field 'id'`.
>
> a proposal that is not there panics with `ERR_NO_PROPOSAL` — that is
> what a removed proposal looks like, since the dao keeps the id in
> its counter and the record in the actions log but drops the proposal
> itself.

#### get_last_proposal_id - VIEW

- no args necessary
- answers the id the dao's **next** proposal will carry

> this is the next id, not the count of the proposals. the contract
> starts it at 0 and bumps it on every add, so a dao whose proposals
> were all removed still reports a non-zero id.

#### get_actions_log - VIEW

- no args necessary
- answers every action the dao has recorded — who did what, to which
  proposal, and in which block

```json
[
  { "account_id": "sleet.near", "proposal_id": "0", "action": "AddProposal", "block_height": "189585245" }
]
```

> a proposal is not a transaction. the contract stores the proposal's
> record and this trail of actions, but it does not store the
> transaction hashes.

### BOUNTIES

#### get_bounties - VIEW

- `from_index` — the bounty to start at, a JSON number
- `limit` — how many bounties to return, a JSON number

```json
{
  "from_index": 0,
  "limit": 100
}
```

> the amount is in the token's own smallest unit and the contract does
> not say how many decimals that is. NEAR is 24 decimals and most
> other tokens are 18, so read the decimals from the token registry —
> never from a guess. an empty `token_id` means NEAR.

#### get_bounty - VIEW

- `id` — the bounty's id, a JSON number

```json
{
  "id": 0
}
```

> the arg is `id`. calling it with `bounty_id` panics with
> `missing field 'id'`.
>
> a bounty that is not there panics with `ERR_NO_BOUNTY`.

#### get_bounty_claims - VIEW

- `account_id` — the account whose claims to read

```json
{
  "account_id": "sleet.near"
}
```

> this is the **only** arg. the contract ignores any other field.

#### get_bounty_number_of_claims - VIEW

- `id` — the bounty's id, a JSON number

```json
{
  "id": 0
}
```

#### get_last_bounty_id - VIEW

- no args necessary
- answers the id the dao's **next** bounty will carry

### BALANCES AND STAKING

#### get_available_amount - VIEW

- no args necessary
- answers the NEAR the dao can spend, in yoctoNEAR, as a string

```json
"68302660735259599999998"
```

#### get_locked_storage_amount - VIEW

- no args necessary
- answers the NEAR the dao holds for storage, in yoctoNEAR, as a
  string — the money it cannot spend

```json
"34700000000000000000000"
```

#### get_staking_contract - VIEW

- no args necessary
- answers the staking contract the dao uses, or an empty string when
  it has none

```json
""
```

#### get_factory_info - VIEW

- no args necessary
- answers the factory that deployed the dao and whether it follows the
  factory's default code

```json
{ "factory_id": "sputnik-dao.near", "auto_update": true }
```

### DELEGATION

#### delegation_balance_of - VIEW

- `account_id` — the account whose delegated voting power to read

```json
{
  "account_id": "sleet.near"
}
```

- answers the delegated voting power, as a string

#### delegation_balance_ratio - VIEW

- `account_id` — the account whose ratio to read

```json
{
  "account_id": "sleet.near"
}
```

- answers `[balance, ratio]`, both as strings

```json
["0", "0"]
```

#### delegation_total_supply - VIEW

- no args necessary
- answers the total delegated voting power the dao has, as a string

### BLOBS

#### has_blob - VIEW

- `hash` — the blob's 32-byte crypto hash, in **base58**

```json
{
  "hash": "11111111111111111111111111111111"
}
```

> base58, not base64. a base64 hash panics with `base58 decoding
> error`, and the wrong number of bytes panics with `invalid length of
> the crypto hash`.

### CALLS

> the arg formats for the call methods are **not documented yet**.
> treat them as a starting point, not a specification. get the
> authoritative signatures before using any of them:
>
> ```sh
> near contract inspect huggies.sputnik-dao.near network-config mainnet now
> ```

#### add_proposal - CALL

- arg format not documented yet

#### act_proposal - CALL

- arg format not documented yet

#### on_proposal_callback - CALL

- arg format not documented yet

#### bounty_claim - CALL

- arg format not documented yet

#### bounty_done - CALL

- arg format not documented yet

#### bounty_giveup - CALL

- arg format not documented yet

#### store_blob - CALL

- arg format not documented yet

#### remove_blob - CALL

- arg format not documented yet

#### register_delegation - CALL

- arg format not documented yet

#### migrate - CALL

- arg format not documented yet

==========================
<br/>
copyright 2026 by sleet.near
