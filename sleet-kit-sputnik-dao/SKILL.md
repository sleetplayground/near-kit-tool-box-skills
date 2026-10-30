---
name: sleet-kit-sputnik-dao
description: Documents the Sputnik DAO V2 contracts - the factories sputnik-dao.near on mainnet and sputnikovv2.testnet on testnet, and a dao itself at <name>.sputnik-dao.near or <name>.sputnikv2.testnet - the contract ids, the difference between factory and dao, and each method's call type (VIEW or CALL) and argument shape. Use this skill whenever the user mentions sputnik-dao, Sputnik DAO, a dao account like huggies.sputnik-dao.near, get_policy, get_proposals, add_proposal, get_dao_list, or a DAO treasury - including when they want to call or inspect these contracts and need the method list, the argument names, or which account is the factory and which is a dao.
---

# Sputnik DAO

the Sputnik DAO contracts — the **factory** that deploys the daos, and
the **daos** themselves.

there are two kinds of account and they are not interchangeable.

the **factory** is `sputnik-dao.near` on mainnet and
`sputnikv2.testnet` on testnet. it deploys the daos and keeps the list
of them. it has no dao state of its own.

a **dao** is `<name>.sputnik-dao.near` on mainnet and
`<name>.sputnikv2.testnet` on testnet — `huggies.sputnik-dao.near`,
`onsocial.sputnik-dao.near`, `sleet.sputnik-dao.near`,
`web3stick.sputnik-dao.near`.

this skill covers both. each has its own file:

| component | file                       | mainnet deployed                | testnet deployed                 |
| --------- | -------------------------- | ------------------------------- | -------------------------------- |
| factory   | [factory.md](./factory.md) | yes — `sputnik-dao.near`        | yes — `sputnikv2.testnet`        |
| dao       | [dao.md](./dao.md)         | yes — `<name>.sputnik-dao.near` | yes — `<name>.sputnikv2.testnet` |

---

CONTRACT ID PER NETWORK

- MAINNET: sputnik-dao.near (factory), `<name>.sputnik-dao.near` (a dao)
- TESTNET: sputnikv2.testnet (factory), `<name>.sputnikv2.testnet` (a dao)

> #### THE FACTORY IS NOT A DAO
>
> calling the factory `sputnik-dao.near` as if it were a dao fails with
> `MethodResolveError(MethodNotFound)` for **every** method, because
> none of the dao methods exist there. the error says "method not
> found", not "wrong contract".
>
> the factory and the dao are also different contracts with different
> versions. the mainnet factory publishes five builds of the dao
> contract — `[2,0]`, `[2,1]`, `[3,0]`, `[3,1]` and `[3,2]` — and a dao
> can be pinned to an older one. the testnet factory publishes seven
> builds — `[2,0]`, `[2,1]`, `[3,0]`, `[3,1]`, `[3,2]`, `[3,3]` and
> `[3,31]`.
>
> > #### A DAO'S `version` IS NOT ITS BUILD
> >
> > the dao answers `version` with the **crate version** of the build it
> > runs, e.g. `2.3.1`. that string does **not** identify the build: a
> > dao on the `[3,1]` build also answers `2.3.1`. to know the build,
> > read the dao's **code hash** and match it against the factory's
> > `get_contracts_metadata` pairs. `near contract inspect <dao>` prints
> > the code hash. the presence of `get_actions_log` or
> > `contract_source_metadata` also identifies the `[3,2]` build.

> #### THE ARGS ARE JSON
>
> the dao contract is `#[near(serializers=[borsh, json])]` and
> near-sdk's json mode is what a view call uses, so the args are **JSON**
> and the numbers are **JSON numbers**.
>
> sending the numbers as borsh strings panics with
> `Failed to deserialize input from JSON`. the factory's `get_daos`
> panics with `invalid type: string "0", expected u64` for the same
> reason.

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument _shape_ only, and
> substitute the caller's own values — an agent must never reuse the
> example account id as though it were the user's intent. A `Transfer`
> proposal moves real funds.

> #### SOURCE
>
> the dao contract is `sputnikdao2`, from `near-daos/sputnik-dao-contract`.
> the factory's `get_contracts_metadata` reports each stored DAO build's
> version, source commit, and changelog URL.
>
> ```sh
> near contract inspect sputnik-dao.near network-config mainnet now
> near contract inspect huggies.sputnik-dao.near network-config mainnet now
> near contract inspect sputnikovv2.testnet network-config testnet now
> near contract inspect genesis.sputnikv2.testnet network-config testnet now
> ```
>
> > #### THE FACTORIES DIFFER
> >
> > the mainnet and testnet factories are different deployments with
> > different method sets. the mainnet factory has
> > `contract_source_metadata` and `store`; the testnet factory has
> > `store_contract_self`, `remove_contract_self` and
> > `on_remove_contract_self`, and no `contract_source_metadata`.
>
> > #### SOME METHODS TAKE RAW BYTES
> >
> > the factory's `store`, the DAO's `store_blob`, and the DAO's
> > `update` take their input as **raw bytes**, not JSON. they do not
> > appear in the contract's ABI, so `near contract inspect` shows no
> > argument shape for them.

---

#### THE MEMBERS

there is **no** method that lists every member of a dao. the policy
only holds role -> member lists, and the roles that are open to
everyone name no accounts at all.

so the member list is the union of the roles' member lists, and an
account in three roles is **one** member, not three. summing the
roles' member counts reads 6 for a dao with 2 people.

---

#### TYPES

a role's `kind` is an enum with three shapes:

- `{ "Group": ["a.near", "b.near"] }` — the accounts in the role
- `"Everyone"` — a bare string, meaning any account may act in the
  role. it names no accounts, so it has no member list
- `{ "Member": "1000000000000000000000000" }` — accounts holding at
  least this much delegated balance

a proposal's `kind` is an enum, one of:

`ChangeConfig` `ChangePolicy` `AddMemberToRole` `RemoveMemberFromRole`
`FunctionCall` `UpgradeSelf` `UpgradeRemote` `Transfer`
`SetStakingContract` `AddBounty` `BountyDone` `Vote`
`FactoryInfoUpdate` `ChangePolicyAddOrUpdateRole`
`ChangePolicyRemoveRole` `ChangePolicyUpdateDefaultVotePolicy`
`ChangePolicyUpdateParameters`

a proposal's `status` is one of:

`InProgress` `Approved` `Rejected` `Removed` `Expired` `Moved` `Failed`

a proposal's `vote_counts` maps a role name to `[yes, no, spam]`, as
strings. `votes` maps an account id to `Approve`, `Reject`, `Remove` or
`Unknown`.

all 128-bit amounts are decimal strings in yocto units.

> #### A `FunctionCall` PROPOSAL
>
> a `FunctionCall` proposal names the contract to call and the methods
> to call on it:
>
> ```json
> {
>   "FunctionCall": {
>     "receiver_id": "target.contract.near",
>     "actions": [
>       {
>         "method_name": "some_method",
>         "args": "eyJmb28iOiJiYXIifQ==",
>         "deposit": "0",
>         "gas": "30000000000000"
>       }
>     ]
>   }
> }
> ```
>
> `args` are the target method's arguments as **base64-encoded bytes**,
> `deposit` and `gas` are decimal strings in yoctoNEAR and yoctoGas.
>
> the policy label for this proposal kind is `call`, not
> `FunctionCall` — a role needs `call:*` or `call:VoteApprove` before
> the proposal can even be created.
>
> the DAO only runs the call after the proposal is approved, and the
> approval lands in the `on_proposal_callback` receipt — not in the
> receipt of the vote that triggered it.

==========================
<br/>
copyright 2026 by sleet.near
