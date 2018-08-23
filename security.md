

# Security explanations

## 1) Reentrancy: buyNumber function of Generic bet contract
the add to betPrize is made by an internal value not by the value send by the player

the transfer of the value exceeding the standard value for buying a new number is given back to the sender. The function buyNumber have to be called with a value. so in case of reentrancy the money considered to be given back is the value in the parameters of the new buyNumber function called


## 2) Reentrancy: endBet function of Generic bet contract

In case of reentrancy, the value of betprize is already set to zero before the possible call to another function

*Checks-Effects-Interactions Pattern* is used: The transfer function is called as late as possible

## 3) Gas Limit  and Loops

Huge effort was put in making as few as possible loops. In fact the only loop is in the constructor of the contract genericBet. 
(That also allow not to penalize player lowering the gas cost of the bets)

## 3) tx.origin
is never used

## 4) Restrict the Amount of Ether

The contract add Ether not relying on the value from the call of the fucntion but from the value (public) necesary to buy a number, stored as internal variable

## 5) Stucked contract

A new bet cannot be made until the actula bet is in the state "ended=false". But the ended=true (and pause=true) is the first thing executed when a bet is ended.

During buyNumber function somebody can stuck the contract make the transfer of the exceeding vlaur of the bet fails.
This is noticed: further version could not implement this function for security reason.

## 6) Integer Overflow and Underflow

All the computation with value passed from outside are made using SafeMath functions
