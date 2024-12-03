## Motivation

The Ethereum Cancun upgrade has significantly reduced Layer 2 (L2) data uploading costs by introducing BLOB
transactions to Layer 1 (L1). This innovation has also enabled a variety of additional applications based on
the BLOBs due to their low cost, such as [blob.fm](https://blob.fm/), [EthStorage](https://ethstorage.io), and
[Ethscriptions](https://ethscriptions.com/). However, while the data upload costs have decreased, the execution
costs on L1 remain high compared to L2, leading to high costs for L2 state proposals and non-financial applications
that rely on BLOBs.

To address these challenges, the L2 BLOB feature introduces support for BLOB transactions on L2. enabling L3 solutions that settle on L2 to use an enshrined 4844-compatible DA layer without needing to
integrate third-party DA providers or deal with the security risks associated with DA bridges. Additionally, the
applications mentioned above could migrate to L2 with minimal costs.

Furthermore, the L2 BLOB feature uses
[Alt-DA](https://github.com/ethereum-optimism/specs/blob/main/specs/experimental/alt-da.md) to upload L2 BLOBs while
still using L1 DA for L2 calldata. This approach, referred to as a “hybrid DA L2”, combines the best features of
different DA solutions. This allows
users and applications of an L2 to choose between L1 DA and alt-DA for different types of transaction data within the
same network, without the need to maintain multiple L2s. Specifically, users can upload and store non-financial data
at a very low cost using L2 BLOBs and Alt-DA, while still conducting critical financial data using L2 calldata and
L1 DA. In some cases, these two types of data may even occur within the same transaction. Here are a few potential
scenarios:

- While the L2 continues to use L1 DA for uploading calldata, multiple app-specific or game-focused
L3s settled on it can directly use the enshrined 4844-compatible L2 DA layer, benefiting from easy integration, robust
security, and lower costs.
- Users might use a platform like Decentralized Twitter primarily for social networking (non-financial), while
also sending payments (financial) to other users within the same application.

## How It Works
The following diagram illustrates the transaction data flow for a hybrid DA L2:

<img src="https://0x1fd7ca75e5495a1f1635561297a2c90c27e7635f.3336.w3link.io/L2-BLOB.png" alt="Alt Text" width="600">

This hybrid approach provides a versatile, cost-effective solution for data availability across diverse applications on L2, supporting Ethereum’s ongoing scalability efforts.

