 ### `x/oracle` module in Shentu
#### Transactions and Queries 
#### **Transactions**
- `certik tx oracle claim-reward <address> [flags]`: Withdraw all of an operator's accumulated rewards. 
```{engine='sh'}
$ certik tx oracle claim-reward certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgWithdrawReward","address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

- `certik tx oracle create-operator <address> <collateral> [flags]`: Create an operator and deposit collateral. For instance, you want to create new  operator with alice's address from jack's key by using:
```{engine='sh'}
$ certik tx oracle create-operator certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we 500000uctk --fees 10000uctk --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgCreateOperator","address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","collateral":[{"denom":"uctk","amount":"500000"}],"proposer":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","name":""}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[{"denom":"uctk","amount":"10000"}],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

- `certik tx oracle create-task <contract_address> <function> <bounty> [flags]`: Create a task
```{engine='sh'}
$ certik tx oracle create-task 0xD850942eF8811f2A866692A623011bDE52a462C1 0x00000000 1000uctk --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgCreateTask","contract":"0xD850942eF8811f2A866692A623011bDE52a462C1","function":"0x00000000","bounty":[{"denom":"uctk","amount":"1000"}],"description":"","creator":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","wait":"0","valid_duration":"0s"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

- `certik tx oracle deposit-collateral <address> <amount> [flags]`: Increase an operator's collateral.
```{engine='sh'}
$ certik tx oracle deposit-collateral certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we 10000uctk --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgAddCollateral","address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","collateral_increment":[{"denom":"uctk","amount":"10000"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```
- `certik tx oracle withdraw-collateral <address> <amount> [flags]`: Reduce an operator's collateral
```{engine = 'sh'}
$ certik tx oracle withdraw-collateral certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we 10000uctk --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgReduceCollateral","address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","collateral_decrement":[{"denom":"uctk","amount":"10000"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```
- `certik tx oracle remove-operator <address> [flags]`: Remove an operator and withdraw collateral & rewards. For example, we remove the operator that we created earlier above:
```{engine = 'sh'}
$ certik tx oracle remove-operator certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgRemoveOperator","address":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we","proposer":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```
- `certik tx oracle delete-task <contract_address> <function> [flags]`: Delete a finished task.
```{engine = 'sh'}
$ certik tx oracle delete-task 0xD850942eF8811f2A866692A623011bDE52a462C1 0x00000000 --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgDeleteTask","contract":"0xD850942eF8811f2A866692A623011bDE52a462C1","function":"0x00000000","force":false,"deleter":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

- `certik tx oracle respond-to-task <contract_address> <function> <score> [flags]`: Respond to a task. 
```{engine = 'sh'}
$ certik tx oracle respond-to-task 0xD850942eF8811f2A866692A623011bDE52a462C1 0x00000000 9 --from jack --chain-id certikchain

{"body":{"messages":[{"@type":"/shentu.oracle.v1alpha1.MsgTaskResponse","contract":"0xD850942eF8811f2A866692A623011bDE52a462C1","function":"0x00000000","score":"9","operator":"certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we"}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

#### **Queries**
- `certik query oracle operator <address> [flags]`: Get operator information

```{engine = 'sh'}
$ certik query oracle operator certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we --chain-id certikchain

operator:
accumulated_rewards: []
address: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
collateral:
- amount: "500000"
denom: uctk
name: ""
proposer: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
```
- `certik query oracle operators [flags]`: Get operators information
```{engine = 'sh'}
$ certik query oracle operator certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we --chain-id certikchain

operator: #We just add 1 operator above.
accumulated_rewards: []
address: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
collateral:
- amount: "500000"
denom: uctk
name: ""
proposer: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
```
- `certik query oracle withdraws [flags]`: Get all withdrawals. 
```{engine = 'sh'}
$ certik query oracle withdraws --chain-id certikchain

withdraws:
- address: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
amount:
- amount: "10000"
denom: uctk
due_block: "12271"
```

- `certik query oracle response <operator_address> <contract_address> <function> [flags]`: Get response information. 
```{engine = 'sh'}
$ certik query oracle response certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we 0xD850942eF8811f2A866692A623011bDE52a462C1 0x00000000 --chain-id certikchain

response:
	operator: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
	reward:
	- amount: "1000"
	denom: uctk
	score: "9"
	weight: "500000"
```

- `certik query oracle task <contract_address> <function> [flags]`: Get task information
```{engine = 'sh'}
$ certik query oracle task 0xD850942eF8811f2A866692A623011bDE52a462C1 0x00000000 --chain-id certikchain

task:
	begin_block: "23850"
	bounty:
	- amount: "1000"
	denom: uctk
	closing_block: "23870"
	contract: 0xD850942eF8811f2A866692A623011bDE52a462C1
	creator: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
	description: ""
	expiration: "2021-07-28T15:38:08.510323Z"
	function: "0x00000000"
	responses:
	- operator: certik1nc6v8tme0env488494ys09ld39dn9xzw6gc7we
	reward:
	- amount: "1000"
	denom: uctk
	score: "9"
	weight: "500000"
	result: "9"
	status: TASK_STATUS_SUCCEEDED
	waiting_blocks: "20"
```
