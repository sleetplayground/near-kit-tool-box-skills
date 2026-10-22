---
name: sleet-kit-near
description: Documents the near and testnet top-level-domain registrar / linkdrop contracts - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL) and exact argument shape. Use this skill whenever the user mentions the near TLD registrar, testnet TLD, linkdrop, create_account on a TLD, claim, or new_account creation via the registrar - including when they want to call or inspect these contracts and need the method list, argument names, or network-specific contract id.
---

# near / testnet (TLD registrar + linkdrop)

the `near` (mainnet) and `testnet` (testnet) top-level-domain
registrar / linkdrop contracts — same `near-linkdrop` source compiled
for each network.

---

CONTRACT ID PER NETWORK

- MAINNET: near
- TESTNET: testnet

### get_key_balance - VIEW

```json
{
  "public_key": "ed25519:..."
}
```

### get_key_information - VIEW

```json
{
  "public_key": "ed25519:..."
}
```

### create_account - CALL

```json
{
  "new_account_id": "sleet.near",
  "new_public_key": "ed25519:..."
}
```

### create_account_advanced - CALL

- arg format not documented yet

### create_account_and_claim - CALL

- private method — not documented yet

### send - CALL

- arg format not documented yet

### claim - CALL

- private method — not documented yet

### contract_source_metadata - VIEW

- no args necessary — mainnet only

### on_account_created - CALL

- private callback — not documented yet

### on_account_created_and_claimed - CALL

- private callback — not documented yet

### new - CALL

- private initializer — not documented yet

==========================
<br/>
copyright 2026 by sleet.near