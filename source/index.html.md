---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:

includes:

search: true
---

# Introduction

This document shows examples of how to interact with our smart contracts. .

**Contact**<br>
Feel free to contact us when you need a support.<br>
Email: info@ftwcoin.io<br>
Telegram: https://t.me/ftwcoin

## Dependencies

[Neon-js](http://cityofzion.io/neon-js/en/) is a great tool to interact with NEO blockchain.

# NEO2

## FTW-LOTTO

**Smart contract hash**<br>
ada839286d23cdfb42eb556461b9382d02b6e12f

**Github**<br>
[Link](https://github.com/ForTheWinn/smart-contracts/tree/master/contracts/ftw-lotto)

### Write

#### buyLotteryTicket()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const playerAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";
const referralAddress = "AT5Sf5s4cZDqeGZc1aev7ZuBZkSumztTge";

const invocationScript = Neon.create.script({
 scriptHash: "ada839286d23cdfb42eb556461b9382d02b6e12f",
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

This method used to participate the lottery. Referral hash gets 5% of ticket price.

Parameter | Type | Description
--------- | ------- | -----------
playerHash | Byte[] | The contract will transfer the ticket price from player hash to NLP contract.
walletType | Int 1 or 2 | If set to 1, the contract will transfer the fund from player hash. The other will be from the users balance of the contract
ticketType | Int 1, 2 or 3 | 1 is FTX, 2 is CNEO and 3 is CGAS.
number1 | Int | Numbers between 1 and 39.
number2 | Int | "
number3 | Int | "
number4 | Int | "
number5 | Int | "
referralHash? | Byte[] | Referral hash will be rewarded 5% of ticket price.

#### drawLottery()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const casterAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";

const invocationScript = Neon.create.script({
 scriptHash: "ada839286d23cdfb42eb556461b9382d02b6e12f",
 operation: "drawLottery",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(casterAddress)),
 ]
})
```

This method draws the current game. Caster must have participated the current FTX lottery to be qualified for this method. The caster will be rewarded 5% of FTX, CNEO and CGAS sales of the game.

Parameter | Type | Description
--------- | ------- | -----------
casterHash | Byte[] |            

#### verityLotteryTicket()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const casterAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";
const ticketNo = 1;

const invocationScript = Neon.create.script({
 scriptHash: "ada839286d23cdfb42eb556461b9382d02b6e12f",
 operation: "verifyLotteryTicket",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(casterAddress)),
     ticketNo
 ]
})
```

This method verifies lottery tickets. Verifier will be rewarded its ticket price that has been verified.

Parameter | Type | Description
--------- | ----------- | -----------
verifierHash | Byte[] | 
ticketNo? | Int | It set a ticket number, it verifies the target ticket. If set none, it finds if there are available tickets to verify by using the latest verification height and verifies.

#### claimLotteryWinningTicket()

```javascript
import Neon, {u, wallet} from "@cityofzion/neon-js";

const casterAddress = "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS";
const ticketNo = 1;

const invocationScript = Neon.create.script({
 scriptHash: "ada839286d23cdfb42eb556461b9382d02b6e12f",
 operation: "claimLotteryWinningTicket",
 args: [
     u.reverseHex(wallet.getScriptHashFromAddress(casterAddress)),
     ticketNo
 ]
})
```

This method claims the prize of winning tickets.

Parameter | Type | Description
--------- | ----------- | -----------
casterHash | Byte[] |
ticketNo? | Int | It set a ticket number, it claims the target ticket. If set none, it finds available tickets to claim by using the latest claim height.

### Read

Methods that fetch blockchain data.

#### getLotteryTicket(ticketNo)

```javascript
const totalItems = ""; // current entry height
const pageSize = 30; // how many per page
const currentPage = 1;
const totalPages = totalItems / pageSize;
const sort = 0;

let start;
let stop;

if (sort === 1) {
  start = (currentPage - 1) * pageSize + 1;
  stop = totalPages === currentPage ? totalItems + 1 : start + pageSize;
} else {
  const skip = (currentPage - 1) * pageSize;
  start = totalPages > 1 ? totalItems - skip : totalItems;
  stop = start - pageSize > 0 ? start - pageSize : 0;
}

const sb = Neon.create.scriptBuilder();

_.range(start, stop, sort).forEach(i => {
  sb.emitAppCall(
    "ada839286d23cdfb42eb556461b9382d02b6e12f",
    "getLotteryTicket",
    i
  );
});

const result = await rpc.Query.invokeScript(sb.str).execute(
  rpcEndpoint
);

return result.result.stack.map(item => item.value);
```

It returns serialized data. You can deserialize it using neon-js.

Index | value 
----- | -----
0 | gameNo
1 | ticketNo
2 | ticketType
3 | playerHash
4 | serializedLotteryNumbers
5 | serializedVerification
6 | serializedResult
7 | createdAt
8 | referralHash

#### getLotteryResult(gameNo)

```javascript
const totalItems = ""; // current entry height
const pageSize = 30; // how many per page
const currentPage = 1;
const totalPages = totalItems / pageSize;
const sort = 0;

let start;
let stop;

if (sort === 1) {
  start = (currentPage - 1) * pageSize + 1;
  stop = totalPages === currentPage ? totalItems + 1 : start + pageSize;
} else {
  const skip = (currentPage - 1) * pageSize;
  start = totalPages > 1 ? totalItems - skip : totalItems;
  stop = start - pageSize > 0 ? start - pageSize : 0;
}

const sb = Neon.create.scriptBuilder();

_.range(start, stop, sort).forEach(i => {
  sb.emitAppCall(
    "ada839286d23cdfb42eb556461b9382d02b6e12f",
    "getLotteryResult",
    i
  );
});

const result = await rpc.Query.invokeScript(sb.str).execute(
  rpcEndpoint
);

return result.result.stack.map(item => item.value);
```

Parameter | Type | Description
--------- | ----------- | -----------
gameNo | Int | 

It returns serialized data. You need to deserialize it.

Index | value 
----- | -----
0 | gameNo
1 | winningNumbers
2 | lastTicketNo
3 | caster
9[0] | totalTickets
9[1] | totalVerifiedTickets
10 | referralHash


#### getLotteryHeight()

#### getLotteryEntryHeight()

#### getLotteryVerificationHeight()

#### getLotteryWinnerHeight()

#### getLotteryClaimHeight()

#### getLotteryWinner(winnerHeight)

#### getLotteryPool(currencyType, gameNo)

#### getLotteryUserBalance(ownerHash, currencyType)

#### getTimeLeft()

#### getLotteryCurrentJackpots()

#### getLotteryParticipation()

#### getLotteryVerification()

#### getLotteryClaim()


