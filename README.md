## What is interop-std?

A set of basic libraries built on top of OpenZeppelin and Solady contracts which help developers write multichain smart contracts based on Optimism interops.

## Why we developed it?

Managing deployments on many chains is tough. These libraries provide a simple and easy way for developers to change permissions on one chain which are then automatically updated on all chains the contract is deployed on using interops.

## What's included in the library?

The library currently contains 3 contracts:

### 1. SuperOwnable

[Source](https://github.com/RiftLend/interop-std/blob/main/src/auth/SuperOwnable.sol)

Built on top of Ownable from Solady, it enables developers to set one admin chainId which can update ownership across all chains. For example, if governance is operated on `OP Mainnet` and contracts are deployed on `Base`, `Mode`, and `OP Mainnet`:

- Setting the admin chain as `OP Mainnet` allows governance to call `transferOwnership`
- This updates ownership on the main chain (`OP Mainnet`)
- Emits an `InitiateCrosschainOwnershipTransfer` event
- Event is indexed in the `CrossL2Inbox` and picked up by a relayer
- Relayer calls `updateCrosschainOwner()` on contracts on all other chains (`Base`, `Mode`, etc.)

### 2. SuperPausable

[Source](https://github.com/RiftLend/interop-std/blob/main/src/utils/SuperPausable.sol)

Built on top of Pausable from OpenZeppelin, it helps developers manage pausing of multichain contracts using interops. Benefits:

- Eliminates need to pause contracts individually on each chain
- Reduces delays during emergencies
- When paused on one chain, emits `CrosschainPauseStateChanged` event
- Event is picked up by relayer from CrossL2Inbox
- Relayer calls `updateCrosschainPauseState()` to pause contracts on all deployed chains

### 3. SuperProxyAdmin 🔨

[Source](https://github.com/RiftLend/interop-std/blob/main/src/utils/SuperProxyAdmin.sol)

_Development in Progress_

When the ProxyAdmin owner or proxy implementation is updated, it automatically updates all proxied implementations and owners across all chains where the proxied contract is deployed.
