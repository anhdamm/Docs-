## `x/staking` module 
The Cosmos-SDK-based blockchain can now handle an advanced Proof-of-Stake system thanks to `staking` module. Holders of the chain's native staking token can become validators and delegate tokens to validators in this system, which determines the system's effective validator set.

The staking module contains the following parameters:
	-   `unbonding_time`: The duration of unbonding;
	-   `max_validators`: The maximum number of validator;
	-   `max_entries`: The max entries for either unbonding delegation or redelegation;
	-   `historical_entries`: The number of historical entries to persist
	-   `bond_denom`: Coin denomination for staking.


### `x/staking` module in Shentu
 `staking` module in Shentu has the same transactions and queries as Cosmos SDK. 
