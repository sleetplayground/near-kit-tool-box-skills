# INTEAR DEX minimal sub-dex

the INTEAR DEX `minimal` sub-dex — a no-op.

it has **no views at all**. its only method is `swap`, which requires
`asset_in` and `asset_out` to be the same asset and returns the
requested amount unchanged as both `amount_in` and `amount_out`.

it exists as the smallest possible example of a dex running inside the
INTEAR DEX engine — useful as a reference for the engine's dex
interface, not as a trading venue.

see [SKILL.md](./SKILL.md) for the overview and the other sub-dexes.

---

CONTRACT ID PER NETWORK

- MAINNET: **not deployed** — no dex id is live
- TESTNET: (none)

the dex id is `<deployer>/<id>`, but no minimal dex has been deployed
on mainnet.

> #### ARG EXAMPLES
>
> This dex has no arguments to copy — the shape below shows the
> engine's swap envelope, whose `message` the minimal dex ignores.
>
> #### BORSH, NOT JSON
>
> the engine's arguments and responses are **borsh** encoded, not
> JSON. they travel through the engine's `dex_view` / `dex_call` as
> base64 blobs.
>
> #### NOT AUDITED
>
> The INTEAR DEX source repository states plainly: *"Not audited."*

---

#### VIEWS

none. the minimal dex defines no view method, so `dex_view` on it
always fails.

---

### swap - CALL

the swap message is borsh encoded and **ignored** by this dex.

the swap is executed through the engine's `swap_simple`, not through
`dex_view` — the engine reserves the name `swap`.

the call fails when `asset_in` and `asset_out` differ — the dex
asserts that they are the same, *"since this dex is a no-op"*.

```json
{
  "dex_id": "<deployer>/<id>",
  "message": "",
  "asset_in": "near",
  "asset_out": "near",
  "amount": { "ExactIn": "1000000000000000000000000" },
  "referrer": null,
  "referral_fees": [],
  "constraint": null
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

---

#### SOURCE

- https://github.com/INTEARnear/dex/tree/main/dexes/minimal

```sh
# the engine's method list — the dex's own methods are not on the
# account, they run inside it
near contract inspect dex.intear.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
