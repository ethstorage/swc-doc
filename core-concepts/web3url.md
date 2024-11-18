## Motivation
With the advancements in Ethereum scalability, including our “Super World Computer” initiative, the ecosystem is progressing toward broad adoption of fully on-chain applications. However, a critical component remains underdeveloped: a decentralized protocol that enables direct access to on-chain resources, such as NFT images and dynamic websites hosted within smart contracts.

To address this need, we have introduced a new standard for defining HTTP-style web3:// links through ERC-4804 and ERC-6860. This protocol allows seamless navigation to dynamic on-chain resources governed by smart contracts, effectively transforming the Ethereum Virtual Machine (EVM) into a decentralized, unstoppable HTTP server.

When combined with the programmable storage provided by EthStorage, this approach lays the groundwork for **a fully decentralized web** (Web3), a reimagined internet that replaces centralized entities with permissionless protocols. Our aim is to establish a resilient, censorship-resistant infrastructure that inherently provides direct access to decentralized resources.

## How It Works

### URL structure

```
web3://<contract>[:<chainId>]/<path>
```

The URLs are following a structure close to traditional HTTP URLs:

- ``<contract>`` can either be an contract address such as ``0x5ad14e8439b9619e165db27545faf6df13e2b947`` or a domain name such as ``web3url.eth``. Learn more about [domain name resolution](https://docs.web3url.io/web3-url-structure/domain-name).
- ``chainId`` is optional and indicate the chain id of the blockchain where to query the smart contract. ``web3://0x5a985f13345e820aa9618826b85f74c3986e1463:5/tokenHTML/2`` will for example query on the goerli blockchain (chain id = 5).
- ``path`` follows a similar structure than traditional HTTP URLs, in the form of ``/path/path2?query1=xx&query2=xx``. To know how to build a path, we first need to know the [**resolve mode** of the called smart contract](https://docs.web3url.io/web3-url-structure/resolve-mode)


### Standards

The ``web3://`` protocol is made of various Ethereum ERCs, some of which are still in draft status. We will flag on this documentation what is definitive and what could change.

Here is the list of ERCs : 

- [ERC-4804](https://eips.ethereum.org/EIPS/eip-4804): The base ERC from which everything is based on. This ERC is final and cannot be edited anymore.
- [ERC-6860](https://eips.ethereum.org/EIPS/eip-6860): (Draft) This ERC updates the base [ERC-4804](https://eips.ethereum.org/EIPS/eip-4804) with clarifications, minor fixes and changes. Still in draft status.
- [ERC-6821](https://eips.ethereum.org/EIPS/eip-6821): (Draft) ENS resolution : support for the ``contentcontract`` TXT field to point to a contract in another chain. Still in draft status.
- [ERC-6944](https://eips.ethereum.org/EIPS/eip-6944): (Draft) New resolve mode offloading some parsing processing on the browser side, based on [ERC-5219](https://eips.ethereum.org/EIPS/eip-5219). Still in draft status.
- [ERC-7087](https://eips.ethereum.org/EIPS/eip-7087) : (Draft) Auto mode : Add MIME type support. Still in draft status.
- [ERC-7617](https://eips.ethereum.org/EIPS/eip-7617) : (Draft) Add chunk support in ERC-6944 resource request mode. Still in pending merge status.
- [ERC-7618](https://eips.ethereum.org/EIPS/eip-7618) : (Draft) Add Content-encoding handling in ERC-6944 resource request mode. Still in pending merge status.

### Examples

#### Access an on-chain website

```
web3://web3url.eth
```

This on-chain website is located in the ``0x5ad14e8439b9619e165db27545faf6df13e2b947`` smart contract in the QuarkChain L2 testnet blockchain.

> ⏩ Try now with a [web3:// gateway](https://web3url.w3eth.io), or with the others ``web3://`` clients


#### Get a NFT

```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenHTML/9352
```

This URL will fetch the HTML of the NFT number 9352 of the Terraforms NFT collection located at [``0x4e1f41613c9084fdb9e34e11fae9412427480e56``](https://etherscan.io/address/0x4e1f41613c9084fdb9e34e11fae9412427480e56) on the Ethereum mainnet blockchain.

> ⏩ Try now with a [web3:// gateway](https://0x4e1f41613c9084fdb9e34e11fae9412427480e56.w3eth.io/tokenHTML/9352), or with the others ``web3://`` clients


#### Fetch an USDC balance

```
web3://0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48/balanceOf/nemorino.eth?returns=(uint256)
```

This URL will fetch the balance of USDC of the account ``nemorino.eth``.

> ⏩ Try now with a [web3:// gateway](https://0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48.w3eth.io/balanceOf/nemorino.eth?returns=(uint256)), or with the others ``web3://`` clients

## Additional Resources
 - [Offical Website](https://web3url.io)
 - [Full Documentation](https://docs.web3url.io)
