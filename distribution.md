## `x/distribution module`

### Introduction
The  `distribution`  module describes a functional way to passively distribute rewards between validators and delegators. 

### Overview
The `distribution` module contains the following network parameters:

 -   `community_tax`  (type: string (dec)): The rate of community tax;
-   `base_proposer_reward` (type: string (dec)):    Base bonus on transaction fees collected in a valid block;
-   `bonus_proposer_reward`  (type: string (dec)):  Maximum bonus on transaction fees collected in a valid block;
-   `withdraw_addr_enabled`  (type: bool):  Whether delegators can set a different address to withdraw their rewards.

 #### Validator & Delegator Rewards.
The collected rewards are pooled globally and distributed to validators and delegators in a passive way. Each validator has the option of charging the delegators commission on the rewards gathered on their behalf. Fees are deposited into a global reward pool as well as a validator proposer-reward pool. Because of the nature of passive accounting, if parameters that affect the rate of reward distribution are changed, rewards must be withdrawn as well.
	


### `x/distribution` module in Shentu
#### Transactions and Queries 
#### **Transactions**
- `certik tx distribution fund-community-pool [amount] [flags]`: Funds the community pool with the specified amount. Jack can make a contribution to the community pool 100uctk by using:
```{engine='sh'}
$ certik tx distribution fund-community-pool 100uctk --from jack --chain-id=certikchain

{"body":{"messages":[{"@type":"/cosmos.distribution.v1beta1.MsgFundCommunityPool","amount":[{"denom":"uctk","amount":"100"}],"depositor":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

- `certik tx distribution set-withdraw-addr [withdraw-addr] [flags]`: Set the withdraw address for rewards associated with a delegator address by using:
```{engine='sh'}
$ certik tx distribution set-withdraw-addr certik1f6fgmdrrxpvvhr0ngxq5fazs8mu5x9sljv2dky --from alice --chain-id=certikchain

{"body":{"messages":[{"@type":"/cosmos.distribution.v1beta1.MsgSetWithdrawAddress","delegator_address":"certik1f6fgmdrrxpvvhr0ngxq5fazs8mu5x9sljv2dky","withdraw_address":"certik1f6fgmdrrxpvvhr0ngxq5fazs8mu5x9sljv2dky"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```
- `certik tx distribution withdraw-all-rewards [flags]`: Withdraw all rewards for a single delegator. For example, Jack wants to withdraw all rewards for his delegator by using:
```{engine='sh'}
$ certik tx distribution withdraw-all-rewards --from jack --chain-id=certikchain

{"body":{"messages":[{"@type":"/cosmos.distribution.v1beta1.MsgWithdrawDelegatorReward","delegator_address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","validator_address":"certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```
- `certik tx distribution withdraw-rewards [validator-addr] [flags]`: Withdraw rewards from a given delegation address, and optionally withdraw validator commission if the delegation address given is a validator operator. 

```{engine = 'sh'
$ certik tx distribution withdraw-rewards certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r --from alice --chain-id=certikchain

{"body":{"messages":[{"@type":"/cosmos.distribution.v1beta1.MsgWithdrawDelegatorReward","delegator_address":"certik1f6fgmdrrxpvvhr0ngxq5fazs8mu5x9sljv2dky","validator_address":"certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}
  
confirm transaction before signing and broadcasting [y/N]: y
```

#### **Queries**
- `certik query distribution commission [validator] [flags]`: Query validator commission rewards from delegators to that validator.

```{enginer = 'sh'} 
$ certik query distribution commission certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r --chain-id=certikchain

commission:
- amount: "1213.600000000000000000"
denom: uctk
```
- `certik query distribution community-pool [flags]`: Query all coins in the community pool which is under Governance control.
```{engine = 'sh'}
$ certik query distribution community-pool --chain-id=certikchain

pool:
- amount: "100.400000000000000000"
denom: uctk
```
- `certik query distribution params [flags]`: Query the current distribution parameters.
```{engine = 'sh'}
$ certik query distribution params --chain-id certikchain

base_proposer_reward: "0.010000000000000000"
bonus_proposer_reward: "0.040000000000000000"
community_tax: "0.000000000000000000"
withdraw_addr_enabled: true
```
- `certik query distribution rewards [delegator-addr] [validator-addr] [flags]`: Query all rewards earned by a delegator, optionally restrict to rewards from a single validator. 
```{engine = 'sh'}
$ certik query distribution rewards certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r --chain-id certikchain

rewards:
- amount: "423.000000000000000000"
denom: uctk
```
- `certik query distribution slashes [validator] [start-height] [end-height] [flags]`: Query all slashes of a validator for a given block range.
```{engine = 'sh'}
$ certik query distribution slashes certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r 0 10 --chain-id certikchain

pagination:
next_key: null
total: "0"
slashes: [] #No slashes here
```
- `certik query distribution validator-outstanding-rewards [validator] [flags]`:  Query distribution outstanding (un-withdrawn) rewards for a validator and all their delegations.
```{engine = 'sh'}
$ certik query distribution validator-outstanding-rewards certikvaloper1nc6v8tme0env488494ys09ld39dn9xzwduu78r --chain-id certikchain

rewards:
- amount: "1754.600000000000000000"
denom: uctk
```
