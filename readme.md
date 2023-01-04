# Summary


Learn how to build fast, scalable, and secure dApps on Kava. Kava is EVM compatible.

### What is Kava EVM?

Kava is a lighting fast blockchain built with the Cosmos SDK that includes Kava EVM, an EVM compatible smart contract execution platform. Kava's best-in-class Tendermint based BFT consensus engine allows Kava to be quick, cheap, and secure.

### Speed

With single block finality and unrivalled scalability, Tendermint Consensus enables Kava to support the transaction needs of thousands of protocols and millions of users.

### Interoperability

Kava is connected to the 30 chains and \$60B+ of the Cosmos ecosystem via the IBC protocol.

### Smart Contract Support

Kava EVM is fully compatible with Ethereum, offering an execution environment that empowers Solidity developers and their dApps to benefit from the scalability and security of the Kava Network.


# Connect to Metamask

To see full details: [Click Here](https://docs.kava.io/docs/ethereum/metamask)

# Development Tutorial (Kava Local)
Kava provides tools for easily setting up a local Kava development environment for Solidity smart contract deployment and interaction.

### kvtool

kvtool enables developers to spin up a local Kava development environment. A dockerized local testnet with ethermint EVM can be created with:

```
kvtool testnet bootstrap --kava.configTemplate master 
```

