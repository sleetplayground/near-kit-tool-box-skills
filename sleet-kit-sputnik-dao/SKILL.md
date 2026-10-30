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
> can be pinned to an older one. the testnet factory publishes more
> builds. read a dao's own `version` to know which build it runs.

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
> ```

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

==========================
<br/>
copyright 2026 by sleet.near
