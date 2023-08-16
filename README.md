# CLP Coin (CLPC)

**CLP Coin** is the first stablecoin pegged to CLP (Chilean pesos) in the XRPL network.

[CLP Coin uses hooks for development.](https://hooks.xrpl.org/)

_For this version (0.1), CLP Coin uses a similar structure the the peggy.c example for hooks development_

## About CLP Coin

Ticker: CLPC

CLP Coin is an over-collateralized XRP-backed stablecoin pegged to Chilean pesos.

Sending XRP to it creates a vault and sends back CLPC, which can be redeemed against the vault to get back the original XRP.

The exchange rate between XRP and CLPC is the limit set on a trustline established between two special oracle accounts.

[The collateralization is between 120% to 150%](./clpcoin.c).

## How to test
- Open [Hooks Builder](https://hooks-builder.xrpl.org/)
- Make sure Hooks Builder has at least 5 accounts: Alice, Bob, Carol, Carlos, and Charlie
- run [decode.js](./decode.js], twice, to convert Carlos and Charlie's accounts to binary form; save the values somewhere
- if Carlos's account number is numerically higher than Charlie, switch the accounts (either re-import them or just switch their names in the following text)
- set up a trust limit for the CLPC user, by running trust-user.js; the script requires 2 parameters:
	1. the user account (Alice) sends the TrustSet transaction so that the script requires its private key
	2. the hook account (Carol) is set up as the trusted issuer
- set up trust limit on the oracle, by running trust-oracle.js; the script requires 2 parameters:
	1. the low account (Carlos) sends the TrustSet transaction so that the script requires its private key
	2. the high account (Charlie) is set up as the trusted issuer
- compile peggy.c and deploy it to Carol account, with 2 parameters:
  1. "oracle_lo" set Carlos binary account
	2. and "oracle_hi" set to the Charlie binary account
- set up a payment transaction from Alice to Carol
- open debug stream filtered on Carol
- run the transaction, and see it succeed:
