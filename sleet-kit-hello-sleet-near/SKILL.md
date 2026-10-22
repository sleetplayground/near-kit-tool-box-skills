---
name: sleet-kit-hello-sleet-near
description: Documents the hello.sleet.near greeting smart contract - its deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL), exact argument shape, and required gas and attached deposit. Use this skill whenever the user mentions hello.sleet.near, the sleet greeting contract, get_greeting, or set_greeting, or asks what arguments a greeting contract method takes - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---

# hello.sleet.near

a simple near greeting smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: hello.sleet.near
- TESTNET: hello.sleet.testnet

> #### ARG EXAMPLES
>
> The JSON blocks below are **worked examples**, not values to copy
> verbatim. Treat them as a template for argument *shape* only, and
> substitute the caller's own values — an agent must never reuse the
> example account id or amount as though it were the user's intent.

### get_greeting - VIEW

- no args necessary
- gas: none — a view call is read-only and free
- attached deposit: 0

### set_greeting - CALL

```json
{
  "greeting": "Hello"
}
```

- gas: 3 Tgas
- attached deposit: 0

==========================
<br/>
copyright 2026 by sleet.near