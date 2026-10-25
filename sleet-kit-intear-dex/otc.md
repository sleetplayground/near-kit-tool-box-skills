# INTEAR DEX otc sub-dex

the INTEAR DEX `otc` sub-dex — an over-the-counter desk.

two or more parties sign a **trade intent** — an amount of one asset
for an amount of another — and a `match` call settles them against the
desk's internal balances. an intent can be authorized by the party
itself (the caller is the party) or by a signature from a key the
party registered with `set_authorized_key`, so a third party can
submit the match on its behalf.

it runs as WASM **inside** the INTEAR DEX engine (`dex.intear.near`),
not as its own account. its views are read through the engine's
`dex_view`, and its calls go through the engine's `dex_call`.

see [SKILL.md](./SKILL.md) for the overview and the other sub-dexes.

---

CONTRACT ID PER NETWORK

- MAINNET: **not deployed** — no dex id is live
- TESTNET: (none)

the dex id is `<deployer>/<id>`, but no otc dex has been deployed on
mainnet. the methods below are the ones the source defines; check
that the dex exists before reading anything.

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id, asset id, or nonce as though it were the user's
> intent. Depositing or withdrawing with a copied value moves real
> funds.
>
> #### BORSH, NOT JSON
>
> the otc dex's arguments and responses are **borsh** encoded, not
> JSON. they travel through the engine's `dex_view` / `dex_call` as
> base64 blobs. the JSON blocks below show the *logical* shape of
> each argument, not the bytes on the wire.
>
> #### NOT AUDITED
>
> The INTEAR DEX source repository states plainly: *"Not audited."*

---

#### THE `dex_view` ENVELOPE

```json
{
  "dex_id": "<deployer>/<id>",
  "method": "get_balance",
  "args": "<base64 of the borsh-encoded arguments>"
}
```

the response is a base64 string holding the dex's borsh-encoded
result.

---

### get_balance - VIEW

```json
{
  "account_id": "alice.near",
  "asset_id": "near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the desk's internal balance of one asset for one account, or
`null` when the account has no balance entry for that asset.

this is the desk's own ledger — balances the account deposited with
`deposit_assets`, separate from the engine's inner balances.

an `asset_id` is `near`, `nep141:<contract_id>`,
`nep245:<contract_id>:<token_id>` or
`nep171:<contract_id>:<token_id>`.

### get_authorized_key - VIEW

```json
{
  "account_id": "alice.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the public key an account authorized to sign its own trade
intents, or `null` when the account has not authorized any key.

### is_nonce_used - VIEW

```json
{
  "nonce": "42",
  "account_id": "alice.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns whether an account has already used a trade-intent nonce.

a nonce makes a signed trade intent single-use. before submitting a
`match` with a signed intent, check that its nonce has not been used
yet.

### storage_balance_of - VIEW

```json
{
  "account_id": "alice.near"
}
```

- gas: none — a view call is read-only and free
- attached deposit: 0

returns the NEAR storage deposit an account has with the desk, or
`null` when the account has no storage registered. the shape is
`{ "total": "...", "used": "..." }` — the part of `total` the
account's entries do not take up can be withdrawn.

---

### match - CALL

```json
{
  "authorized_trade_intents": [
    {
      "trade_intent": {
        "user_id": "alice.near",
        "asset_in": "near",
        "asset_out": "nep141:wrap.near",
        "amount_in": "1000000000000000000000000",
        "amount_out": "2000000000000000000000000",
        "validity": {
          "expiry": { "BlockHeight": 200000000 },
          "nonce": "1",
          "only_for_whitelisted_parties": null
        }
      },
      "authorization_method": { "Predecessor": null }
    }
  ],
  "output_destination": "InternalOtcBalance"
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

settles the trade intents against the desk's balances. the sum of
every intent's `asset_in` and `asset_out` must net to zero per asset,
or the call fails.

`authorization_method` is either `{"Predecessor": null}` — the caller
is the party — or `{"Signature": "<base64 signature>"}`, signed over
`sha256(borsh(trade_intent))` by the party's authorized key.

`output_destination` is one of `InternalOtcBalance`,
`IntearDexBalance` or `WithdrawToUser`.

`validity.expiry` is `{ "BlockHeight": 123 }` or
`{ "Timestamp": { "milliseconds": "..." } }`. `validity.nonce` makes
the intent single-use.

### deposit_assets - CALL

```json
{}
```

- gas: not documented yet
- attached deposit: the assets to deposit, and the NEAR for storage

deposits assets into the desk's internal balance for the caller.

### withdraw_assets - CALL

```json
{
  "assets": [
    {
      "asset_id": "near",
      "amount": null,
      "to": null,
      "to_inner_balance": false
    }
  ]
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

withdraws from the desk's internal balance. omit `amount` to withdraw
the whole balance of that asset, omit `to` to withdraw to the caller's
own account, and set `to_inner_balance` to true to move the assets to
the engine's inner balance instead of out of the contract.

### storage_deposit - CALL

```json
{}
```

- gas: not documented yet
- attached deposit: the NEAR amount to reserve as storage

### set_authorized_key - CALL

```json
{
  "key": "ed25519:..."
}
```

- gas: not documented yet
- attached deposit: 1 yoctoNEAR

registers the key that may sign the caller's trade intents.

---

#### SOURCE

- https://github.com/INTEARnear/dex/tree/main/dexes/otc

```sh
# the engine's method list — the dex's own methods are not on the
# account, they run inside it
near contract inspect dex.intear.near network-config mainnet now
```

==========================
<br/>
copyright 2026 by sleet.near
