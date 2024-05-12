---
sidebar_position: 7
---

# Orca DEX API

In this section, we'll show you how to access information about Orca DEX data using Bitquery APIs.

This Solana API is part of our Early Access Program (EAP).

You can use this program to try out the data and see how it works with your applications before you fully integrate it. You can learn more about this [here](https://docs.bitquery.io/docs/graphql/dataset/EAP/).

<head>
<meta name="title" content="Orca DEX API - Liquidity Pools & Trades"/>
<meta name="description" content="Use our Solana API & Websockets, designed for developers, to get SPL token trading data on Orca DEX. Access details on pools, as well as adding and removing liquidity."/>
<meta name="robots" content="index, follow"/>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
<meta name="language" content="English"/>

<!-- Open Graph / Facebook -->

<meta property="og:type" content="website" />
<meta
  property="og:title"
  content="Orca DEX API - Liquidity Pools & Trades"
/>
<meta
  property="og:description"
  content="Use our Solana API & Websockets, designed for developers, to get SPL token trading data on Orca DEX. Access details on pools, as well as adding and removing liquidity."
/>

<!-- Twitter -->

<meta property="twitter:card" content="summary_large_image" />
<meta property="twitter:title" content="Orca DEX API - Liquidity Pools & Trades" />
<meta property="twitter:description" content="Use our Solana API & Websockets, designed for developers, to get SPL token trading data on Orca DEX. Access details on pools, as well as adding and removing liquidity." />
</head>

## Latest Pools created on Orca

To retrieve the newest pools created on Orca DEX, we will utilize the Solana instructions API/Websocket.

We will specifically look for the latest instructions from Orca's Whirlpool program, identified by the program ID `whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc`. 
Whenever a new pool is created on Orca, it triggers the `initializePool` instructions. The pool address can be obtained from the program addresses listed in the transaction's instructions.

For instance, Index 1 and 2 represent the tokens involved in the pool, while Index 4 is for the pool's address. Note that the indexing starts from 0.

You can run this query using this [link](https://ide.bitquery.io/Latest-pool-created-on-Orca---Websocket_1).

```graphql
subscription {
  Solana {
    Instructions(
      where: {
        Instruction: {
          Program: {
            Method: { is: "initializePool" }
            Address: { is: "whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc" }
          }
        }
      }
    ) {
      Instruction {
        Program {
          Method
          Arguments {
            Name
            Value {
              __typename
              ... on Solana_ABI_Integer_Value_Arg {
                integer
              }
              ... on Solana_ABI_String_Value_Arg {
                string
              }
              ... on Solana_ABI_Address_Value_Arg {
                address
              }
              ... on Solana_ABI_BigInt_Value_Arg {
                bigInteger
              }
              ... on Solana_ABI_Bytes_Value_Arg {
                hex
              }
              ... on Solana_ABI_Boolean_Value_Arg {
                bool
              }
              ... on Solana_ABI_Float_Value_Arg {
                float
              }
              ... on Solana_ABI_Json_Value_Arg {
                json
              }
            }
          }
        }
        Accounts {
          Address
        }
      }
      Transaction {
        Signature
      }
    }
  }
}
```

## Latest Trades on Orca DEX API Websocket

To access a real-time stream of trades for Solana Orca DEX, [check out this GraphQL subscription (WebSocket)](https://ide.bitquery.io/Orca-DEX-Trades-Websocket).

```graphql
subscription {
  Solana {
    DEXTrades(
      where: {
        Trade: {
          Dex: {
            ProgramAddress: {
              is: "whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc"
            }
          }
        }
      }
    ) {
      Transaction {
        Signature
      }
      Block {
        Time
      }
      Trade {
        Dex {
          ProgramAddress
          ProtocolName
          ProtocolFamily
        }
        Buy {
          Account {
            Address
          }
          Amount
          AmountInUSD
          Currency {
            MintAddress
            Symbol
            Name
          }
          Price
          PriceInUSD
        }
        Sell {
          Account {
            Address
          }
          Amount
          AmountInUSD
          Currency {
            MintAddress
            Symbol
            Name
          }
          Price
          PriceInUSD
        }
      }
    }
  }
}
```

## Latest Trades for a specific currency on Solana Orca DEX

If you want to monitor [trades for a specific currency on Orca DEX](https://ide.bitquery.io/Orca-DEX-Trades-for-a-specific-currency-Websocket), you can use the stream provided. Input the currency's mint address; for example, in the query below, we use the WSOL token's Mint address to fetch buys of the WSOL token.

By setting the limit to 1, you will receive the most recent trade, which reflects the latest price of the token.

Execute this query [by following this link](https://ide.bitquery.io/Orca-DEX-Trades-for-a-specific-currency-Websocket).

```graphql
subscription {
  Solana {
    DEXTrades(
      where: {
        Trade: {
          Dex: {
            ProgramAddress: {
              is: "whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc"
            }
          }
          Buy: {
            Currency: {
              MintAddress: { is: "So11111111111111111111111111111111111111112" }
            }
          }
        }
      }
    ) {
      Transaction {
        Signature
      }
      Block {
        Time
      }
      Trade {
        Dex {
          ProgramAddress
          ProtocolName
          ProtocolFamily
        }
        Buy {
          Account {
            Address
          }
          Amount
          AmountInUSD
          Currency {
            MintAddress
            Symbol
            Name
          }
          Price
          PriceInUSD
        }
        Sell {
          Account {
            Address
          }
          Amount
          AmountInUSD
          Currency {
            MintAddress
            Symbol
            Name
          }
          Price
          PriceInUSD
        }
      }
    }
  }
}
```

## Latest price of a token

You can use the following query to get the latest price of a token, we have used WSOL address here in the below example. We are getting realtime price of WSOL on Orca DEX on Solana in different pools.

You can run this query using [this link](https://ide.bitquery.io/Price-of-a-token-on-Orca).

```subscription{
  Solana {
    DEXTradeByTokens(
      where: {Trade: {Currency: {MintAddress: {is: "So11111111111111111111111111111111111111112"}}, Dex: {ProgramAddress: {is: "whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc"}}}}
    ) {
      Block {
        Time
      }
      Trade {
        Amount
        PriceAgainstSideCurrency: Price
        Currency {
          Symbol
          Name
          MintAddress
        }
        Side {
          Amount
          Currency {
            Symbol
            Name
            MintAddress
          }
        }
        Dex {
          ProgramAddress
          ProtocolFamily
          ProtocolName
        }
        Market {
          MarketAddress
        }
        Order {
          LimitAmount
          LimitPrice
          OrderId
        }
      }
    }
  }
}


```

## Orca OHLC API

You can also query OHLC data in real time from Orca DEX.

[Here](https://ide.bitquery.io/Orca-OHLC-for-all-currencies) is Websocket to get OHLC data for all currencies, however, if you want to get OHLC data for any specific currency pair, you can use [this Websocket api](https://ide.bitquery.io/Orca-OHLC-for-specific-pair_3).

```graphql
subscription {
  Solana {
    DEXTradeByTokens(
      orderBy: { descendingByField: "Block_Timefield" }
      where: {
        Trade: {
          Currency: {
            MintAddress: { is: "6D7NaB2xsLd7cauWu1wKk6KBsJohJmP2qZH9GEfVi5Ui" }
          }
          Side: {
            Currency: {
              MintAddress: { is: "So11111111111111111111111111111111111111112" }
            }
          }
          Dex: {
            ProgramAddress: {
              is: "whirLbMiicVdio4qvUfM5KAg6Ct8VwpYzGff3uctyCc"
            }
          }
        }
      }
    ) {
      Block {
        Timefield: Time(interval: { in: minutes, count: 1 })
      }
      volume: sum(of: Trade_Amount)
      Trade {
        high: Price(maximum: Trade_Price)
        low: Price(minimum: Trade_Price)
        open: Price(minimum: Block_Slot)
        close: Price(maximum: Block_Slot)
      }
      count
    }
  }
}
```

## Video Tutorial | How to Track Latest Trades, Latest Price of a Token on Solana Orca DEX

<VideoPlayer url="https://youtu.be/IWgf37jTUuc?si=T4Sl4Np1lXrVqY5J" />

## Video Tutorial | How to Track Latest Created Liquidity Pools, OHLC data of a specific pair on Solana Orca DEX

<VideoPlayer url="https://youtu.be/7gwT9fbWl9M?si=zS14U2FV4wG1PoGf" />