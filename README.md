# near-kit-tool-box-skills

personal skill library for interacting with near blockchain and smart contracts.

---

ℹ️ this is developed mainly for internal use.
<br/>
ℹ️ i develop a few tools/apps that interact with different near smart contracts
<br/>
ℹ️ nice to have a clean source of truth

#### THINGS I AM BUILDING

**near-kit-tool-box**
<br/>
tool box for near kit with custom web client and reusable functions
<br/>
🐱 TS: https://github.com/sleet-js/near-kit-tool-box-ts
<br/>
🐱 RUST: https://github.com/sleet-rs/near-kit-tool-box-rs

see more at https://github.com/near-kit

---

#### SKILLS

see [TOOLS.md](./TOOLS.md) for the tool boxes, near cli, and explorers
these skills are sourced from.

- [sleet-kit-hello-sleet-near/SKILL.md](./sleet-kit-hello-sleet-near/SKILL.md)
- [sleet-kit-wrap-near/SKILL.md](./sleet-kit-wrap-near/SKILL.md)
- [sleet-kit-near/SKILL.md](./sleet-kit-near/SKILL.md)
- [sleet-kit-v2-ref-finance-near/SKILL.md](./sleet-kit-v2-ref-finance-near/SKILL.md)
- [sleet-kit-world-pumpopoly-near/SKILL.md](./sleet-kit-world-pumpopoly-near/SKILL.md)
- [sleet-kit-pool-near-factory/SKILL.md](./sleet-kit-pool-near-factory/SKILL.md)
- [sleet-kit-pool-near-staking/SKILL.md](./sleet-kit-pool-near-staking/SKILL.md)
- [sleet-kit-ft/SKILL.md](./sleet-kit-ft/SKILL.md)
- [sleet-kit-hos-venear-dao/SKILL.md](./sleet-kit-hos-venear-dao/SKILL.md)
- [sleet-kit-hos-lockup/SKILL.md](./sleet-kit-hos-lockup/SKILL.md)
- [sleet-kit-hos-vote-dao/SKILL.md](./sleet-kit-hos-vote-dao/SKILL.md)
- [sleet-kit-social-near/SKILL.md](./sleet-kit-social-near/SKILL.md)
- [sleet-kit-rhea-dclv2-ref-labs-near/SKILL.md](./sleet-kit-rhea-dclv2-ref-labs-near/SKILL.md)
  - [views.md](./sleet-kit-rhea-dclv2-ref-labs-near/views.md)
  - [calls.md](./sleet-kit-rhea-dclv2-ref-labs-near/calls.md)
  - [ft_transfer_call.md](./sleet-kit-rhea-dclv2-ref-labs-near/ft_transfer_call.md)
  - [multi.md](./sleet-kit-rhea-dclv2-ref-labs-near/multi.md)
  - [recipes.md](./sleet-kit-rhea-dclv2-ref-labs-near/recipes.md)
- [sleet-kit-nft-pumpopoly-near/SKILL.md](./sleet-kit-nft-pumpopoly-near/SKILL.md)
- [sleet-kit-exchequer-pumpopoly-near/SKILL.md](./sleet-kit-exchequer-pumpopoly-near/SKILL.md)
- [sleet-kit-berryclub-ek-near/SKILL.md](./sleet-kit-berryclub-ek-near/SKILL.md)
- [sleet-kit-linear-protocol-near/SKILL.md](./sleet-kit-linear-protocol-near/SKILL.md)
- [sleet-kit-meta-pool-near/SKILL.md](./sleet-kit-meta-pool-near/SKILL.md)
- [sleet-kit-meme-cooking-near/SKILL.md](./sleet-kit-meme-cooking-near/SKILL.md)
- [sleet-kit-app-chess-game-near/SKILL.md](./sleet-kit-app-chess-game-near/SKILL.md)
- [sleet-kit-intear-dex/SKILL.md](./sleet-kit-intear-dex/SKILL.md)
  - [engine.md](./sleet-kit-intear-dex/engine.md)
  - [xyk.md](./sleet-kit-intear-dex/xyk.md)
  - [otc.md](./sleet-kit-intear-dex/otc.md)
  - [simple-amm.md](./sleet-kit-intear-dex/simple-amm.md)
  - [minimal.md](./sleet-kit-intear-dex/minimal.md)
- [sleet-kit-sputnik-dao/SKILL.md](./sleet-kit-sputnik-dao/SKILL.md)
  - [factory.md](./sleet-kit-sputnik-dao/factory.md)
  - [dao.md](./sleet-kit-sputnik-dao/dao.md)

> `pool.near` is split into two skills — `pool-near-factory` is the
> factory that deploys validator pools, `pool-near-staking` is the
> per-validator staking pool child contract itself.
>
> The house-of-stake set follows the same shape — `hos-venear-dao`
> deploys each user's `hos-lockup` sub-account, and `hos-vote-dao` is
> its own contract.
>
> The INTEAR DEX is one skill with a file per component —
> `intear-dex/engine.md` is the host contract `dex.intear.near`, and
> each sub-dex that runs inside it gets its own file:
> `intear-dex/xyk.md` (the deployed AMM), `intear-dex/otc.md`,
> `intear-dex/simple-amm.md` and `intear-dex/minimal.md`.
> Only the xyk sub-dex is deployed on mainnet, under the dex id
> `slimedragon.near/xyk`.
>
> The Sputnik DAO set follows the same shape — `sputnik-dao/factory.md`
> is the factory `sputnik-dao.near` on mainnet and
> `sputnikv2.testnet` on testnet, and `sputnik-dao/dao.md` is a dao
> itself at `<name>.sputnik-dao.near` or `<name>.sputnikv2.testnet`.
> The two are not interchangeable: calling the factory as if it were a
> dao fails with a misleading `MethodNotFound` for every method.
>
> Skills marked with an **ARG AND GAS** note have no verified fun
> helper yet, so they list methods by name and call type only.

=====================
<br/>
copyright 2026 by sleet.near
