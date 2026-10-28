# <name>.sputnik-dao.near (a Sputnik DAO)

a single Sputnik DAO — the config, the policy, the proposals and the
bounties of one dao.

a dao's account id is `<name>.sputnik-dao.near`, e.g.
`huggies.sputnik-dao.near`, `onsocial.sputnik-dao.near`,
`sleet.sputnik-dao.near`.

see [SKILL.md](./SKILL.md) for the shared types and the factory.

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
> the dao contract is `sputnikdao2`, from `near-daos/sputnik-dao-contract`.
> a dao answers `version` with the build it runs — `2.3.1` for the
> current one.
>
> ```sh
> near contract inspect huggies.sputnik-dao.near network-config mainnet now
> ```

### get_config - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the dao's `name`, `purpose` and `metadata`. the metadata is
base64-encoded JSON — it decodes to fields like `{ primaryColor,
flagLogo }`, the dao's own site branding.

### get_policy - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the dao's `roles`, each with a `name`, a `kind`, the
`permissions` the role grants and its `vote_policy`.

### version - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the dao contract's version, e.g. `2.3.1`

### contract_source_metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_proposals - VIEW

```json
{
  "from_index": 0,
  "limit": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns a page of proposals

> a dao with no proposals answers with an empty list. on some rpc
> endpoints it answers with a wasm failure that carries no result field
> instead — read "no result" as "no proposals", not as a failure.

### get_proposal - VIEW

```json
{
  "id": 0
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns one proposal

> the arg is `id`. calling it with `proposal_id` panics with
> `missing field 'id'`.
>
> a proposal that is not there panics with `ERR_NO_PROPOSAL` — that is
> what a removed proposal looks like, since the dao keeps the id in
> its counter and the record in the actions log but drops the proposal
> itself.

### get_last_proposal_id - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the id the dao's **next** proposal will carry

> this is the next id, not the count of the proposals. the contract
> starts it at 0 and bumps it on every add, so a dao whose proposals
> were all removed still reports a non-zero id.

### get_actions_log - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns every action the dao has recorded — `account_id`,
`proposal_id`, `action` and `block_height`

> a proposal is not a transaction. the contract stores the proposal's
> record and this trail of actions, but it does not store the
> transaction hashes.

### get_bounties - VIEW

```json
{
  "from_index": 0,
  "limit": 100
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns a page of bounties

> the amount is in the token's own smallest unit and the contract does
> not say how many decimals that is. NEAR is 24 decimals and most
> other tokens are 18, so read the decimals from the token registry —
> never from a guess. an empty `token_id` means NEAR.

### get_bounty - VIEW

```json
{
  "id": 0
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns one bounty

> the arg is `id`. calling it with `bounty_id` panics with
> `missing field 'id'`.
>
> a bounty that is not there panics with `ERR_NO_BOUNTY`.

### get_bounty_claims - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the claims on the dao's bounties

> this is the **only** arg. the contract ignores any other field.

### get_bounty_number_of_claims - VIEW

```json
{
  "id": 0
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns how many times a bounty has been claimed

### get_last_bounty_id - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the id the dao's **next** bounty will carry

### get_available_amount - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR the dao can spend, in yoctoNEAR

### get_locked_storage_amount - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR the dao holds for storage, in yoctoNEAR — the money
it cannot spend

### get_staking_contract - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the staking contract the dao uses, or an empty string when it
has none

### get_factory_info - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the `factory_id` that deployed the dao and whether it follows
the factory's default code

### delegation_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the account's delegated voting power

### delegation_balance_ratio - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns `[balance, ratio]`, the account's delegated voting power and
its share of the dao's total

### delegation_total_supply - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the total delegated voting power the dao has

### has_blob - VIEW

```json
{
  "hash": "11111111111111111111111111111111"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns whether the dao holds a blob with that hash

> base58, not base64. a base64 hash panics with `base58 decoding
> error`, and the wrong number of bytes panics with `invalid length of
> the crypto hash`.

### add_proposal - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### act_proposal - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_proposal_callback - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### bounty_claim - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### bounty_done - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### bounty_giveup - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### store_blob - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### remove_blob - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### register_delegation - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### migrate - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

==========================
<br/>
copyright 2026 by sleet.near
