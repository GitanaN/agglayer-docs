---
title: Client Initialization
---

<!-- Page Header Component -->
<h1 style="text-align: left; font-size: 38px; font-weight: 700; font-family: 'Inter Tight', sans-serif;">
  Client Initialization
</h1>

<div style="text-align: left; margin: 0.5rem 0;">
  <p style="font-size: 18px; color: #666; max-width: 600px; margin: 0;">
    Configure and initialize the Lxly.js client for cross-chain operations
  </p>
</div>

## Overview

Client initialization is the first step in using Lxly.js. The client manages connections to multiple blockchain networks, handles provider configuration, and sets up the necessary infrastructure for cross-chain operations.

## Basic Initialization

### Web3.js Provider

```javascript
const { LxLyClient, use } = require('@maticnetwork/lxlyjs');
const { Web3ClientPlugin } = require('@maticnetwork/maticjs-web3');
const HDWalletProvider = require('@truffle/hdwallet-provider');

// Enable Web3.js support
use(Web3ClientPlugin);

const client = new LxLyClient();

await client.init({
  log: true,  // Enable logging for debugging
  network: 'testnet',  // 'testnet' or 'mainnet'
  providers: {
    0: {  // Ethereum/Sepolia
      provider: new HDWalletProvider([privateKey], rpcUrl),
      configuration: {
        bridgeAddress: '0x528e26b25a34a4A5d0dbDa1d57D318153d2ED582',
        wrapperAddress: '0x0f04f8434bac2e1db8fca8a34d3e177b6c7ccaba',
        isEIP1559Supported: true
      },
      defaultConfig: {
        from: userAddress
      }
    },
    1: {  // Polygon zkEVM/Cardona
      provider: new HDWalletProvider([privateKey], rpcUrl),
      configuration: {
        bridgeAddress: '0x528e26b25a34a4A5d0dbDa1d57D318153d2ED582',
        isEIP1559Supported: false
      },
      defaultConfig: {
        from: userAddress
      }
    }
  }
});
```

### Ethers.js Provider

```javascript
const { LxLyClient, use } = require('@maticnetwork/lxlyjs');
const { Web3ClientPlugin } = require('@maticnetwork/maticjs-ethers');
const { providers, Wallet } = require('ethers');

// Enable Ethers.js support
use(Web3ClientPlugin);

const client = new LxLyClient();

await client.init({
  log: true,
  network: 'testnet',
  providers: {
    0: {
      provider: new Wallet(privateKey, new providers.JsonRpcProvider(rpcUrl)),
      configuration: {
        bridgeAddress: '0x528e26b25a34a4A5d0dbDa1d57D318153d2ED582',
        wrapperAddress: '0x0f04f8434bac2e1db8fca8a34d3e177b6c7ccaba',
        isEIP1559Supported: true
      },
      defaultConfig: {
        from: userAddress
      }
    },
    1: {
      provider: new Wallet(privateKey, new providers.JsonRpcProvider(rpcUrl)),
      configuration: {
        bridgeAddress: '0x528e26b25a34a4A5d0dbDa1d57D318153d2ED582',
        isEIP1559Supported: false
      },
      defaultConfig: {
        from: userAddress
      }
    }
  }
});
```

## Configuration Options

### Provider Configuration

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `provider` | Web3Provider | ✅ | Web3 provider instance (HDWalletProvider, JsonRpcProvider, etc.) |
| `configuration.bridgeAddress` | string | ✅ | Address of the bridge contract on this network |
| `configuration.wrapperAddress` | string | ❌ | Address of the wrapper contract (for certain operations) |
| `configuration.isEIP1559Supported` | boolean | ❌ | Whether the network supports EIP-1559 transactions |
| `defaultConfig.from` | string | ✅ | Default sender address for transactions |

### Client Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `log` | boolean | `false` | Enable debug logging |
| `network` | string | Required | Network environment ('testnet' or 'mainnet') |
| `providers` | object | Required | Provider configurations for each network |
| `requestConcurrency` | number | `10` | Maximum concurrent requests |

## Complete Example

Here's a complete initialization example with error handling:

```javascript
const { LxLyClient, use, setProofApi } = require('@maticnetwork/lxlyjs');
const { Web3ClientPlugin } = require('@maticnetwork/maticjs-web3');
const HDWalletProvider = require('@truffle/hdwallet-provider');

// Load configuration
const config = require('./config');

// Enable Web3.js support
use(Web3ClientPlugin);

// Set proof API for merkle proof generation
setProofApi('https://api-gateway.polygon.technology/api/v3/proof/testnet/');

async function initializeClient() {
  try {
    const client = new LxLyClient();
    
    await client.init({
      log: true,
      network: config.network,
      providers: {
        0: {
          provider: new HDWalletProvider(
            [config.user1.privateKey], 
            config.configuration[0].rpc
          ),
          configuration: {
            bridgeAddress: config.configuration[0].bridgeAddress,
            wrapperAddress: config.configuration[0].wrapperAddress,
            isEIP1559Supported: config.configuration[0].isEIP1559Supported
          },
          defaultConfig: {
            from: config.user1.address
          }
        },
        1: {
          provider: new HDWalletProvider(
            [config.user1.privateKey], 
            config.configuration[1].rpc
          ),
          configuration: {
            bridgeAddress: config.configuration[1].bridgeAddress,
            isEIP1559Supported: config.configuration[1].isEIP1559Supported
          },
          defaultConfig: {
            from: config.user1.address
          }
        }
      }
    });

    console.log('✅ Client initialized successfully');
    return client;
    
  } catch (error) {
    console.error('❌ Client initialization failed:', error);
    throw error;
  }
}

// Export for use in other modules
module.exports = { initializeClient };
```

## Advanced Configuration

### Multiple Accounts

```javascript
const providers = {
  0: {
    provider: new HDWalletProvider(
      [account1PrivateKey, account2PrivateKey], 
      rpcUrl
    ),
    configuration: {
      bridgeAddress: '0x...',
      isEIP1559Supported: true
    },
    defaultConfig: {
      from: account1Address  // Default account
    }
  }
};

// Switch accounts for specific operations
const token = client.erc20('0xTokenAddress', 0);
await token.bridgeAsset('100', account2Address, 1, {
  from: account2Address  // Override default account
});
```

### Custom Gas Configuration

```javascript
const providers = {
  0: {
    provider: customProvider,
    configuration: {
      bridgeAddress: '0x...',
      isEIP1559Supported: true
    },
    defaultConfig: {
      from: userAddress,
      gasLimit: 300000,  // Custom gas limit
      maxFeePerGas: '20000000000',  // 20 gwei
      maxPriorityFeePerGas: '2000000000'  // 2 gwei
    }
  }
};
```

## Environment-Specific Setup

### Development Environment

```javascript
// development.js
const config = {
  network: 'testnet',
  log: true,  // Enable detailed logging
  providers: {
    // Use public RPCs for development
    0: {
      provider: new HDWalletProvider([devPrivateKey], 'https://eth-sepolia.g.alchemy.com/v2/demo'),
      // ... configuration
    }
  }
};
```

### Production Environment

```javascript
// production.js
const config = {
  network: 'mainnet',
  log: false,  // Disable logging in production
  requestConcurrency: 5,  // Limit concurrent requests
  providers: {
    // Use private, reliable RPCs
    0: {
      provider: new HDWalletProvider([process.env.PRIVATE_KEY], process.env.ETHEREUM_RPC),
      // ... configuration
    }
  }
};
```

### Testing Environment

```javascript
// test.js
const config = {
  network: 'testnet',
  log: true,
  providers: {
    // Use local test networks or dedicated test RPCs
    0: {
      provider: new HDWalletProvider([testPrivateKey], 'http://localhost:8545'),
      // ... configuration
    }
  }
};
```

## Validation and Testing

### Connection Test

```javascript
async function validateConnection(client) {
  try {
    // Test network connectivity
    const token = client.erc20('0x0000000000000000000000000000000000000000', 0);
    const balance = await token.getBalance(userAddress);
    console.log('✅ Network 0 connected, balance:', balance);
    
    // Test bridge connectivity
    const bridgeAddress = token.getBridgeAddress();
    console.log('✅ Bridge address:', bridgeAddress);
    
    return true;
  } catch (error) {
    console.error('❌ Connection test failed:', error);
    return false;
  }
}
```

### Configuration Validation

```javascript
function validateConfiguration(config) {
  const required = ['network', 'providers'];
  const missing = required.filter(key => !config[key]);
  
  if (missing.length > 0) {
    throw new Error(`Missing required configuration: ${missing.join(', ')}`);
  }
  
  // Validate provider configurations
  Object.entries(config.providers).forEach(([networkId, provider]) => {
    if (!provider.configuration?.bridgeAddress) {
      throw new Error(`Missing bridgeAddress for network ${networkId}`);
    }
    if (!provider.defaultConfig?.from) {
      throw new Error(`Missing from address for network ${networkId}`);
    }
  });
  
  console.log('✅ Configuration validated');
}
```