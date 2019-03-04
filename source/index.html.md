---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:

includes:

search: true
---

# Introduction

This document shows examples of how to create your lottery using NLP smart contract on NEO blockchain.

#### Source code:

https://github.com/ForTheWinn/NLP-SmartContract

#### Smart contract hash:

ada839286d23cdfb42eb556461b9382d02b6e12f

#### Contact

Feel free to contact our developers when you have questions. 

Email: info@ftwcoin.io

Telegram: https://t.me/ftwcoin

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

In order to trigger the contract, you need to create a raw transaction and send it to the chain. 

[Neon-js](http://cityofzion.io/neon-js/en/) is a great tool that makes the process easier.

## buyLotteryTicket()

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
referralHash | Byte[] | Referral hash will be rewarded 5% of ticket price.


## drawLottery()

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

### Parameters

Parameter | Type | Description
--------- | ----------- | -----------
casterHash | Byte[] |

## verityLotteryTicket()

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
ticketNo | Int | It set a ticket number, it verifies the target ticket. If set none, it finds if there are available tickets to verify by using the latest verification height and verifies.


### Related method

You should run vm machine with the script before you send the actual transaction to check if the chain returns True state.


## claimLotteryWinningTicket()

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

### Parameters

Parameter | Type | Description
--------- | ----------- | -----------
casterHash | Byte[] |
ticketNo | Int | It set a ticket number, it claims the target ticket. If set none, it finds available tickets to claim by using the latest claim height.



# Lottery entries

> Entry structured like this:

```json
[
 {
     "_id": "LHBxJuPTS6JAsKHuk",
     "gameNo": 41,
     "ticketNo": 2312,
     "ticketCurrency": "FTX",
     "player": "AUNMeF1EsifxHxKSH6h21kwqekbe8bZw3D",
     "numbers": [
       19,
       14,
       32,
       28,
       7
     ],
     "isVerified": false,
     "matched": 0,
     "isClaimed": false,
     "prize": 0,
     "createdAt": 1551344522,
     "referral": "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS"
   }
]
```


## Get all lottery entries

> Returns like this:

```json
{
  "page": 1,
  "page_len": 30,
  "total": 195,
  "total_page": 6,
  "results": [...]
  }
```

This endpoint retrieves all entries.

### HTTP Request

`GET https://api.ftwcoin.io/api/lottery/entries/{page}`


## Get a specific entry

> Returns like this:

```json
{
  "page": 1,
  "page_len": 30,
  "total": 195,
  "total_page": 6,
  "results": [...]
  }
```

This endpoint retrieves a specific entry.

### HTTP Request

`GET https://api.ftwcoin.io/api/lottery/entry/{ticketNo}`


## Get entries by address

> Returns like this:

```json
 {
     "_id": "LHBxJuPTS6JAsKHuk",
     "gameNo": 41,
     "ticketNo": 2312,
     "ticketCurrency": "FTX",
     "player": "AUNMeF1EsifxHxKSH6h21kwqekbe8bZw3D",
     "numbers": [
       19,
       14,
       32,
       28,
       7
     ],
     "isVerified": false,
     "matched": 0,
     "isClaimed": false,
     "prize": 0,
     "createdAt": 1551344522,
     "referral": "AbvAMiWRib6GzGZKU1o8QxUntnzhCwjXdS"
   }
```

This endpoint retrieves entries by address.

### HTTP Request

`GET https://api.ftwcoin.io/api/lottery/entries/{address}/{page}`

## Get winning entries

> Returns like this:

```json
{
  "page": 1,
  "page_len": 30,
  "total": 195,
  "total_page": 6,
  "results": [...]
  }
```

This endpoint retrieves winning entries.

### HTTP Request

`GET https://api.ftwcoin.io/api/lottery/entries/winnings/{page}`



## Get entries from the chain

You can get and paginate all entries using height keys directly from the chain.

### Entries

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

It returns serialized array. You need to deserialize it.


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

### Related methods

getLotteryEntryHeight()

getLotteryTicket(ticketNo)


## Get draw results from the chain

You can get and paginate all draw results using height keys directly from the chain.


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

It returns serialized array. You need to deserialize it.


Index | value 
----- | -----
0 | gameNo
1 | winningNumbers
2 | lastTicketNo
3 | caster
9[0] | totalTickets
9[1] | totalVerifiedTickets
10 | referralHash


