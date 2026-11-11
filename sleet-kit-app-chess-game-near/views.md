# app.chess-game.near — views

the read methods of the on-chain chess contract.

a view is a read: free, no signer needed, no deposit, no gas.

see [SKILL.md](./SKILL.md) for the overview and the other files.

---

CONTRACT ID PER NETWORK

- MAINNET: app.chess-game.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Substitute the caller's own values — an agent must never
> reuse the example account id or game id as though it were the user's
> intent.

---

#### THE GAME ID

a game is identified by a `GameId`, a tuple of three values:

```json
[128903456, "player1.near", "player2.near"]
```

- the first value is the block height the game was created at
- the second is the white player, always an account id
- the third is the black player, or `null` for a game against the ai

a game against the ai is therefore
`[128903456, "player1.near", null]`.

the api takes the game id as the json-stringified tuple, percent-encoded
into the path.

---

#### GAME STATE

### get_board - VIEW

the board as eight strings, one per rank. row 0 is rank 8, row 7 is
rank 1.

```json
{
  "game_id": [128903456, "player1.near", null]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

the pieces are `KQRBNP` for white and `kqrbnp` for black, a space for
an empty square.

### render_board - VIEW

the same board as one formatted string, with unicode pieces and
chessboard squares.

```json
{
  "game_id": [128903456, "player1.near", null]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### game_info - VIEW

the players, whose turn it is, and whether the game carries bets.

```json
{
  "game_id": [128903456, "player1.near", "player2.near"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

the response holds `white` and `black` as a `Human` account id or an
`Ai` difficulty, the `turn_color` of `"White"` or `"Black"`, the
`last_block_height`, and `has_bets`.

### get_game_ids - VIEW

the active games of one account.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### bet_info - VIEW

the bets on a pair of players, and whether they are locked.

```json
{
  "players": ["player1.near", "player2.near"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

see [bets.md](./bets.md) for the full betting picture.

---

#### ACCOUNTS

### get_account - VIEW

the registration, rating, points and stats of one account.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

the response holds `near_amount`, `is_agent`, `points`, `pending_points`,
`elo`, `wins`, `win_streak`, `max_win_streak`, `bets_placed`,
`bets_won`, `wagers_played`, `wager_wins` and `challenges_sent`.

### get_accounts - VIEW

the registered account ids, paginated.

```json
{
  "skip": 0,
  "limit": 100
}
```

- both fields are optional
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_elo_ratings - VIEW

the elo leaderboard, as account id and rating pairs.

```json
{
  "skip": 0,
  "limit": 50
}
```

- both fields are optional
- gas: none — a view call is read-only and free
- attached deposit: 0

### get_elo_ratings_by_ids - VIEW

the elo rating of specific accounts.

```json
{
  "account_ids": ["player1.near", "player2.near"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_ppp_balances_by_ids - VIEW

the ppp point balances of specific accounts.

```json
{
  "account_ids": ["player1.near", "player2.near"]
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_tokens - VIEW

the tokens the contract holds for one account.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_token_amount - VIEW

the balance of one account in one token.

```json
{
  "account_id": "player1.near",
  "token_id": "wrap.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_token_whitelist - VIEW

the token contracts the contract accepts for wagers and bets.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

---

#### CHALLENGES

### get_challenge - VIEW

one challenge by its id.

```json
{
  "challenge_id": "player1.near-vs-player2.near"
}
```

- the challenge id is the two accounts joined with `-vs-`
- gas: none — a view call is read-only and free
- attached deposit: 0

the response holds the `id`, the `challenger`, the `challenged` and the
`wager`, which is `null` for a challenge without a wager.

### get_challenges - VIEW

the open challenges of one account, in one direction or the other.

```json
{
  "account_id": "player1.near",
  "is_challenger": true
}
```

- `is_challenger` is `true` for the challenges the account sent and
  `false` for the ones it received
- gas: none — a view call is read-only and free
- attached deposit: 0

---

#### MATCHMAKING

### is_queued - VIEW

whether an account is in the matchmaking queue.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_matchmaking_queue - VIEW

the matchmaking queue, paginated.

```json
{
  "skip": 0,
  "limit": 50
}
```

- both fields are optional
- gas: none — a view call is read-only and free
- attached deposit: 0

each entry holds the `account_id` and the `wager`, which is `null` for
a queue entry without a wager.

---

#### QUESTS AND ACHIEVEMENTS

### get_quest_list - VIEW

the quests the contract tracks.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_quest_cooldowns - VIEW

the quest cooldowns of one account.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_achievement_list - VIEW

every achievement the contract tracks.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_achievements - VIEW

the achievements one account unlocked.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

---

#### FEES AND OWNERSHIP

### get_fees - VIEW

the treasury fee, in basis points.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

the fee is taken from the losing side of a settled bet.

### get_treasury_tokens - VIEW

the token balances the treasury holds.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### get_owner - VIEW

the owner account id.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

---

#### STORAGE

### storage_balance_bounds - VIEW

the minimum and maximum storage balance.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### storage_balance_of - VIEW

the storage balance of one account.

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

---

#### THE PPP TOKEN

the contract is also a non-transferable ppp token, with the standard
`ft-1.0.0` interface.

### ft_metadata - VIEW

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

the symbol is `PPP`, the decimals are `6`, and the name is
`Protocol Pawns Points`.

### ft_balance_of - VIEW

```json
{
  "account_id": "player1.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

### ft_total_supply - VIEW

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

---

#### CONTRACT METADATA

### contract_source_metadata - VIEW

the metadata the contract publishes about its own build.

```json
{}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near
