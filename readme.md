# Summary


Learn how to build fast, scalable, and secure dApps on Kava. Kava is EVM compatible.

### What is Kava EVM?

Kava is a lighting fast blockchain built with the Cosmos SDK that includes Kava EVM, an EVM compatible smart contract execution platform. Kava's best-in-class Tendermint based BFT consensus engine allows Kava to be quick, cheap, and secure.

|   |  |
| ------------- | ------------- |
| Speed | With single block finality and unrivalled scalability, Tendermint Consensus enables Kava to support the transaction needs of thousands of protocols and millions of users. |
| Interoperability | Kava is connected to the 30 chains and \$60B+ of the Cosmos ecosystem via the IBC protocol.  |
| Smart Contract Support | Kava EVM is fully compatible with Ethereum, offering an execution environment that empowers Solidity developers and their dApps to benefit from the scalability and security of the Kava Network.  |


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

## Contract Deployment

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

## Contract Interaction
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