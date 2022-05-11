# Truster

## Impact

**Critical** The lender pool allows an attacker to call any contract as the pool. 


## Background

The pool `flashLoan()`, triggers a flash loan and an arbitrary call as the pool, it is supposed to allow the user to performs actions with the flash loan but the implementation is flawed.

## Vulnerability

The `flashLoan()` body contains: `target.functionCall(data);` using user provided call data and target. 

An attacker can trigger a flashLoan for 0 dvt, simply set an allowance from the pool to the attacker address in the calldata and then syphon the pool.

## POC

The proof of concept can be found in TrusterLenderPool.t.sol

## Reccomendations


1. The pool should not accept loans of 0 dvt
2. The lender pool should not allow users to provide an arbitrary target to call with any calldata. One way to limit risk is to call the `msg.sender` instead of a user provided target, but this will limit functionnality.
   
