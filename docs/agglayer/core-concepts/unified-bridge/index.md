---
title: Unified Bridge
---

<!-- Page Header Component -->
<h1 style="text-align: left; font-size: 38px; font-weight: 700; font-family: 'Inter Tight', sans-serif;">
  Unified Bridge
</h1>

<div style="text-align: left; margin: 0.5rem 0;">
  <p style="font-size: 18px; color: #666; max-width: 600px; margin: 0;">
    The core interoperability layer enabling seamless cross-chain communication and asset transfers across Agglayer connected chains
  </p>
</div>

## Overview

The Unified Bridge is the core interoperability layer that enables seamless cross-chain communication and asset transfers across Agglayer connected chains. It solves the fragmentation problem by providing a unified interface for L1 to L2, L2 to L1, and L2 to L2 transfers.

**Key Benefits:**

- **Unified Experience**: Single interface across all connected chains
- **Trustless Security**: Cryptographic verification of all transactions
- **Developer Friendly**: Simple APIs and comprehensive tooling

## What You Can Do

- **Transfer Assets**: Bridge tokens and native assets between any connected chains
- **Execute Cross-Chain Calls**: Trigger smart contract functions on destination chains
- **Build Cross-Chain Apps**: Create applications that work seamlessly across multiple chains

## How It Works

The Unified Bridge uses a sophisticated Merkle tree structure to securely track and verify all cross-chain transactions:

![Unified Bridge Data Structure](../../../img/agglayer/UnifiedBridgeTree.png)

**Security Model:**

- All transactions are settled on Ethereum before claiming
- Cryptographic proofs verify every cross-chain operation  
- Immutable transaction data prevents incorrect claims

## Core Concepts

Understand the fundamental architecture and components:

<div style="display: flex; flex-direction: column; gap: 1rem; max-width: 800px; margin: 1rem 0;">

  <!-- Architecture Card -->
  <div style="background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 6px; padding: 1rem 1rem; margin: 0.25rem 0;">
    <h3 style="color: #0071F7; margin: 0 0 0.5rem 0; font-size: 18px; font-weight: 600;">
      Architecture
    </h3>
    <p style="color: #666; margin-bottom: 0.75rem; line-height: 1.4; font-size: 14px;">
      Understand the complete system architecture and component interactions.
    </p>
    <a href="/agglayer/core-concepts/unified-bridge/architecture/" style="color: #0071F7; text-decoration: none; font-weight: 500; font-size: 14px;">
      Learn more →
    </a>
  </div>

  <!-- Data Structures Card -->
  <div style="background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 6px; padding: 1rem 1rem; margin: 0.25rem 0;">
    <h3 style="color: #0071F7; margin: 0 0 0.5rem 0; font-size: 18px; font-weight: 600;">
      Data Structures
    </h3>
    <p style="color: #666; margin-bottom: 0.75rem; line-height: 1.4; font-size: 14px;">
      Learn about Local Exit Root, Rollup Exit Root, Mainnet Exit Root, and Global Exit Root.
    </p>
    <a href="/agglayer/core-concepts/unified-bridge/data-structures/" style="color: #0071F7; text-decoration: none; font-weight: 500; font-size: 14px;">
      Learn more →
    </a>
  </div>

  <!-- Bridge Components Card -->
  <div style="background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 6px; padding: 1rem 1rem; margin: 0.25rem 0;">
    <h3 style="color: #0071F7; margin: 0 0 0.5rem 0; font-size: 18px; font-weight: 600;">
      Bridge Components
    </h3>
    <p style="color: #666; margin-bottom: 0.75rem; line-height: 1.4; font-size: 14px;">
      Understand the smart contracts, services, and tools that power the Unified Bridge.
    </p>
    <a href="/agglayer/core-concepts/unified-bridge/bridge-components/" style="color: #0071F7; text-decoration: none; font-weight: 500; font-size: 14px;">
      Learn more →
    </a>
  </div>

</div>

## Bridge Operations

Master the three types of cross-chain operations:

<div style="display: flex; flex-direction: column; gap: 1rem; max-width: 800px; margin: 1rem 0;">

  <!-- Asset Bridging Card -->
  <div style="background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 6px; padding: 1rem 1rem; margin: 0.25rem 0;">
    <h3 style="color: #0071F7; margin: 0 0 0.5rem 0; font-size: 18px; font-weight: 600;">
      Asset Bridging
    </h3>
    <p style="color: #666; margin-bottom: 0.75rem; line-height: 1.4; font-size: 14px;">
      Learn how to bridge tokens and native assets between different chains.
    </p>
    <a href="/agglayer/core-concepts/unified-bridge/asset-bridging/" style="color: #0071F7; text-decoration: none; font-weight: 500; font-size: 14px;">
      Learn more →
    </a>
  </div>

  <!-- Message Bridging Card -->
  <div style="background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 6px; padding: 1rem 1rem; margin: 0.25rem 0;">
    <h3 style="color: #0071F7; margin: 0 0 0.5rem 0; font-size: 18px; font-weight: 600;">
      Message Bridging
    </h3>
    <p style="color: #666; margin-bottom: 0.75rem; line-height: 1.4; font-size: 14px;">
      Enable cross-chain smart contract communication and execution.
    </p>
    <a href="/agglayer/core-concepts/unified-bridge/message-bridging/" style="color: #0071F7; text-decoration: none; font-weight: 500; font-size: 14px;">
      Learn more →
    </a>
  </div>


</div>