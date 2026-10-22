---
name: sleet-kit-social-near
description: Documents the social.near decentralized social contract - the deployed contract id on mainnet and testnet, and each method's call type (VIEW or CALL). Use this skill whenever the user mentions social.near, the decentralized social protocol, get_nodes, get_accounts on a social contract, share_storage, or grant_write_permission - including when they want to call or inspect this contract and need the method list, argument names, or network-specific contract id.
---

# social.near

the social.near decentralized social smart contract

---

CONTRACT ID PER NETWORK

- MAINNET: social.near
- TESTNET: v1.social08.testnet

> #### ARG AND GAS
>
> No argument format, gas, or attached deposit has been confirmed for
> this contract. Every method below is listed as callable or read-only
> by name only — treat the list as a starting point, not a
> specification. Get the authoritative signatures before using any of
> them:
>
> ```sh
> near contract inspect social.near network-config mainnet now
> near contract inspect v1.social08.testnet network-config testnet now
> ```

### get - VIEW

- arg format not documented yet

### keys - VIEW

- arg format not documented yet

### get_account - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### get_accounts - VIEW

- arg format not documented yet

### get_account_count - VIEW

- no args necessary

### get_account_storage - VIEW

- arg format not documented yet

### get_node_count - VIEW

- no args necessary

### get_nodes - VIEW

- arg format not documented yet

### get_status - VIEW

- no args necessary

### get_version - VIEW

- no args necessary

### get_shared_storage_pool - VIEW

- arg format not documented yet

### debug_get_permissions - VIEW

- arg format not documented yet

### is_write_permission_granted - VIEW

- arg format not documented yet

### contract_source_metadata - VIEW

- no args necessary

### set - CALL

- arg format not documented yet

### share_storage - CALL

- arg format not documented yet

### shared_storage_pool_deposit - CALL

- arg format not documented yet

### grant_write_permission - CALL

- arg format not documented yet

### get_node - CALL

- arg format not documented yet

### new - CALL

- arg format not documented yet

### set_status - CALL

- arg format not documented yet

### storage_deposit - CALL

- no args necessary

### storage_withdraw - CALL

- arg format not documented yet

### storage_unregister - CALL

- no args necessary

### storage_balance_of - VIEW

```json
{
  "account_id": "sleet.near"
}
```

### storage_balance_bounds - VIEW

- no args necessary

### migrate_state - CALL

- arg format not documented yet

### genesis_init_node_count - CALL

- arg format not documented yet

### genesis_init_nodes - CALL

- arg format not documented yet

### genesis_init_accounts - CALL

- arg format not documented yet

==========================
<br/>
copyright 2026 by sleet.near