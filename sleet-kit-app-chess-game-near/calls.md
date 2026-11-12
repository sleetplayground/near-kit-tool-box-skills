# app.chess-game.near — calls

the state-changing methods of the on-chain chess contract.

a call mutates state: it needs a signer, it costs gas, and some of them
attach a deposit.

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

---

#### REGISTRATION

### storage_deposit - CALL

registers an account on the contract, or tops up the storage balance of
one that is already registered.

```json
{
  "registration_only": true
}
```

- `account_id` is optional and registers **another** account. omit it
  to register the caller
- `registration_only` is optional and defaults to `false`
- the deposit is the storage cost, at least **0.05 NEAR**. read the
  exact minimum with the `storage_balance_bounds` view
- gas: 30 Tgas
- attached deposit: at least 0.05 NEAR

a new account starts with an elo rating of 1000, no points, and every
stat at zero. registering again adds to the storage balance, it does
not reset anything.

the account must be registered before it can play, challenge, or bet.

### set_is_agent - CALL

marks the account as an automated agent.

```json
{
  "is_agent": true
}
```

- gas: 10 Tgas
- attached deposit: 1 yoctoNEAR

the flag is public and visible to anyone through `get_account`. it does
not change how the contract behaves.

### storage_withdraw - CALL

not implemented — the call fails with `OperationNotSupported`.

```json
{
  "amount": "50000000000000000000000"
}
```

- `amount` is optional
- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

the contract's `internal_storage_withdraw` returns
`ContractError::OperationNotSupported` for every call, so no storage
balance can ever be withdrawn this way.

### storage_unregister - CALL

not implemented — the call panics.

```json
{
  "force": true
}
```

- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

---

#### PLAYING

### create_ai_game - CALL

starts a game against the on-chain ai, as white.

```json
{
  "difficulty": "Easy"
}
```

- `difficulty` is one of `"Easy"`, `"Medium"`, `"Hard"`, `"VeryHard"`
- the caller must be registered
- gas: 50 Tgas for `Easy`, 150 Tgas for `Medium`, 250 Tgas for `Hard`,
  400 Tgas for `VeryHard`
- attached deposit: 0

the response is the `GameId`, with a `null` black player.

### play_move - CALL

plays a move in a game.

```json
{
  "game_id": [128903456, "player1.near", null],
  "mv": "e2e4"
}
```

- `mv` is **coordinate notation**, not san. the accepted forms are
  `"e2e4"`, `"e2 e4"`, `"e2 to e4"`, `"O-O"`, `"O-O-O"` and a promotion
  like `"e7 to e8 Q"`
- gas: 50 Tgas for `Easy`, 150 Tgas for `Medium` or a human game,
  250 Tgas for `Hard`, 400 Tgas for `VeryHard`
- attached deposit: 0

the response is the outcome and the board. the outcome is `null` while
the game runs, or `{"result":"Victory","color":"White"}` and
`{"result":"Stalemate"}` when it ends.

against the ai, the ai replies inside the same transaction.

### resign - CALL

gives up the game.

```json
{
  "game_id": [128903456, "player1.near", "player2.near"]
}
```

- gas: 300 Tgas
- attached deposit: 0

the opponent wins. a resignation can happen on any turn.

### cancel - CALL

cancels a game that has gone stale.

```json
{
  "game_id": [128903456, "player1.near", "player2.near"]
}
```

- gas: 300 Tgas
- attached deposit: 0

a player may cancel their own game after **3 days** of inactivity, and
only when it is not their turn. anyone else may cancel a game after
**14 days** of inactivity.

wagers and bets are refunded on a cancellation.

---

#### CHALLENGES

### challenge - CALL

challenges another account to a game.

```json
{
  "challenged_id": "player2.near"
}
```

- gas: 30 Tgas
- attached deposit: 0

the caller must be registered, and cannot challenge themselves. an
account may hold **25 open challenges**.

to challenge with a **wager**, use an `ft_transfer_call` on the token
instead — see [bets.md](./bets.md).

### accept_challenge - CALL

accepts a challenge, which starts the game.

```json
{
  "challenge_id": "player1.near-vs-player2.near"
}
```

- the challenge id is the two accounts joined with `-vs-`
- gas: 30 Tgas
- attached deposit: 0

the response is the `GameId`.

to accept a **wagered** challenge, use an `ft_transfer_call` on the
token instead — see [bets.md](./bets.md).

### reject_challenge - CALL

declines a challenge.

```json
{
  "challenge_id": "player1.near-vs-player2.near",
  "is_challenger": true
}
```

- `is_challenger` is `true` for the sender of the challenge and `false`
  for the receiver
- gas: 30 Tgas
- attached deposit: 0

either party may reject. a wagered challenge refunds the tokens to the
challenger.

---

#### MATCHMAKING

### join_matchmaking - CALL

joins the matchmaking queue, to be paired with a similarly rated
opponent.

```json
{
  "min_elo": 100,
  "max_elo": 200
}
```

- `min_elo` and `max_elo` are the rating range of the opponent to
  match. the fields are required, and they are floats
- the caller must be registered, and must not already be in a
  challenge or in the queue
- the queue holds **500 entries**
- a queue entry **expires after one hour**, and the expired entries
  are purged lazily — when the queue is next walked
- the elo test is **mutual**: the joiner's band and the queued
  player's band must both accept the other player
- gas: 30 Tgas
- attached deposit: 0

the call takes **no deposit**. it is not payable, so an attached deposit
is rejected.

to join the queue **with a wager**, use an `ft_transfer_call` on the
token instead — that is the path that carries a token deposit — see
[bets.md](./bets.md).

### cancel_matchmaking - CALL

leaves the matchmaking queue.

```json
{}
```

- gas: 10 Tgas
- attached deposit: 0

the storage deposit is refunded, and a wager deposited with the queue
entry is refunded too.

---

#### POINTS

### claim_points - CALL

claims the pending ppp points of the caller.

```json
{}
```

- gas: 30 Tgas
- attached deposit: 0

quest points earned in a wager or a bet flow are held as pending points
and land in the balance after this call.

---

#### TOKENS AND BETS

### cancel_bet - CALL

cancels a bet that has not locked yet.

```json
{
  "players": ["player1.near", "player2.near"],
  "token_id": "wrap.near"
}
```

- gas: 30 Tgas
- attached deposit: 0

see [bets.md](./bets.md) for the betting picture.

### withdraw_token - CALL

withdraws the tokens the contract holds for the caller.

```json
{
  "token_id": "wrap.near"
}
```

- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

the balance is the cancelled bet, a refund or a payout. read it with
the `get_token_amount` view.

### ft_transfer - CALL

the ppp token is **non-transferable** — this call panics.

```json
{
  "receiver_id": "player2.near",
  "amount": "1000000",
  "msg": ""
}
```

- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

### ft_transfer_call - CALL

the standard token receiver call. on this contract it is the **entry
point for every token flow** — a bet, a wagered challenge, a wagered
matchmaking entry. it is called on the **token** contract, with this
contract as the `receiver_id`.

see [bets.md](./bets.md) for the message shapes.

---

#### OWNERSHIP

these are owner-only. the owner is readable with the `get_owner` view.

### set_fees - CALL

sets the treasury fee, in basis points.

```json
{
  "treasury": 150
}
```

- the fee cannot exceed 10000, which is 100%
- gas: 30 Tgas
- attached deposit: 0

### set_token_whitelist - CALL

replaces the list of token contracts accepted for wagers and bets.

```json
{
  "whitelist": ["wrap.near", "token.0xshitzu.near"]
}
```

- gas: 30 Tgas
- attached deposit: 0

read the current list with the `get_token_whitelist` view.

### withdraw_treasury - CALL

withdraws the treasury balance of one token.

```json
{
  "token_id": "wrap.near"
}
```

- gas: 30 Tgas
- attached deposit: 0

read the balance with the `get_treasury_tokens` view.

### transfer_ownership - CALL

hands the contract to another account.

```json
{
  "new_owner_id": "player2.near"
}
```

- gas: 30 Tgas
- attached deposit: 0

### pause - CALL

stops the contract from taking new calls.

```json
{}
```

- gas: 30 Tgas
- attached deposit: 0

### resume - CALL

starts the contract again after a pause.

```json
{}
```

- gas: 30 Tgas
- attached deposit: 0

---

#### DEPLOYMENT

### new - CALL

initializes a fresh contract. it is the constructor, and it is called
once at deploy time.

```json
{
  "owner_id": "player1.near"
}
```

- gas: 300 Tgas
- attached deposit: 0

### migrate - CALL

migrates the contract to the current state version.

```json
{}
```

- gas: 300 Tgas
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near
