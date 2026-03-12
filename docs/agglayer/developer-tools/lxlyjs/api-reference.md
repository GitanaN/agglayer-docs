---
title: API Reference
---

<!-- Page Header Component -->
<h1 style="text-align: left; font-size: 38px; font-weight: 700; font-family: 'Inter Tight', sans-serif;">
  API Reference
</h1>

<div style="text-align: left; margin: 0.5rem 0;">
  <p style="font-size: 18px; color: #666; max-width: 600px; margin: 0;">
    Complete API documentation for all Lxly.js methods, parameters, and return types
  </p>
</div>

## LxLyClient

The main client class for managing cross-chain operations.

### Constructor

```typescript
new LxLyClient()
```

Creates a new Lxly.js client instance.

### Methods

#### `init(config: IBaseClientConfig): Promise<LxLyClient>`

Initializes the client with network providers and configuration.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `config.network` | `string` | Network environment ('testnet' or 'mainnet') |
| `config.providers` | `IProviders` | Provider configurations for each network |
| `config.log` | `boolean?` | Enable debug logging (default: false) |
| `config.requestConcurrency` | `number?` | Max concurrent requests (default: 10) |

**Returns:** Promise resolving to initialized client instance

**Example:**
```javascript
await client.init({
  network: 'testnet',
  providers: {
    0: { provider: ethereumProvider, configuration: { bridgeAddress: '0x...' } },
    1: { provider: polygonProvider, configuration: { bridgeAddress: '0x...' } }
  }
});
```

#### `erc20(tokenAddress: string, networkId: number, bridgeAdapterAddress?: string): ERC20`

Creates an ERC20 token instance for the specified network.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `tokenAddress` | `string` | Token contract address (use '0x0000...' for native ETH) |
| `networkId` | `number` | Network ID where token exists |
| `bridgeAdapterAddress` | `string?` | Custom bridge adapter address (optional) |

**Returns:** ERC20 token instance

**Example:**
```javascript
const token = client.erc20('0x3fd0A53F4Bf853985a95F4Eb3F9C9FDE1F8e2b53', 0);
const ethToken = client.erc20('0x0000000000000000000000000000000000000000', 0);
```

#### `contract(abi: AbiItem[], address: string, networkId: number): Contract`

Creates a custom contract instance.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `abi` | `AbiItem[]` | Contract ABI definition |
| `address` | `string` | Contract address |
| `networkId` | `number` | Network ID where contract is deployed |

**Returns:** Contract instance

#### `isBridgeClaimable(txHash: string, sourceNetwork: number, bridgeIndex?: number): Promise<boolean>`

Checks if a bridge transaction is ready to be claimed.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `txHash` | `string` | Bridge transaction hash |
| `sourceNetwork` | `number` | Source network ID |
| `bridgeIndex` | `number?` | Bridge index in transaction (default: 0) |

**Returns:** Promise resolving to boolean indicating if claimable

#### `isBridged(txHash: string, sourceNetwork: number, destinationNetwork: number, bridgeIndex?: number): Promise<boolean>`

Checks if a bridge transaction has been claimed.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `txHash` | `string` | Bridge transaction hash |
| `sourceNetwork` | `number` | Source network ID |
| `destinationNetwork` | `number` | Destination network ID |
| `bridgeIndex` | `number?` | Bridge index in transaction (default: 0) |

**Returns:** Promise resolving to boolean indicating if claimed

## ERC20

Class for ERC20 token operations.

### Methods

#### `getBalance(userAddress: string, option?: ITransactionOption): Promise<string>`

Gets token balance for a user address.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `userAddress` | `string` | Address to check balance for |
| `option` | `ITransactionOption?` | Transaction options |

**Returns:** Promise resolving to balance string (in wei for ETH, token units for ERC20)

#### `isApprovalNeeded(): Promise<boolean>`

Checks if approval is needed for bridging this token.

**Returns:** Promise resolving to boolean (false for ETH, true for most ERC20s)

#### `getAllowance(userAddress: string, option?: IAllowanceTransactionOption): Promise<string>`

Gets current allowance for the bridge contract.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `userAddress` | `string` | Token owner address |
| `option.spenderAddress` | `string?` | Spender address (defaults to bridge) |

**Returns:** Promise resolving to allowance string

#### `approve(amount: TYPE_AMOUNT, option?: IApproveTransactionOption): Promise<ITransactionWriteResult>`

Approves tokens for bridging.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | `TYPE_AMOUNT` | Amount to approve |
| `option.spenderAddress` | `string?` | Spender address (defaults to bridge) |
| `option.gasLimit` | `number?` | Gas limit override |

**Returns:** Promise resolving to transaction result

#### `approveMax(option?: IApproveTransactionOption): Promise<ITransactionWriteResult>`

Approves maximum amount of tokens.

**Returns:** Promise resolving to transaction result

#### `bridgeAsset(amount: TYPE_AMOUNT, userAddress: string, destinationNetworkId: number, option?: IBridgeTransactionOption): Promise<ITransactionWriteResult>`

Bridges tokens to another network.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | `TYPE_AMOUNT` | Amount to bridge |
| `userAddress` | `string` | Recipient address on destination |
| `destinationNetworkId` | `number` | Destination network ID |
| `option.permitData` | `string?` | Permit data for gasless approval |
| `option.forceUpdateGlobalExitRoot` | `boolean?` | Force GER update (default: true) |

**Returns:** Promise resolving to transaction result

#### `claimAsset(transactionHash: string, sourceNetworkId: number, option?: ITransactionOption): Promise<ITransactionWriteResult>`

Claims bridged assets on destination network.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `transactionHash` | `string` | Original bridge transaction hash |
| `sourceNetworkId` | `number` | Source network ID |
| `option.bridgeIndex` | `number?` | Bridge index (default: 0) |

**Returns:** Promise resolving to transaction result

#### `transfer(amount: TYPE_AMOUNT, to: string, option?: ITransactionOption): Promise<ITransactionWriteResult>`

Transfers tokens to another address on the same network.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | `TYPE_AMOUNT` | Amount to transfer |
| `to` | `string` | Recipient address |
| `option` | `ITransactionOption?` | Transaction options |

**Returns:** Promise resolving to transaction result

## Contract

Class for custom contract interactions.

### Methods

#### `read(method: string, ...args: any[]): Promise<any>`

Calls a read-only contract method.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `method` | `string` | Method name to call |
| `args` | `any[]` | Method parameters |

**Returns:** Promise resolving to method return value

#### `write(option: ITransactionOption, method: string, ...args: any[]): Promise<ITransactionWriteResult>`

Calls a state-changing contract method.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `option` | `ITransactionOption` | Transaction options |
| `method` | `string` | Method name to call |
| `args` | `any[]` | Method parameters |

**Returns:** Promise resolving to transaction result

#### `encodeAbi(method: string, ...args: any[]): Promise<string>`

Encodes function call data.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `method` | `string` | Method name to encode |
| `args` | `any[]` | Method parameters |

**Returns:** Promise resolving to encoded call data

## Bridge

Class for direct bridge contract interactions.

### Methods

#### `bridgeAsset(destinationNetwork: number, destinationAddress: string, amount: TYPE_AMOUNT, token: string, forceUpdateGlobalExitRoot: boolean, permitData?: string, option?: ITransactionOption): Promise<ITransactionWriteResult>`

Low-level bridge asset function.

#### `claimAsset(...): Promise<ITransactionWriteResult>`

Low-level claim asset function with full proof parameters.

#### `bridgeMessage(destinationNetwork: number, destinationAddress: string, forceUpdateGlobalExitRoot: boolean, permitData?: string, option?: ITransactionOption): Promise<ITransactionWriteResult>`

Bridges a message to another network.

#### `claimMessage(...): Promise<ITransactionWriteResult>`

Claims a bridged message on destination network.

## BridgeUtil

Utility class for bridge operations.

### Methods

#### `buildPayloadForClaim(transactionHash: string, networkId: number, bridgeIndex?: number): Promise<IClaimPayload>`

Builds complete claim payload with proofs.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `transactionHash` | `string` | Bridge transaction hash |
| `networkId` | `number` | Source network ID |
| `bridgeIndex` | `number?` | Bridge index (default: 0) |

**Returns:** Promise resolving to claim payload with all required proofs

#### `computeGlobalIndex(indexLocal: number, sourceNetworkId: number): bigint`

Computes global index for a bridge transaction.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `indexLocal` | `number` | Local index (deposit count) |
| `sourceNetworkId` | `number` | Source network ID |

**Returns:** Global index as bigint

#### `getBridgeLogData(transactionHash: string, networkId: number, bridgeIndex?: number): Promise<IBridgeEventInfo>`

Extracts bridge event data from transaction.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `transactionHash` | `string` | Transaction hash |
| `networkId` | `number` | Network ID |
| `bridgeIndex` | `number?` | Bridge index (default: 0) |

**Returns:** Promise resolving to bridge event information

## Type Definitions

### IBaseClientConfig

```typescript
interface IBaseClientConfig {
  network: string;              // 'testnet' or 'mainnet'
  providers: IProviders;        // Provider configurations
  log?: boolean;                // Enable logging
  requestConcurrency?: number;  // Max concurrent requests
}
```

### IProviders

```typescript
interface IProviders {
  [networkId: number]: IProviderConfig;
}

interface IProviderConfig {
  provider: any;                    // Web3 provider instance
  configuration?: {
    bridgeAddress?: string;         // Bridge contract address
    wrapperAddress?: string;        // Wrapper contract address
    isEIP1559Supported?: boolean;   // EIP-1559 support
  };
  defaultConfig?: {
    from?: string;                  // Default sender address
  };
}
```

### ITransactionOption

```typescript
interface ITransactionOption {
  from?: string;                    // Sender address
  gasLimit?: number | string;       // Gas limit
  gasPrice?: number | string;       // Gas price
  maxFeePerGas?: number | string;   // EIP-1559 max fee
  maxPriorityFeePerGas?: number | string; // EIP-1559 priority fee
  value?: number | string;          // ETH value to send
  returnTransaction?: boolean;      // Return tx object instead of sending
  bridgeIndex?: number;             // Bridge index for multi-bridge txs
}
```

### ITransactionWriteResult

```typescript
interface ITransactionWriteResult {
  getTransactionHash(): Promise<string>;     // Get transaction hash
  getReceipt(): Promise<ITransactionReceipt>; // Get transaction receipt
}
```

### IBridgeEventInfo

```typescript
interface IBridgeEventInfo {
  originNetwork: number;        // Source network ID
  originTokenAddress: string;   // Source token address
  destinationNetwork: number;   // Destination network ID
  destinationAddress: string;   // Recipient address
  amount: TYPE_AMOUNT;          // Bridge amount
  metadata: string;             // Additional metadata
  depositCount: number;         // Deposit count (local index)
}
```

### IClaimPayload

```typescript
interface IClaimPayload {
  smtProof: string[];           // Merkle proof for local exit root
  smtProofRollup?: string[];    // Merkle proof for rollup exit root
  globalIndex: string;          // Global transaction index
  mainnetExitRoot: string;      // Mainnet exit root
  rollupExitRoot: string;       // Rollup exit root
  originNetwork: number;        // Source network ID
  originTokenAddress: string;   // Source token address
  destinationNetwork: number;   // Destination network ID
  destinationAddress: string;   // Recipient address
  amount: TYPE_AMOUNT;          // Amount to claim
  metadata: string;             // Transaction metadata
}
```

## Utility Functions

### Global Functions

#### `use(plugin: IPlugin): void`

Enables a provider plugin (Web3.js or Ethers.js).

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `plugin` | `IPlugin` | Provider plugin instance |

**Example:**
```javascript
import { Web3ClientPlugin } from '@maticnetwork/maticjs-web3';
use(Web3ClientPlugin);
```

#### `setProofApi(url: string): void`

Sets the proof API URL for merkle proof generation.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `url` | `string` | Proof API base URL |

**Example:**
```javascript
setProofApi('https://api-gateway.polygon.technology/api/v3/proof/testnet/');
```

## Constants

### Addresses

```typescript
const ADDRESS_ZERO = '0x0000000000000000000000000000000000000000';
const MAX_AMOUNT = '0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff';
```

### Permit Types

```typescript
enum Permit {
  DAI = "DAI",
  EIP_2612 = "EIP_2612", 
  UNISWAP = "UNISWAP"
}
```

### Error Types

```typescript
enum ERROR_TYPE {
  AllowedOnEthereum = "allowed_on_ethereum",
  ProofAPINotSet = "proof_api_not_set",
  EIP1559NotSupported = "eip-1559_not_supported",
  BridgeAdapterNotFound = "bridge_adapter_address_not_passed"
}
```

## Network Configurations

### Testnet Networks

| Network ID | Name | Chain ID | Bridge Address |
|------------|------|----------|----------------|
| 0 | Sepolia | 11155111 | `0x528e26b25a34a4A5d0dbDa1d57D318153d2ED582` |
| 1 | Cardona | 2442 | `0x528e26b25a34a4A5d0dbDa1d57D318153d2ED582` |

### Mainnet Networks

| Network ID | Name | Chain ID | Bridge Address |
|------------|------|----------|----------------|
| 0 | Ethereum | 1 | `0x2a3DD3EB832aF982ec71669E178424b10Dca2EDe` |
| 1 | Polygon zkEVM | 1101 | `0x2a3DD3EB832aF982ec71669E178424b10Dca2EDe` |

## Error Handling

### Common Errors

| Error Type | Description | Solution |
|------------|-------------|----------|
| `ProofAPINotSet` | Proof API not configured | Call `setProofApi()` with valid URL |
| `EIP1559NotSupported` | EIP-1559 used on unsupported network | Use legacy gas pricing |
| `BridgeAdapterNotFound` | Custom bridge adapter not provided | Provide adapter address for custom tokens |
| `AllowedOnEthereum` | Operation only allowed on Ethereum | Switch to Ethereum network |
| `AllowedOnNonNativeTokens` | Operation not allowed for ETH | Use ERC20 token instead |

### Error Recovery Examples

```javascript
// Handle insufficient allowance
try {
  await token.bridgeAsset(amount, userAddress, destinationNetwork);
} catch (error) {
  if (error.message.includes('allowance')) {
    console.log('Insufficient allowance, approving...');
    await token.approve(amount);
    // Retry bridge operation
    await token.bridgeAsset(amount, userAddress, destinationNetwork);
  }
}

// Handle network issues
async function robustNetworkCall(operation) {
  const maxRetries = 3;
  let lastError;
  
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;
      if (error.message.includes('network')) {
        console.log(`Network error, retrying... (${i + 1}/${maxRetries})`);
        await new Promise(resolve => setTimeout(resolve, 2000 * (i + 1)));
      } else {
        throw error; // Non-network error, don't retry
      }
    }
  }
  
  throw lastError;
}
```

## Migration Guide

### From v2.2.x to v2.3.x

```javascript
// Old initialization (v2.2.x)
const client = new LxLyClient();
await client.init(config);

// New initialization (v2.3.x) - same API
const client = new LxLyClient();
await client.init(config);

// Bridge operations remain the same
const result = await token.bridgeAsset(amount, userAddress, destinationNetwork);
```

### Breaking Changes

| Version | Change | Migration |
|---------|--------|-----------|
| v2.3.0 | Updated bridge contract addresses | Update configuration with new addresses |
| v2.2.0 | Changed permit data format | Update permit data generation |
| v2.1.0 | Added bridge extension support | Add bridge extension addresses to config |

## Examples Repository

For complete working examples, see the [Agglayer Unified Bridge Examples](https://github.com/BrianSeong99/AggLayer_UnifiedBridge) repository:

- **Basic Asset Bridging**: L1 ↔ L2 token transfers
- **Message Bridging**: Cross-chain contract calls  
- **Custom Contracts**: Advanced contract interactions
- **Error Handling**: Robust error recovery patterns

## Performance Tips

### Optimization Strategies

```javascript
// 1. Reuse client instances
const globalClient = await initializeClient();

// 2. Cache token instances
const tokenCache = new Map();
function getToken(address, networkId) {
  const key = `${address}-${networkId}`;
  if (!tokenCache.has(key)) {
    tokenCache.set(key, globalClient.erc20(address, networkId));
  }
  return tokenCache.get(key);
}

// 3. Batch read operations
const [balance, allowance, totalSupply] = await Promise.all([
  token.getBalance(userAddress),
  token.getAllowance(userAddress),
  contract.read('totalSupply')
]);

// 4. Use appropriate gas limits
const gasLimits = {
  approve: 80000,
  bridgeAsset: 300000,
  claimAsset: 400000,
  bridgeMessage: 350000,
  claimMessage: 450000
};
```

This API reference provides complete documentation for building production applications with Lxly.js!
