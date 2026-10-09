# challenge-scroll

An ERC-20 token (`michmich56`, symbol `MM`) built on **OpenZeppelin Contracts v5** and written for a Scroll challenge.

> **Educational code.** This contract is a learning exercise. It has not been audited and must not be used in production as is. See [Security notes](#security-notes).

## Overview

The contract in [`challenge-scroll`](./challenge-scroll) is a flattened Solidity source: the OpenZeppelin ERC-20 implementation followed by the `michmich56` token.

| Property | Value |
| --- | --- |
| Name / symbol | `michmich56` / `MM` |
| Decimals | 18 |
| Compiler | Solidity `^0.8.24` |
| Base | OpenZeppelin Contracts v5 `ERC20` |
| License | MIT |

## Interface

| Function | Description |
| --- | --- |
| `constructor(uint256 initialSupply)` | Mints `initialSupply` to the deployer |
| `mint(address to, uint256 amount)` | Mints new tokens to `to` |
| `burn(address from, uint256 amount)` | Burns tokens from `from` |
| `transfer`, `approve`, `transferFrom` | Standard ERC-20 functions, overridden |
| `getBalanceOf(address account)` | Convenience alias of `balanceOf` |

## Deploying on Scroll

1. Open the source in [Remix](https://remix.ethereum.org) and compile with Solidity 0.8.24 or later.
2. Connect a wallet to Scroll Sepolia (testnet).
3. Deploy `michmich56` with the initial supply expressed in base units (for 1,000,000 tokens, pass `1000000 * 10**18`).

## Security notes

A short self-review of the contract, kept here on purpose:

- **Unrestricted `mint` and `burn`.** Both are `public` with no access control, so any address can mint to itself or burn another holder's balance. A real token needs `Ownable` or `AccessControl`, and `burn` should only act on the caller's balance or on an allowance (`ERC20Burnable`).
- **`transferFrom` override.** It transfers before checking the allowance and drops the infinite-allowance behaviour of OpenZeppelin's `_spendAllowance`. The revert still protects funds, but the inherited implementation is safer and cheaper; the override should be removed.
- **Redundant overrides.** `transfer`, `approve` and `getBalanceOf` duplicate inherited behaviour and add surface without adding features.

## License

MIT
