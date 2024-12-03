## **Motivation**

To bridge the gap between traditional web users and the growing world of Web3, we propose a **non-transferable, value-less gas token** named SoulETH. The concept revolves around facilitating Web2 users' entry into Web3 by airdropping them with SoulETH. This token will enable users to pay for transaction gas fees without the immediate need for ETH or other valuable cryptocurrencies. This initiative is particularly aimed at those new to Web3, providing a seamless transition without the upfront cost of acquiring ETH.

## **How It Works (Using OP Stack as a Case Study)**

- **Technical Implementation**: SoulETH is managed through an ERC20 contract with a twist: only the sequencer can **`mint`** new tokens. To maintain its non-transferable nature, all attempts to use ERC20's **`transfer`**, **`transferFrom`**, or **`approve`** methods will result in failure.
- **Wallet Compatibility**: The transaction format remains consistent with existing Rollup ones. Users can transact using ETH wallets without the need for additional ones (e.g., AA wallets), ensuring a smooth user experience.
- **Gas Fee Process**: For an L2 transaction, the fee will first be deducted from the user's SoulETH balance. If the balance is insufficient, the system will then draw from the user's ETH balance. To account for the additional processing costs of SoulETH, the base gas cost may be slightly higher than traditional transactions, likely from 21,000 to 21,000+2,100+5,000=28,100 gas.

## **Potential Challenges**

- **Sequencer Incentives**:  As the miner of SoulETH, there's an inherent expectation for the sequencer to process SoulETH transactions, though it may re-order transactions to prioritize transactions paying fees in ETH over SoulETH.
- **Risk of DoS Attacks**: The introduction of a "free" transaction token like SoulETH could potentially incur denial-of-service (DoS) type attacks. Mitigation strategies include limiting the initial airdrop quantity to support a finite number of transactions, coupled with a mechanism to replenish SoulETH for users with verified on-chain behaviors.
- **Sybil Attack Prevention**: Airdrops are susceptible to exploitation through Sybil attacks. We will implement established best practices of airdrops to identify and mitigate such threats effectively.

This proposal outlines a novel approach to lowering the entry barrier into Web3, leveraging SoulETH as a transitional gas token for Web2 users. By addressing the outlined challenges and focusing on user onboarding experience, we believe SoulETH can significantly contribute to the wider adoption of Web3.
