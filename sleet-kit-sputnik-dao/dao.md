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
- TESTNET: `<name>.sputnikv2.testnet`

> a DAO's contract version is independent of the factory's default
> version. read the DAO's own `version` to know which methods it has.

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument _shape_ only, and
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

> this is the crate version of the build, not the factory's
> `[major, minor]` build number — a dao on the `[3,1]` build also
> answers `2.3.1`. read the contract's code hash and match it against
> the factory's `get_contracts_metadata` to know the build.

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

> the page is clamped to the DAO's next proposal id, so a `limit` larger
> than what remains returns fewer rows.

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

returns the DAO's latest 20 actions — `account_id`, `proposal_id`,
`action` and `block_height`

> the actions are `AddProposal`, `RemoveProposal`, `VoteApprove`,
> `VoteReject`, `VoteRemove`, `Finalize` and `MoveToHub`.

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

> the page is clamped to the DAO's next bounty id, so a `limit` larger
> than what remains returns fewer rows.

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

> this is the account balance minus the protocol storage cost and minus
> the bonds locked in proposal and bounty bonds.

### get_locked_storage_amount - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

returns the protocol storage cost the DAO's data occupies, in
yoctoNEAR — `storage_byte_cost × storage_usage`. it is not the DAO's
locked bond balance.

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

returns the `factory_id` that deployed the dao and `auto_update` —
whether the factory is allowed to push an auto-update to this dao. when
`auto_update` is on, a factory-driven update still needs a matching
`UpgradeSelf` proposal that is at least two weeks old.

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

returns `[balance, total]`, the account's delegated voting power and the
DAO's total delegated voting power — the contract does not divide them
into a ratio

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
error`, and the wrong number of bytes panics with `invalid length of
the crypto hash`.

### add_proposal - CALL

```json
{
  "proposal": {
    "description": "Signal",
    "kind": "Vote"
  }
}
```

- gas: up to 100 Tgas is usually sufficient
- attached deposit: exactly the DAO's current `proposal_bond` — the check is equality, so a larger deposit also fails with `ERR_MIN_BOND`
- a `ChangePolicy` proposal whose `policy` is not a `Current` policy → `ERR_INVALID_POLICY`
- a `Transfer` proposal with `token_id` = `""` (NEAR) and a non-null `msg` → `ERR_BASE_TOKEN_NO_MSG`
- a `SetStakingContract` proposal when a staking contract is already set → `ERR_STAKING_CONTRACT_CANT_CHANGE`
- a proposal kind the caller's roles do not permit → `ERR_PERMISSION_DENIED`

> a `FunctionCall` proposal's action `args` are the target method's
> arguments as **base64-encoded bytes**. the toolbox's
> `sputnik_dao_add_function_call_proposal_fun_json` helper takes plain
> JSON and base64-encodes it for you.

### act_proposal - CALL

```json
{
  "id": 0,
  "action": "VoteApprove",
  "proposal": "Vote",
  "memo": null
}
```

- `proposal` is mandatory and must exactly match the proposal kind stored for `id` — otherwise `ERR_WRONG_KIND`
- `action` is one of `RemoveProposal`, `VoteApprove`, `VoteReject`, `VoteRemove`, `Finalize`, or `MoveToHub`
- `AddProposal` is not accepted as an action — `ERR_WRONG_ACTION`
- `memo` is optional and only logged
- voting on a proposal that is not `InProgress` → `ERR_PROPOSAL_NOT_READY_FOR_VOTE`
- voting twice → `ERR_ALREADY_VOTED`
- `Finalize` on a proposal that is neither expired nor failed → `ERR_PROPOSAL_NOT_EXPIRED_OR_FAILED`
- an action the caller's roles do not permit → `ERR_PERMISSION_DENIED`
- `RemoveProposal` deletes the proposal from storage; `MoveToHub` is accepted but is a no-op
- gas: up to 100 Tgas is usually sufficient
- attached deposit: 0

### on_proposal_callback - CALL

```json
{
  "proposal_id": 0
}
```

- private callback invoked by the DAO contract
- asserts exactly one promise result (`ERR_UNEXPECTED_CALLBACK_PROMISES`)
- flips the proposal to `Approved` and returns the bond, or to `Failed`
- not intended to be called by a user

### bounty_claim - CALL

```json
{
  "id": 0,
  "deadline": "3600000000000"
}
```

- `deadline` is a duration in nanoseconds and must not exceed the bounty's `max_deadline`
- attached deposit: exactly the DAO's current `bounty_bond`
- a bounty that is not there → `ERR_NO_BOUNTY`
- claiming more times than the bounty's `times` → `ERR_BOUNTY_ALL_CLAIMED`
- `deadline` greater than the bounty's `max_deadline` → `ERR_BOUNTY_WRONG_DEADLINE`

### bounty_done - CALL

```json
{
  "id": 0,
  "account_id": null,
  "description": "Bounty complete"
}
```

- `account_id` may be null to use the caller; when non-null it names the claim owner
- while the claim is still active the caller must be that owner → `ERR_BOUNTY_DONE_MUST_BE_SELF`
- once the claim has expired, anyone may call it to free the claim slot; no proposal is created and no bond is needed
- an already-completed claim → `ERR_BOUNTY_CLAIM_COMPLETED`
- attached deposit: exactly the DAO's current `proposal_bond`, because this call creates a proposal

### bounty_giveup - CALL

```json
{
  "id": 0
}
```

- attached deposit: 0
- within the DAO's forgiveness period the bond is returned; after it, the bond is forfeited
- the forgiveness period is measured from the claim's `start_time` — the block timestamp at which `bounty_claim` landed

### store_blob - CALL

- the input is the raw blob bytes — a JSON body is stored verbatim as
  the blob, it is not parsed
- the same blob cannot be stored twice — `ERR_ALREADY_EXISTS`
- attached deposit: at least the storage cost for the blob plus 32 bytes
- returns the blob's base58 hash

### update - CALL

- the input is the raw new contract WASM, not JSON
- callable by the account itself, or by its factory when the DAO's
  `auto_update` is on
- when the factory triggers it, the DAO first requires an `UpgradeSelf`
  proposal whose hash matches the incoming code, and that proposal must
  be at least two weeks old — otherwise a new `UpgradeSelf` proposal is
  created and nothing is deployed
- attached deposit: 0

### remove_blob - CALL

```json
{
  "hash": "11111111111111111111111111111111"
}
```

- only the account that stored the blob may remove it
- a hash that is not there → `ERR_NO_BLOB`
- attached deposit: 0
- refunds `storage_byte_cost × (blob_len + 32)` to the original storer

### register_delegation - CALL

```json
{
  "account_id": "alice.near"
}
```

- callable only by the DAO's staking contract
- the DAO must have a staking contract set — otherwise `ERR_NO_STAKING`
- attached deposit: exactly the storage cost for 16 bytes

### delegate - CALL

```json
{
  "account_id": "alice.near",
  "amount": "1000000000000000000000000"
}
```

- callable only by the DAO's staking contract
- the account must already be registered — `ERR_NOT_REGISTERED`
- `amount` is a decimal string of yocto units
- returns `[previous_amount, new_amount, total_delegated_amount]`

### undelegate - CALL

```json
{
  "account_id": "alice.near",
  "amount": "1000000000000000000000000"
}
```

- callable only by the DAO's staking contract
- removing more than the delegated amount → `ERR_INVALID_STAKING_CONTRACT`
- returns `[previous_amount, new_amount, total_delegated_amount]`

### migrate - CALL

- private migration callback invoked during a contract upgrade
- `init(ignore_state)` and `#[private]`
- reads the stored state version and upgrades `V1` → `V2`; a V2 contract
  re-reads its state as-is
- not intended to be called by a user

### new - CALL (init)

```json
{
  "config": {
    "name": "My DAO",
    "purpose": "What the DAO does",
    "metadata": ""
  },
  "policy": ["alice.near", "bob.near"]
}
```

- only the factory calls this to bring a new DAO up
- `policy` as an array is the `Default` policy form — the listed
  accounts become the `council` role
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near
