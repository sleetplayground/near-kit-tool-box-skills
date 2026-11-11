# app.chess-game.near — bets

the spectator betting side of the on-chain chess contract.

these are **not methods on this contract**. placing a bet is a
standard `ft_transfer_call` made **on the token contract** — `wrap.near`,
`token.0xshitzu.near`, or any other whitelisted FT — with
`app.chess-game.near` as the `receiver_id` and the bet carried inside
the `msg` field.

the contract has no `place_bet` method and no `bet` method. `bet_info`
is the only bet method on the contract, and it is a view.

see [SKILL.md](./SKILL.md) for the overview and the other files.

---

CONTRACT ID PER NETWORK

- MAINNET: app.chess-game.near
- TESTNET: (none — mainnet only)

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Substitute the caller's own values — an agent must never
> reuse the example token id, account id, or amount as though it were
> the user's intent. Amounts here are the token's smallest unit, and
> copying one verbatim moves a real balance.

---

#### THE BET MESSAGE

the chess contract implements the `FungibleTokenReceiver` standard. its
`ft_on_transfer` parses the `msg` string into one of four shapes:

| msg                      | flow                             |
| ------------------------ | -------------------------------- |
| `{"Bet":{...}}`          | place a bet on a game            |
| `{"Challenge":{...}}`    | challenge a player with a wager  |
| `{"AcceptChallenge":{...}}` | accept a wagered challenge   |
| `{"Matchmaking":{...}}`  | join matchmaking with a wager    |

the `Bet` shape is:

```json
{
  "players": ["player1.near", "player2.near"],
  "winner": "player1.near"
}
```

- `players` is a tuple of the two accounts in the game
- `winner` must be one of the two `players`
- the contract **sorts** `players` alphabetically, so the order given
  does not matter — the same bet is found either way

---

#### PLACING A BET

a bet is an `ft_transfer_call` on the token contract:

```json
{
  "receiver_id": "app.chess-game.near",
  "amount": "1000000000000000000000",
  "msg": "{\"Bet\":{\"players\":[\"player1.near\",\"player2.near\"],\"winner\":\"player1.near\"}}"
}
```

- `amount` is in the token's smallest unit
- `msg` is the **JSON string** of the bet object, escaped inside the
  outer JSON. it carries both quotes and backslashes
- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

#### RULES

the contract rejects a bet that breaks any of these:

| rule                        | error                          |
| --------------------------- | ------------------------------ |
| `winner` is not a player    | `InvalidBetWinner`             |
| betting on yourself         | `PlayerCannotBetOnSelf`       |
| `amount` is zero            | `Bet amount must be positive`  |
| the bettor is not registered | `AccountNotRegistered`        |
| the game is already locked  | `BetLocked`                    |
| the bettor has 10 open bets | `MaxBetsReached`               |
| the game has 250 bets       | `MaxBetsPerGameReached`        |
| the token is not whitelisted | `TokenNoWhitelist`             |

- a bettor may hold **10 open bets** at once
- a single game may hold **250 bets** at once
- betting the same pair of players again **adds to** the existing bet
  and does **not** change the recorded `winner` — the first bet's
  winner is the one that counts
- the bettor must be **registered** on the contract first, with
  `storage_deposit` — see [calls.md](./calls.md)

#### THE TOKEN MUST BE WHITELISTED

the contract only accepts a token listed in its whitelist, and the
whitelist is read from the contract, not from this file:

```json
{
  "skip": 0,
  "limit": 50
}
```

that is the shape of `get_token_whitelist` — it takes no arguments.
read the live list with the `get_token_whitelist` view before betting.
a token that is not on the list fails with `TokenNoWhitelist` and the
funds are refunded.

#### WHEN BETS LOCK

bets are **locked** when the game they belong to starts, which happens
when the challenged player accepts the challenge. a locked bet can no
longer be cancelled, and no further bets can be added to it.

bets are **resolved** when the game ends:

- on a victory, the losing side is pooled and paid out **proportionally**
  to the winners, minus the treasury fee
- on a stalemate, every bettor is **refunded in full**
- the treasury fee is the `get_fees` value in basis points, taken from
  the losing side

---

#### VIEWING BETS

`bet_info` is the one bet method on the contract, and it is a view:

```json
{
  "players": ["player1.near", "player2.near"]
}
```

- `players` is the pair to look up, in any order
- gas: none — a view call is read-only and free
- attached deposit: 0

the response holds the lock flag and the bets per token:

```json
{
  "is_locked": false,
  "bets": {
    "wrap.near": [
      ["bettor.near", { "amount": "1000000000000000000000", "winner": "player1.near" }]
    ]
  }
}
```

- `is_locked` is true once the game started
- `bets` maps a token account id to the list of bets on that token
- each bet is a pair of the bettor and the bet itself

---

#### CANCELLING A BET

a bet can be cancelled while the game has not started. the tokens go
back to the bettor's balance on the contract, and are withdrawable
with `withdraw_token`.

```json
{
  "players": ["player1.near", "player2.near"],
  "token_id": "wrap.near"
}
```

- `token_id` is the token the bet was placed in
- only the bettor's own bet can be cancelled
- gas: 30 Tgas
- attached deposit: 0

a locked bet fails with `BetLocked`, a bet that does not exist fails
with `BetNotExists`, and a bet the caller does not hold in that token
fails with `BetNotFound`.

---

#### WITHDRAWING THE BALANCE

tokens the contract holds for a bettor — a cancelled bet, a refund, or
a payout — sit on the contract until they are withdrawn:

```json
{
  "token_id": "wrap.near"
}
```

- gas: 30 Tgas
- attached deposit: 1 yoctoNEAR

read the balance first with the `get_token_amount` view.

---

#### WHERE THE BETS ARE INDEXED

the contract is the source of truth, and a read-only index of it runs
at `https://api.protocol-pawns.com`:

| endpoint                              | what it holds                    |
| ------------------------------------- | -------------------------------- |
| `GET /bets`                           | the global bets                  |
| `GET /game/{game_id}/bets`            | the bets on one game             |
| `GET /account/{account_id}/bets`      | the bets of one account          |
| `GET /account/{account_id}/bet-stats` | the betting stats of one account |
| `GET /leaderboard/bets`               | the bettor leaderboard           |

the api has no auth and is read-only for the chain — it cannot place a
bet.

==========================
<br/>
copyright 2026 by sleet.near
