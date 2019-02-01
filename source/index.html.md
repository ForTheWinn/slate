---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

NLP (NEP-5 Lottery Platform) smart contract has deployed onto NEO blockchain by FTW. This document shows examples of how to trigger the smart contract methods using NEO RPC.

#### Smart contract source code:

https://github.com/ForTheWinn

#### Smart contract hash:

356ad59da95f1f6947b8f2c8cc69595fbbd511f3

# Invoke

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const privateKey = "";
const invocationScript = "";

const provider = new api.neoscan.instance("MainNet");
const account = new wallet.Account(privateKey);
const balance = await provider.getBalance(account.address);
const config = {
  api: provider,
  account,
  privateKey,
  balance,
  script: invocationScript
};

const result = await Neon.doInvoke(config);
```

In order to trigger the contract, you need to create a raw transaction and send it to the chain. [Neon-js](http://cityofzion.io/neon-js/en/) is a great tool that makes the process easier.


# Lottery methods

## buyLotteryTicket()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const playerAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";
const referralAddress = "AT5Sf5s4cZDqeGZc1aev7ZuBZkSumztTge";

const invocationScript = Neon.create.script({
 scriptHash: "356ad59da95f1f6947b8f2c8cc69595fbbd511f3",
 operation: "buyLotteryTicket",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(playerAddress)),
     1,
     1,
     20,
     21,
     22,
     23,
     34,
     u.reverseHex(wallet.getScriptHashFromAddress(referralAddress))
 ]
})
```

This method used to participate the game.

### Parameters

Parameter | Type | Description
--------- | ------- | -----------
playerHash | Byte[] | The contract will transfer the fund from player hash to the contract.
walletType | Int 1 or 2 | If set to 1, the contract will transfer the fund from player hash. The other will be from the users balance of the contract
ticketType | Int 1, 2 or 3 | 1: 1 FTX, 2: 0.1 CNEO 3: 1 CGAS.
number1 | Int | Numbers between 1 and 39. The contract will reject if you submit same numbers.
number2 | Int | "
number3 | Int | "
number4 | Int | "
number5 | Int | "
referralHash | Byte[] | Reward referral hash will be rewarded 5% of ticket price.


## drawLottery()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const casterAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";

const invocationScript = Neon.create.script({
 scriptHash: "356ad59da95f1f6947b8f2c8cc69595fbbd511f3",
 operation: "drawLottery",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(casterAddress)),
 ]
})
```

This method draws the current game.

### Parameters

Parameter | Type | Description
--------- | ----------- | -----------
casterHash | Byte[] | The contract will reward 5% of total ticket sales of the game whoever triggers first

## verityLotteryTicket()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const casterAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";
const ticketNo = 1;

const invocationScript = Neon.create.script({
 scriptHash: "356ad59da95f1f6947b8f2c8cc69595fbbd511f3",
 operation: "verifyLotteryTicket",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(casterAddress)),
     ticketNo
 ]
})
```


This method verifies lottery tickets.


### Parameters

Parameter | Type | Description
--------- | ----------- | -----------
casterHash | Byte[] | The contract will reward 5% of the ticket price of being verified.
ticketNo | Int | It set a ticket number, it verifies the target ticket. If set none, it finds if there are available tickets to verify by using the latest verification height and verifies.

<aside class="notice">
Vm script should be run before submitting a real transaction in order to check if the target is avaiable or if there are avaiable tickets. 
</aside>

### Status getters

Method | Parameters | Description
--------- | ----------- | -----------
casterHash | Byte[] | The contract will reward 5% of the ticket price of being verified.
ticketNo | Int | It set a ticket number, it verifies the target ticket. If set none, it finds if there are available tickets to verify by using the latest verification height and verifies.


## claimLotteryWinningTicket()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const casterAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";
const ticketNo = 1;

const invocationScript = Neon.create.script({
 scriptHash: "356ad59da95f1f6947b8f2c8cc69595fbbd511f3",
 operation: "claimLotteryWinningTicket",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(casterAddress)),
     ticketNo
 ]
})
```

This method claim the prize of winning tickets.


### Parameters

Parameter | Type | Description
--------- | ----------- | -----------
casterHash | Byte[] |
ticketNo | Int | It set a ticket number, it claims the target ticket. If set none, it finds available tickets to claim by using the latest claim height.
