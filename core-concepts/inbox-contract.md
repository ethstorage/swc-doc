## Motivation

The batch inbox is currently an Externally Owned Account (EOA), which has both advantages and disadvantages:

Advantages:
- Low submission gas cost due to the absence of onchain execution.
- Verification logic is moved offchain to the derivation part, protected by a fault dispute game with a correct absolute prestate.

Disadvantage:
- Onchain verification is not possible.


This specification aims to allow the batch inbox to be a contract, enabling customized batch submission conditions such as:
- Requiring the batch transaction to be signed by a quorum of sequencers in a decentralized sequencing network; or
- Mandating that the batch transaction call a BLOB storage contract (e.g., EthStorage) with a long-term storage fee, which is then distributed to data nodes that prove BLOB storage over time.


## How It Works

The integration process consists of three primary components:
1. The [`BatchInboxAddress`](https://github.com/ethereum-optimism/optimism/blob/db107794c0b755bc38a8c62f11c49320c95c73db/op-chain-ops/genesis/config.go#L77) can now be set to either an Externally Owned Account (EOA) or a smart contract. When a contract is used, it assumes responsibility for verifying and enforcing batch submission conditions.
2. Modification of the `op-node` derivation process: The `op-node` will be updated to exclude failed batch transactions during the derivation process. This change ensures that only successfully executed batch transactions are processed and included in the derived state. 
3. Modification of the op-batcher submission process: The op-batcher will be updated to [call `recordFailedTx`](https://github.com/blockchaindevsh/optimism/blob/02e3b7248f1b590a2adf1f81488829760fa2ba03/op-batcher/batcher/driver.go#L537) for failed batch transactions. This modification ensures that the data contained in failed transactions will be resubmitted automatically.
   1. Most failures will be detected during the [`EstimateGas`](https://github.com/ethereum-optimism/optimism/blob/8f516faf42da416c02355f9981add3137a3db190/op-service/txmgr/txmgr.go#L266) call. However, under certain race conditions, failures may occur after the transaction has been included in a block.

These modifications aim to enhance the security and efficiency of the batch submission and processing pipeline, allowing for more flexible and customizable conditions while maintaining the integrity of the derived state.