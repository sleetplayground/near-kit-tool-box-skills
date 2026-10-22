---
name: sleet-kit-world-pumpopoly-near
description: Documents the world.pumpopoly.near monopoly-style game contract - its mainnet contract id, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions world.pumpopoly.near, pumpopoly, the monopoly game contract, view_player, view_players, move_player, or elite_move - including when they want to call or inspect this contract and need the method list, argument names, or contract id.
---

# world.pumpopoly.near

the pumpopoly world.pumpopoly near monopoly-style game smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: world.pumpopoly.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id as though it were the player's own.

### view_player - VIEW

```json
{
  "account_id": "sleet.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### view_players - VIEW

```json
{
  "account_ids": ["sleet.near", "narkmeta.near"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### view_market - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### view_lands - VIEW

- arg format not documented yet
- gas: none — a view call is read-only and free
- attached deposit: 0

### view_player_count - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### move_player - CALL

- no args necessary — the contract infers the player from the signer
- gas: 60 Tgas
- attached deposit: 0 unless the contract
  requires gas staking for the move

### elite_move - CALL

```json
{
  "contract_id": "1"
}
```

- gas: 100 Tgas
- attached deposit: 0 unless the contract
  requires gas staking for the move

### build - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### elite_build - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### helicopter - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### taper - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### buy_land - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### sell_land - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### end_sale - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### award_land - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### exchequer_buy_land - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### exchequer_claim_bond - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### mint_nft - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### claim_nft - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### sync_nft - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_player_count - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_player_invites - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_player_invited_by - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### set_player_balance - CALL

- arg format not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### new - CALL

- private initializer — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_elite_build - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_elite_move - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_exchequer_claim_bond - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_buy_tokens - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_nft_export - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### on_mint_request_settlement - CALL

- private callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### nft_on_transfer - CALL

- NEP-177 callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### ft_on_transfer - CALL

- NEP-141 callback — not documented yet
- gas: not documented yet
- attached deposit: not documented yet

### metadata - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near