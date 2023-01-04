# Summary


Learn how to build fast, scalable, and secure dApps on Kava. Kava is EVM compatible.

### What is Kava EVM?

Kava is a lighting fast blockchain built with the Cosmos SDK that includes Kava EVM, an EVM compatible smart contract execution platform. Kava's best-in-class Tendermint based BFT consensus engine allows Kava to be quick, cheap, and secure.

|   |  |
| ------------- | ------------- |
| Speed | With single block finality and unrivalled scalability, Tendermint Consensus enables Kava to support the transaction needs of thousands of protocols and millions of users. |
| Interoperability | Kava is connected to the 30 chains and \$60B+ of the Cosmos ecosystem via the IBC protocol.  |
| Smart Contract Support | Kava EVM is fully compatible with Ethereum, offering an execution environment that empowers Solidity developers and their dApps to benefit from the scalability and security of the Kava Network.  |

## Outline
- [Development Tutorial](#development-tutorial)
- [Contract Verification](#contract-verification)
- [The Graph](#the-graph)
- [Address Converter](#address-converter)


# Connect to Metamask

To see full details: [Click Here](https://docs.kava.io/docs/ethereum/metamask)

# Development Tutorial (Kava Local)
Kava provides tools for easily setting up a local Kava development environment for Solidity smart contract deployment and interaction.

### kvtool

kvtool enables developers to spin up a local Kava development environment. A dockerized local testnet with ethermint EVM can be created with:

```
kvtool testnet bootstrap --kava.configTemplate master 
```

The ```faucet``` Ethereum account is funded with sufficient gas tokens to be used in EVM transactions such as deploying smart contracts. Configuration file ```hardhat.config.js``` contains field ```networks.local.accounts``` which holds an array of private keys, including the private key for ```faucet```. The private key was exported with:

```
# Add an alias to the dockerized kava blockchain
alias dkava='docker exec -it generated_kavanode_1 kava'

# Export private key for named account 'faucet'
dkava keys unsafe-export-eth-key faucet --keyring-backend test
```

## 1. Contract Deployment

kvtool's ```evm``` directory contains an integrated Hardhat project for smart contract deployment and interaction.

```
# Navigate to the evm directory
cd evm

# Install dependencies
yarn
```

A sample Solidity smart contract located at ```evm/contracts/token/Token.sol``` can be easily deployed with:

```
yarn token-deploy
# TEST token deployed to: 0xff994EcA9074305D353E78a7159fE407537Cf87C
```

## 2. Contract Interaction
Basic contract interaction is possible through additional scripts with the full list available in ```evm/package.json```. Each yarn cmd is aliased to one of the Hardhat tasks found in ```hardhat.config.js```.

```
# Get the deployed TEST token's total supply
yarn token-totalSupply --token [TOKEN_CONTRACT_ADDRESS]
# Total Supply is 100000000
```

You can get information about any cmd including param options by using the ```--help``` flag, for example:

```
You can get information about any cmd including param options by using the --help flag, for example:
```

Using the available scripts, we can transfer tokens:
```
yarn token-transfer --token [TOKEN_CONTRACT_ADDRESS] --recipient [RECIPIENT_ADDRESS] --amount [AMOUNT]

# We can build a valid cmd by plugging in the relevant params:
yarn token-transfer --token 0xff994EcA9074305D353E78a7159fE407537Cf87C --recipient 0xdd2fd4581271e230360230f9337d5c0430bf44c0 --amount 50
# 0x5452643Ffa7Ab2BE7EF7C376f62FDfA7804Ed1d2 has transferred 50 TEST to 0xdd2fd4581271e230360230f9337d5c0430bf44c0
```

## 3. Contract Interaction
Basic contract interaction is possible through additional scripts with the full list available in ```evm/package.json```. Each yarn cmd is aliased to one of the Hardhat tasks found in ```hardhat.config.js```.

```
# Get the deployed TEST token's total supply
yarn token-totalSupply --token [TOKEN_CONTRACT_ADDRESS]
# Total Supply is 100000000
```
You can get information about any cmd including param options by using the ```--help``` flag, for example:
```
yarn token-totalSupply --help
```
Using the available scripts, we can transfer tokens:
```
yarn token-transfer --token [TOKEN_CONTRACT_ADDRESS] --recipient [RECIPIENT_ADDRESS] --amount [AMOUNT]

# We can build a valid cmd by plugging in the relevant params:
yarn token-transfer --token 0xff994EcA9074305D353E78a7159fE407537Cf87C --recipient 0xdd2fd4581271e230360230f9337d5c0430bf44c0 --amount 50
# 0x5452643Ffa7Ab2BE7EF7C376f62FDfA7804Ed1d2 has transferred 50 TEST to 0xdd2fd4581271e230360230f9337d5c0430bf44c0
```

## 4. Deploying your own contracts
You can easily deploy your own contracts:

1. Add your Solidity smart contract to ```evm/contracts```.
2. Add a deployment script to ```evm/scripts/deploy/[YOUR_SCRIPT].js``` modeled on ```evm/scripts/deploy/token.js```.
3. Add Hardhat tasks corresponding to your contract's public methods to the existing tasks in ```hardhat.config.js```.

Done! Now you can deploy and interact with your contract;:
```
# Deployment
npx hardhat run --network local scripts/deploy/[YOUR_SCRIPT].js

# Interaction
npx hardhat [YOUR_TASK_NAME] --network local
```

If you want a cleaner CLI experience, you can add some script aliases to evm/package.json as seen in the ```scripts``` section.


# Contract Verification
Once you have deployed your smart contracts, you can verify them on the [explorer](https://explorer.kava.io/). The following guide uses [hardhat](https://hardhat.org/).

## Via Flattened Source Code

1. Use the following command to get a flattened source code of your smart contract:
```
npx hardhat flatten path/to/your/contract.sol
```
2. [Find your deployed contract on the Kava explorer and follow the steps to verify](https://docs.blockscout.com/for-users/verifying-a-smart-contract). If your contract has constructor arguments, ensure that you provide them in the form. If you do not already have an ABI-encoded version of it, you can use this [online ABI encoding tool](https://docs.blockscout.com/for-users/verifying-a-smart-contract) to manually create it. Additional information about ABI-Encoded constructor arguments can be found on the [Blockscout documentation](https://docs.blockscout.com/for-users/abi-encoded-constructor-arguments).

## Via Hardhat Verification Plugin
You can also verify directly from your Hardhat development environment. This requires the [hardhat-etherscan plugin](https://hardhat.org/hardhat-runner/plugins/nomiclabs-hardhat-etherscan). The Kava explorer uses [BlockScout](https://github.com/blockscout/blockscout) which is compatible with the Hardhat Etherscan plugin.

1. [Install the Hardhat Etherscan plugin](https://hardhat.org/hardhat-runner/plugins/nomiclabs-hardhat-etherscan), ensuring that the version is ```v3.1.0``` or higher.
2. Ensure your ```hardhat.config.js``` file includes the following ```etherscan``` configuration:

```
hardhat.config.js
```
```
module.exports = {
  networks: {
    kava: {
      url: 'https://evm.kava.io',
    },
  },
  etherscan: {
    apiKey: {
      kava: "api key is not required by the Kava explorer, but can't be empty",
    },
    customChains: [
      {
        network: 'kava',
        chainId: 2222,
        urls: {
          apiURL: 'https://explorer.kava.io/api',
          browserURL: 'https://explorer.kava.io',
        },
      },
    ],
  },
};
```
3. Verify that the custom network is configured correctly, by checking if kava is listed in the supported custom networks.

```
$ npx hardhat verify --list-networks

Networks supported by hardhat-etherscan:

...

Custom networks added by you or by plugins:

╔═════════╤══════════╗
║ network │ chain id ║
╟─────────┼──────────╢
║ kava    │ 2222     ║
╚═════════╧══════════╝
```
4. Verify the contract with ```npx hardhat verify```. This may take a few minutes. The following is an example of verifying an WKAVA contract.
```
$ npx hardhat verify --network kava 0x52A87b0703C65a4C30657e7badc808855711CdEc

Nothing to compile
Generating typings for: 0 artifacts in dir: typechain-types for target: ethers-v5
Successfully generated 3 typings!
Successfully generated 3 typings for external artifacts!
Compiling 1 file with 0.8.9
Successfully submitted source code for contract
contracts/WKAVA.sol:WKAVA at 0x52A87b0703C65a4C30657e7badc808855711CdEc
for verification on the block explorer. Waiting for verification result...

Successfully verified contract WKAVA on Etherscan.
https://explorer.kava.io/address/0x52A87b0703C65a4C30657e7badc808855711CdEc#code
```
## Troubleshooting
Here are some common errors and things to look out for when verifying your contracts.

**Bytecode does not match, please try again.**

Ensure the following options are the exact same as what you deployed with, then try again.

- Compiler version
- Optimization runs
- ABI-encoded constructur arguments

**There was an error compiling your contract: Multiple SPDX license identifiers found in source file**

Flattening your contracts may sometimes merge multiple dependent contracts also with SPX identifiers (e.g. // SPDX-License-Identifier: GPL-3.0).

Remove all SPDX comments except one in your flattened file and try verifying again. Alternatively, use the hardhat verification plugin to preserve license identifiers

## Resources
- [Hardhat Verification Plugin](https://hardhat.org/hardhat-runner/plugins/nomiclabs-hardhat-etherscan)
- [BlockScout Documentation](https://docs.blockscout.com/)
- [Online ABI encoding service](https://abi.hashex.org/)

# The Graph

## Important Note

The Graph nodes provided by Kava is only a sandbox. The Graph admin endpoint is made public to allow anyone to be able to use it for testing. Projects should run their own Graph instances in production and avoid exposing the admin endpoint.

## Kava Mainnet

A hosted [The Graph](https://thegraph.com/en/) node is available to use on Kava EVM.

- JSON-RPC Admin (Deploy subgraphs): https://the-graph-admin.kava.io
- GraphQL HTTP Server (Query subgraphs): https://the-graph.kava.io
- Query Metrics: https://the-graph-metrics.kava.io/graphql

[An example deployed subgraph](https://the-graph.evm-alpha.kava.io/subgraphs/name/Kava-Labs/v2-uniswap/graphql) is available for V2 Uniswap on Kava EVM Testnet.


## Deploying Subgraphs

### Install graph-cli

https://github.com/graphprotocol/graph-cli#installation

### Write your subgraph

See https://thegraph.com/docs/en/developer/create-subgraph-hosted/.

You can also use an example project like [v2-uniswap](https://github.com/Uniswap/v2-subgraph).

### Upgrade the manifest

Update the network properties in the ```subgraph.yaml``` manifest file. The network name for Kava EVM is ```kava-evm```.

### Create your subgraph

```
graph create <subgraph-name> --node https://the-graph-admin.kava.io
```

### Deploy your subgraph
```
graph deploy <subgraph-name> --debug --ipfs https://api.thegraph.com/ipfs/ --node https://the-graph-admin.kava.io
```

Note that you can either use your own ipfs node here or https://api.thegraph.com/ipfs/.

Once deployed, your graph should now be deployed and accessible via the GraphQL Server. It can be access at ```https://the-graph.kava.io/subgraphs/name/<subgraph-name>/graphql```.

## Testnet
A hosted The Graph node is available on Kava EVM Testnet.

- JSON-RPC Admin (Deploy subgraphs): https://the-graph-admin.evm-alpha.kava.io
- GraphQL HTTP Server (Query subgraphs): https://the-graph.evm-alpha.kava.io

The ```network``` name for Kava EVM Testnet is ```kava-evm-testnet```.


# Address Converter

To convert address: [Click Here](https://docs.kava.io/docs/ethereum/address_conversion)