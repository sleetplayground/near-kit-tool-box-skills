# near-kit-tool-box-skills

personal skill library for interacting with near blockchain and smart contracts.

this repo is a part of near kit tool box project
- [TS tool box](https://github.com/sleet-js/near-kit-tool-box-ts) - the typescript tool box
- [RUST tool box](https://github.com/sleet-rs/near-kit-tool-box-rs) - the rust tool box
- [SKILLS](https://github.com/sleetplayground/near-kit-tool-box-skills) - this repo, the skill library

---

#### USING

each skill is a folder named `sleet-kit-{contract}` holding a `SKILL.md` with the contract id per network and each method's call type, argument shape, gas and attached deposit.

these skills are sourced from the tool boxes, the near cli and the explorers, see [TOOLS.md](./TOOLS.md) for those sources.

this is meant as a general guide. always verify that the methods and args are right yourself. if you find anything wrong with any of the notes/skills please help by contributing. i want this place to contain accurate information.

---

#### CONTRIBUTING

- see [README.md](./README.md) for the full list of skills
- see [TOOLS.md](./TOOLS.md) for the tool boxes, near cli and explorers the skills are sourced from
- verify methods and args against `near contract inspect` before writing them down
- keep the two tool boxes in sync when a contract method changes

=====================
<br/>
copyright 2026 by sleet.near
