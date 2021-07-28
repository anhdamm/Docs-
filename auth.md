## `x/auth` module

### Introduction
The auth module is responsible for implementing the application's fundamental transaction and account types as SDK is agnostic about these details. It exposes the account keeper, which allows other modules to read, write, and edit accounts, and contains the ante handler, which performs all fundamental transaction validity checks (signatures, nonces, auxiliary fields). 

### Overview
The `distribution` module contains the following network parameters:
	- `MaxMemoCharacters`: The maximum permitted number of characters in the memo of a transaction.
	- `TxSigLimit`: The maximum number of singers in a transaction. A single transaction can have multiple messages and multiple signers. The default limit is 100 since the sig verification cost is much higher than other operations.
	- `TxSizeCostPerByte`:  To calculate gas consumption of the transaction.
	- `SigVerifyCostED25519`: The gas cost for verifying ED25519 signatures.
	- `SigVerifyCostSecp256k1`: The gas cost for verifying Secp256k1 signatures.

 ### `x/auth` module in Shentu
#### Transactions and Queries 
#### **Transactions**
- `certik tx auth unlock [address] [amount] [flags]`: Unlock the amount from a manual vesting account's vesting coins. For example, Jack want to transfer 100 uctk to alice from his manual vesting account's vesting coins: 
```{engine='sh'}
$ certik tx auth unlock $(certik keys show alice -a) 100uctk --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.auth.v1alpha1.MsgUnlock","issuer":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","account":"certik1f6fgmdrrxpvvhr0ngxq5fazs8mu5x9sljv2dky","unlock_amount":[{"denom":"uctk","amount":"100"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

#### **Queries**
- `certik query auth account [address] [flags]`: Query for account by address. We want to query Jack's account by using:

```{enginer = 'sh'} 
$ certik query auth account $(certik keys show jack -a) --chain-id certikchain

'@type': /cosmos.auth.v1beta1.BaseAccount
account_number: "0"
ddress: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
pub_key:
'@type': /cosmos.crypto.secp256k1.PubKey
key: A9ewR6fdy1xoyb0h9YAXK0aXUEzTehYNUn4WDi7o4A2X
sequence: "6"
```
- `certik query auth params [flags]`: Query the current auth parameters
```{engine = 'sh'}
$ certik query auth params --chain-id certikchain

max_memo_characters: "256"
sig_verify_cost_ed25519: "590"
sig_verify_cost_secp256k1: "1000"
tx_sig_limit: "7"
tx_size_cost_per_byte: "10"
```
