---
name: sleet-kit-app-chess-game-near
description: Documents the app.chess-game.near on-chain chess contract - the deployed contract id on mainnet, each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit, and the ft_transfer_call flows for placing bets and wagers. Use this skill whenever the user mentions app.chess-game.near, protocol pawns, on-chain chess, play_move, create_ai_game, challenge, accept_challenge, join_matchmaking, bet_info, cancel_bet, wager_victory, or placing a bet on a chess game - including when they want to call or inspect this contract and need the method list, argument names, the game id format, the bet message shape, the whitelisted tokens, or the network-specific contract id.
---

# app.chess-game.near

the protocol pawns on-chain chess smart contract — play, wager, bet,
matchmake, and climb the elo leaderboard.

this skill is split into files. this page is the overview and the index.

| file                                     | what it holds                                            |
| ---------------------------------------- | -------------------------------------------------------- |
| [views.md](./views.md)                   | the 30 read methods of the contract                       |
| [calls.md](./calls.md)                   | the 26 state-changing methods of the contract             |
| [bets.md](./bets.md)                     | the betting and wagering flows, which are token transfers |

---

CONTRACT ID PER NETWORK

- MAINNET: app.chess-game.near
- TESTNET: (none — mainnet only)

```sh
# near cli rs - get a list of methods
near contract inspect app.chess-game.near network-config mainnet now
```

---

#### THE THREE KINDS OF CALL

1. **a view** — a read, free, no signer needed. `get_board`, `game_info`,
   `bet_info`, `get_account` and the rest live in [views.md](./views.md).
2. **a contract method** — a call to a method *on this contract*.
   `play_move`, `challenge`, `accept_challenge`, `cancel_bet` and the
   rest live in [calls.md](./calls.md).
3. **an `ft_transfer_call` on the token** — a call on the **token
   contract**, with this contract as the `receiver_id` and the call
   carried inside `msg`. **every bet and every wager works this way**,
   and they live in [bets.md](./bets.md).

the contract has **no `place_bet` method, no `bet` method, no `wager`
method and no `challenge_with_wager` method**. those flows are all
`ft_transfer_call` on the token.

the contract also carries a set of settlement callbacks —
`wager_victory_callback`, `wager_stalemate_callback`,
`cancel_wager_refund_callback`, `reject_challenge_wager_callback`,
`withdraw_token_callback` and `withdraw_treasury_callback`. they are
`#[private]`, they are called by the contract itself, and they are not
for a caller to invoke.

---

#### THE GAME ID

a game is identified by a `GameId`, a tuple of three values:

```json
[128903456, "player1.near", "player2.near"]
```

- the first value is the block height the game was created at
- the second is the white player, always an account id
- the third is the black player, or `null` for a game against the ai

the api takes the game id as the json-stringified tuple, percent-encoded
into the path.

---

#### MOVES ARE COORDINATE NOTATION

`play_move` takes **coordinate notation**, not san. `"Nf3"` and
`"Qxe4"` fail.

| form                | example      |
| ------------------- | ------------ |
| compact             | `"e2e4"`     |
| separated           | `"e2 e4"`    |
| explicit            | `"e2 to e4"` |
| kingside castle     | `"O-O"`      |
| queenside castle    | `"O-O-O"`    |
| promotion           | `"e7 to e8 Q"` |

the columns are a–h and the rows are 1–8, counted from white's side.

---

#### GAS AND DEPOSITS

- every **view** is free and takes no deposit
- a **contract method** takes no deposit, except the storage methods
  and the token methods:
  - `storage_deposit` attaches at least **0.05 NEAR**, the storage cost
  - `storage_withdraw` and `storage_unregister` attach exactly
    **1 yoctoNEAR** — but the contract does not implement either one,
    the calls fail with `OperationNotSupported`
  - `set_is_agent` and `withdraw_token` attach exactly
    **1 yoctoNEAR**
- an **`ft_transfer_call`** attaches exactly 1 yoctoNEAR

`join_matchmaking` attaches **nothing** — it is not payable. a **wagered**
queue entry goes through `ft_transfer_call` on the token contract
instead, and that is the path that carries a token deposit.

the gas follows the work the call does:

| flow                    | gas    |
| ----------------------- | ------ |
| `play_move` against Easy | 50    |
| `play_move` against Medium or a human game | 150 |
| `play_move` against Hard | 250   |
| `play_move` against VeryHard | 400 |
| `create_ai_game` against Easy | 50 |
| `create_ai_game` against Medium | 150 |
| `create_ai_game` against Hard | 250 |
| `create_ai_game` against VeryHard | 400 |
| `resign`, `cancel`, `new`, `migrate` | 300 |
| the other calls        | 30     |
| an `ft_transfer_call`  | 30     |

---

#### REGISTRATION IS THE FIRST STEP

an account must be registered on the contract before it can play,
challenge, join the matchmaking queue, or bet. `storage_deposit` with
at least 0.05 NEAR does it, and it is done once.

a new account starts with an elo rating of 1000, no points, and every
stat at zero.

---

#### THE LIMITS

| limit                            | value |
| -------------------------------- | ----- |
| active games per account         | 5     |
| open challenges per account      | 25    |
| open bets per bettor             | 10    |
| bets per game                    | 250   |
| matchmaking queue entries        | 200   |
| blocks before a player may cancel their own game | ~3 days |
| blocks before anyone may cancel a game | ~14 days |

a cancel is only allowed when it is not the caller's turn.

---

#### BETTING

the betting surface lives in [bets.md](./bets.md). the short version:

- a bet is an `ft_transfer_call` on the **token contract**, with
  `app.chess-game.near` as the `receiver_id` and the bet inside `msg`
- the `msg` is the JSON string of
  `{"Bet":{"players":[...],"winner":"..."}}`
- only the whitelisted tokens work. read the live list with the
  `get_token_whitelist` view
- the `winner` must be one of the two `players`, and the bettor cannot
  be one of them
- bets lock when the game starts and resolve when it ends
- on a victory the losing side is pooled and paid out proportionally,
  minus the treasury fee. on a stalemate everyone is refunded in full
- the treasury fee is the `get_fees` value, in basis points

---

#### THE API

the contract is the source of truth, and a read-only index of it runs
at `https://api.protocol-pawns.com`. it has no auth, and it cannot
place a bet or a move.

| endpoint                              | what it holds                    |
| ------------------------------------- | -------------------------------- |
| `GET /bets`                           | the global bets                  |
| `GET /game/{game_id}/bets`            | the bets on one game             |
| `GET /account/{account_id}/bets`      | the bets of one account          |
| `GET /account/{account_id}/bet-stats` | the betting stats of one account |
| `GET /leaderboard/bets`               | the bettor leaderboard           |
| `GET /games`                          | the games, paginated             |
| `GET /game/{game_id}/moves`           | the move history, with fen       |
| `GET /account/{account_id}/active-game` | the active game of an account |
| `GET /leaderboard/elo`                | the elo leaderboard              |
| `GET /challenges`                     | the open challenges              |

the api also serves an event stream at
`GET /events?account={account_id}`, which pushes the `create_game`,
`play_move`, `resign_game`, `cancel_game`, `challenge`,
`accept_challenge` and `reject_challenge` events.

---

#### THE PPP POINTS

the contract is also a non-transferable ppp token. the symbol is `PPP`,
the decimals are `6`, and the name is `Protocol Pawns Points`.

`ft_transfer` and `ft_transfer_call` on the ppp side panic — the points
cannot move.

points are earned from quests and achievements, and are claimed with
`claim_points`. points earned during a wager or a bet flow land as
pending points and are claimed the same way.

---

#### SOURCE

- https://github.com/Protocol-Pawns/chess-on-chain

the contract is `crates/chess`, the shared logic is `crates/chess-lib`
and the shared types are `crates/chess-common`. the files that hold the
argument shapes for this skill:

| file                                        | what it holds                            |
| ------------------------------------------- | ---------------------------------------- |
| `crates/chess-lib/src/view.rs`              | the view methods and their arguments      |
| `crates/chess-lib/src/lib.rs`               | the call methods and their arguments      |
| `crates/chess-lib/src/ft_receiver.rs`       | the `Bet`, `Challenge`, `AcceptChallenge` and `Matchmaking` messages |
| `crates/chess-lib/src/bet.rs`               | the bet rules, the limits and the errors  |
| `crates/chess-lib/src/internal.rs`          | the bet locking and the payout maths      |
| `crates/chess-common/src/lib.rs`            | the `GameId`, the events and the shapes   |

---

#### LINKS

- https://protocol-pawns.com — the landing page
- https://app.protocol-pawns.com — the app, and the bets page at
  https://app.protocol-pawns.com/bets
- https://app.protocol-pawns.com/.well-known/ai/skill.md — the agent
  skill, the upstream guide for agents
- https://app.protocol-pawns.com/llms.txt — the llms file
- https://api.protocol-pawns.com — the indexed api
- https://api.protocol-pawns.com/doc — the api docs
- https://rpc.shitzuapes.xyz — the rpc the upstream skill points at
- https://t.me/protocolpawns — the telegram
- https://gov.near.org/t/approved-protocol-pawns-gaming-dao/38514 — the
  approved gaming dao proposal

---

#### THE MIDDLEMAN PACKAGE

the indexed api has a typed client in the middleman package:

- https://github.com/sleet-js/middleman/tree/main/middleman-protocol-pawns-api
- https://www.npmjs.com/package/@sleet-js/middleman-protocol-pawns-api

it covers the api — the games, the moves, the challenges, the bets, the
leaderboards and the event stream. it is read-only for the chain: it
cannot place a bet or a move, because those are near transactions
against the contract.

---

#### THE METHOD LIST

```sh
# the authoritative method list
near contract inspect app.chess-game.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
