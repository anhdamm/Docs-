
## `x/bank` module

### Introduction
The `bank` module tracks and provides query support for the total supply of all assets used in the application. It supports token transfer functionalities. For example,  the total supply is updated whenever a token is:

-   **Minted**: e.g, as part of the inflation mechanism.
-   **Burned**: e.g, due to slashing or if a governance proposal is vetoed.

The  `bank`  module maintains the state of two primary objects:
-   ***Account balances by address***:   `[]byte("balances") | []byte(address) / []byte(balance.Denom) -> ProtocolBuffer(balance)`
-   ***Total supply of tokens of the chain***: `0x0 -> ProtocolBuffer(Supply)`

### `x/bank` module in Shentu

#### Transactions and Queries 
#### **Transactions**
- `certik tx bank send [from_key_or_address] [to_address] [amount] [flags]`: Send funds from one account to another. You can transfer of tokens between to a designated address by the `certik tx bank send` command. For example, we can send 7000uctk from `address_a` to `address_b` by using:
```{engine='sh'}
$ certik tx send jack $(certik keys show alice -a) 70000uctk --gas-prices=0.025uctk --from jack --chain-id=certikchain

{"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address":"certik1umv6655pyhvzjtvvcy3dwkelc5wuvjz2m53pq4","to_address":"certik1sgyzkqhn04uja5urfkhmuszh8x2q9nu7dzeggu","amount":[{"denom":"uctk","amount":"70000"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[{"denom":"uctk","amount":"5000"}],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

- `certik tx bank locked-send [from_key_or_address] [to_address] [amount] [flags]`: Send coins and have them locked (vesting). For example, we can send 2000uctk from `address_a` to `address_b` by using:
```{engine='sh'
$ certik tx bank locked-send jack $(certik keys show alice -a) 200uctk --from jack --chain-id=certikchain

{"body":{"messages":[{"@type":"/shentu.bank.v1alpha1.MsgLockedSend","from_address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","to_address":"certik1f6fgmdrrxpvvhr0ngxq5fazs8mu5x9sljv2dky","unlocker_address":"","amount":[{"denom":"uctk","amount":"200"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

#### **Queries**
- `certik query bank balances [address] [flags]`:  Query the total balance of an account or of a specific denomination by address. For example, we want to check the balance of jack's key, we use: 
```{engine = 'sh' 
$ certik query bank balances $(certik keys show jack -a) #$(certik keys show jack -a) finds the address of jack's key.
balances:
- amount: "99925000"
denom: uctk
pagination:
next_key: null
total: "0"
```
- `certik query bank denom-metadata [flags]`: Query the client metadata for all the registered coin denominations. 
```{engine = 'sh'}
$ certik query bank denom-metadata --chain-id=certikchain

metadatas: []
pagination:
next_key: null
total: "0"
```
- `certik query bank total [flags]`: Query total supply of coins that are held by accounts in the chain.
```{engine = 'sh'}
$ certik query bank total

supply:
- amount: "200000119"
denom: uctk
```
