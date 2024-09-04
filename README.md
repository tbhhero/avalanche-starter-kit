# Avalanche Starter Kit

This starter kit will get you started with developing solidity smart contract dApps on the C-Chain or on an Avalanche Subnet. It provides all tools to build cross-Subnet dApps using Teleporter. It includes:

- **Avalanche CLI**: Run a local Avalanche Network
- **Foundry**:
  - Forge: Compile and Deploy smart contracts to the local network, Fuji Testnet or Mainnet
  - Cast: Interact with these smart contracts
- **Teleporter**: All contracts you may want to interact with Teleporter
- **AWM Relayer**: The binary to run your own relayer
- **Examples**: Contracts showcasing how to achieve common patterns, such as sending simple messages, call functions of a contract on another blockchain and bridging assets. Please note that these example contracts have not been audited and are for educational purposes only

## Set Up

This starter kit utilizes a Dev Container specification. Dev Containers use containerization to create consistent and isolated development environments. All of the above mentioned components are pre-installed in that container. These containers can be run using GitHub Codespaces or locally using Docker and VS Code. You can switch back and forth between the two options.

### Run on Github Codespace

You can run them directly on Github by clicking **Code**, switching to the **Codespaces** tab and clicking **Create codespace on main**. A new window will open that loads the codespace. Afterwards you will see a browser version of VS code with all the dependencies installed. Codespace time out after some time of inactivity, but can be restarted.

### Run Dev Container locally with Docker

Alternatively, you can run them locally. You need [docker](https://www.docker.com/products/docker-desktop/) installed and [VS Code](https://code.visualstudio.com/) with the extensions [Dev Container extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers). Then clone the repository and open it in VS Code. VS Code will ask you if you want to reopen the project in a container.

If you are running on Apple Silicon you may run into issues while opening and running your dev container in VSCode. The issue resides in Foundry platform targeting. The fix is currently in draft: <https://github.com/foundry-rs/foundry/pull/7512>

To workaround, please edit the file `Dockerfile` to include `--platform linux/amd64` before pulling Foundry.

```
# .devcontainer/Dockerfile
FROM avaplatform/avalanche-cli:latest as avalanche-cli
FROM avaplatform/awm-relayer:latest as awm-relayer
FROM --platform=linux/amd64 ghcr.io/foundry-rs/foundry:latest as foundry
...
```

This should allow you to open the dev container in VSCode.

## Starting a local Avalanche Network

To start a local Avalanche network with your own teleporter-enabled Subnet inside the container follow these commands. Your Avalanche network will be completely independent of the Avalanche Mainnet and Fuji Testnet. It will have its own Primary Network (C-Chain, X-Chain & P-Chain). You will not have access to services available on Fuji (such as Chainlink services or bridges). If you require these, go to the [Fuji Testnet](#fuji-testnet) section.

First let's create out Subnet configuration. Follow the dialog and if you don't have special requirements for precompiles just follow the suggested options. For the Airdrop of the native token select "Airdrop 1 million tokens to the default ewoq address (do not use in production)". Keep the name "mysubnet" to avoid additional configuration.

```
avalanche subnet create mysubnet
```

Now let's spin up the local Avalanche network and deploy our Subnet. This will also deploy the Teleporter messenger and the registry on our Subnet and the C-Chain.

```bash
avalanche subnet deploy mysubnet
```

Make sure to add the RPC Url to the `foundry.toml` file if you have chosen a different name than `mysubnet`. If you've used `mysubnet` the rpc is already configured.

```toml
[rpc_endpoints]
local-c = "http://localhost:9650/ext/bc/C/rpc"
mysubnet = "http://localhost:9650/ext/bc/mysubnet/rpc"
anothersubnet = "http://localhost:9650/ext/bc/BASE58_BLOCKCHAIN_ID/rpc"
```

## Code Examples

### Interchain Messaging
- [0a-send-receive](src/interchain-messaging/0a-send-receive/_INSTRUCTIONS.md) 
- [0b-send-receive](src/interchain-messaging/0b-send-receive/_INSTRUCTIONS.md) 
- [1-send-roundtrip](src/interchain-messaging/1-send-roundtrip/_INSTRUCTIONS.md)
- [2a-invoking-functions](src/interchain-messaging/2a-invoking-functions/_INSTRUCTIONS.md)
- [2b-invoking-functions](src/interchain-messaging/2b-invoking-functions/_INSTRUCTIONS.md)
- [2c-invoking-functions](src/interchain-messaging/2c-invoking-functions/_INSTRUCTIONS.md)
- [3-registry](src/interchain-messaging/3-registry/_INSTRUCTIONS.md)
- [4-incentivize-relayer](src/interchain-messaging/4-incentivize-relayer/_INSTRUCTIONS.md)

### Interchain Token Transfer
- [0-native-to-erc20-interchain-token-transfer](src/interchain-token-transfer/0-native-to-erc20-interchain-token-transfer/_INSTRUCTIONS.md)
- [1-erc20-to-native-interchain-token-transfer](src/interchain-token-transfer/1-erc20-to-native-interchain-token-transfer/_INSTRUCTIONS.md)
- [2-native-to-native-interchain-token-transfer](src/interchain-token-transfer/2-native-to-native-interchain-token-transfer/_INSTRUCTIONS.md)
- [3-erc20-to-erc20-interchain-token-transfer](src/interchain-token-transfer/3-erc20-to-erc20-interchain-token-transfer/_INSTRUCTIONS.md)
- [4-send-and-call](src/interchain-token-transfer/4-send-and-call/_INSTRUCTIONS.md)
