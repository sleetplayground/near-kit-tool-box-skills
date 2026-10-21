---
name: sleet-kit-hello-sleet-near
description: Documents the hello.sleet.near greeting smart contract - its deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL) and exact argument shape. Use this skill whenever the user mentions hello.sleet.near, the sleet greeting contract, get_greeting, or set_greeting, or asks what arguments a greeting contract method takes - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---

# hello.sleet.near

a simple near greeting smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: hello.sleet.near
- TESTNET: hello.sleet.testnet

### get_greeting - VIEW

- no args necessary

### set_greeting - CALL

```json
{
  "greeting": "Hello"
}
```

==========================
<br/>
copyright 2026 by sleet.near