# Unstoppable

## Impact

**Medium** The contract is subject to a DoS attack, preventing users from contracting loans

## Background

The contract maintains a variable containing its balance. It is updated when users deposit via the `depositTokens(uint256 amount)`.

## Vulnerability

The `flashLoan(uint256 borrowAmount)` functions checks an actual balance `balanceBefore` against its `poolBalance`.

If the actual balance of the contract changes the `flashloan()` function always reverts. We can simply send 1 wei to the contract to block it.

```javascript
        uint256 balanceBefore = damnValuableToken.balanceOf(address(this));
        if (balanceBefore < borrowAmount) revert NotEnoughTokensInPool();

        // Ensured by the protocol via the `depositTokens` function
        if (poolBalance != balanceBefore) revert AssertionViolated();

```

## POC

The proof of concept can be found in Unstoppable.t.sol

## Reccomendations

1. The current token contract is non-upgradable. It will need to be replaced with a new contract that has this bug fixed, and holders will need to be migrated to it.
2. Do not track the balance in a variable, instead use the actual token balance to prevent this vulnerability
