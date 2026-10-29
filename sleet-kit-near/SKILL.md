---
name: sleet-kit-near
description: Documents the near and testnet top-level-domain registrar / linkdrop contracts - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions the near TLD registrar, testnet TLD, linkdrop, create_account on a TLD, claim, or new_account creation via the registrar - including when they want to call or inspect these contracts and need the method list, argument names, or network-specific contract id.
---


# near / testnet (TLD registrar + linkdrop)

the `near` (mainnet) and `testnet` (testnet) top-level-domain
registrar / linkdrop contracts — same `near-linkdrop` source compiled
for each network.

---

CONTRACT ID PER NETWORK

- MAINNET: near
- TESTNET: testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id or public key as though it were the user's intent.
> A wrong public key here hands control of the new account to someone
> else.
>
> #### SOURCING
>
> The arg shapes for `create_account_advanced`, `send`,
> `get_key_balance`, and `get_key_information` are **not yet
> confirmed against a live contract** — check them against
> `near contract inspect near network-config mainnet now` before
> relying on them. This contract names the key field `key` on the two
> views and `public_key` on `send`.

### get_key_balance - VIEW

```json
{
  "key": "ed25519:..."
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_key_information - VIEW

```json
{
  "key": "ed25519:..."
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### create_account - CALL

```json
{
  "new_account_id": "sleet.near",
  "new_public_key": "ed25519:..."
}
```

- gas: 30 Tgas
- attached deposit: 0 — the registrar assigns the new account from the
  caller's balance

### create_account_advanced - CALL

```json
{
  "new_account_id": "sleet.near",
  "options": {
    "full_access_keys": ["ed25519:..."],
    "function_call_keys": [
      {
        "public_key": "ed25519:...",
        "allowance": "250000000000000000000000",
        "receiver_id": "sleet.near",
        "method_names": "ping"
      }
    ],
    "contract_bytes": null
  }
}
```

every field inside `options` is optional

- gas: not documented yet
- attached deposit: not documented yet — this method deposits the
  attached funds onto the new account, so the deposit is the opening
  balance

### send - CALL

```json
{
  "public_key": "ed25519:..."
}
```

allows the given public key to claim the attached balance

- gas: not documented yet
- attached deposit: not documented yet — this method sends a
  linkdrop balance, so the deposit is the amount being sent

### contract_source_metadata - VIEW

- no args necessary — mainnet only
- gas: none — a view call is read-only and free
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near