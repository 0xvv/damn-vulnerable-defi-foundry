# Naive Receiver

## Impact

**High** The receiver will pay the fee any time `receiveEther(uint256 fee)` is called by the trusted lender pool which can be called by anyone, allowing the contract to be syphoned. 


## Background

The receiver contract has a function to be called by the lender pool when a flash loan is triggered.

## Vulnerability

The receiver only check if it can pay back the flash loan, not if it should. It does check if the pool is calling the `receiveEther(uint256 fee)` function.

But the pool allows to pass a different address when triggering a flash loan : `flashLoan(address borrower, uint256 borrowAmount)`, this functions proceeds to call the borrower address as the pool thus syphoning the flash loan receiver.


## POC

The proof of concept can be found in NaiveReceiver.t.sol

## Reccomendations

1. The lender pool should not allow anyone to trigger a flash loan to any address and then call the user provided address as the ppol
2. This receiver and similar ones should be emptied of funds.
   
