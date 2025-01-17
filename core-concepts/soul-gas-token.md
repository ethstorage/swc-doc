## **Motivation**

To bridge the gap between traditional web users and the growing world of Web3, we propose a **non-transferable gas token** named Soul Gas Token (SGT), where the gas token is the native token of a Rollup. The concept revolves around facilitating Web2 users' entry into Web3 by airdropping them with SGT. This token will enable users to pay for transaction gas fees without the immediate selling pressure of the airdropped token. This initiative is particularly aimed at those new to Web3, providing a seamless transition without the upfront cost of acquiring a gas token.

## **How It Works (Using OP Stack as a Case Study)**

- **Technical Implementation**: **SGT is managed through an ERC20 contract deployed on the L2 with a twist**: the chain operator or governance can **`mint`** new tokens by deposit SGT. To maintain its non-transferable nature, all attempts to use ERC20's **`transfer`**, **`transferFrom`**, or **`approve`** methods will result in failure.
- **Wallet Compatibility**: The transaction format remains consistent with existing Rollup ones. Users can transact using ETH wallets without the need for additional ones (e.g., AA wallets), ensuring a smooth user experience.
- **Gas Fee Process**: For an L2 transaction, the fee will first be deducted from the user's SGT balance in the ERC20 contract. If the balance is insufficient, the system will then draw from the user's balance. To account for the additional processing cost of SGT, the base gas cost may be slightly higher than traditional transactions, likely from 21,000 to 21,000+2,100+5,000=28,100 gas. Note that the token transferred in `msg.value` will be always from the user balance, preventing transferability of SGT token.

## **Potential Challenges**

- **Sequencer Incentives**:  The sequencer will receive the gas fee in SGT as `used_price * gas_price`.
- **Risk of DoS Attacks**: The introduction of a "free" transaction token like SoulETH could potentially incur denial-of-service (DoS) type attacks. Mitigation strategies include limiting the initial airdrop quantity to support a finite number of transactions, coupled with a mechanism to replenish SGT for users with verified on-chain behaviors.
- **Sybil Attack Prevention**: Airdrops are susceptible to exploitation through Sybil attacks. We will implement established best practices of airdrops to identify and mitigate such threats effectively.

This proposal outlines a novel approach to lowering the entry barrier into Web3, leveraging SoulETH as a transitional gas token for Web2 users. By addressing the outlined challenges and focusing on user onboarding experience, we believe SoulETH can significantly contribute to the wider adoption of Web3.

## Comparison with [SoulETH](https://www.notion.so/acbc11492bc145849c470a8ce5114128?pvs=21)

|                  | SoulETH        | SoulGT                                                                     |
| ---------------- | -------------- | -------------------------------------------------------------------------- |
| Mint             | Only sequencer | Anyone by depositing SGT (e.g., the foundation airdrops SGT by depositing) |
| Sequencer reward | None           | used_price * gas_price                                                     |
| Transferability  | None           | None                                                                       |

## Concluding Remarks

This proposal outlines a novel approach to lowering the entry barrier into Web3, leveraging SGT as a transitional gas token for Web2 users. By addressing the outlined challenges and focusing on user onboarding experience, we believe SGT can signifiQcantly contribute to the wider adoption of Web3.