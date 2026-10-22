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

### get_key_balance - VIEW

```json
{
  "public_key": "ed25519:..."
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_key_information - VIEW

```json
{
  "public_key": "ed25519:..."
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

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### create_account_and_claim - CALL

- private method — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### send - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet — this method sends a
  linkdrop balance, so the deposit is the amount being sent

### claim - CALL

- private method — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### contract_source_metadata - VIEW

- no args necessary — mainnet only
- gas: none — a view call is read-only and free
- attached deposit: 0

### on_account_created - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_account_created_and_claimed - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### new - CALL

- private initializer — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

==========================
<br/>
copyright 2026 by sleet.near