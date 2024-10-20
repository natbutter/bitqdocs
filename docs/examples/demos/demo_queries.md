
# Trades for any wallet address
```
{
  ethereum(network: bsc) {
    dexTrades(
      options: {desc: "block.height", limit: 1}
      makerOrTaker: {is: "0xeF1F0eB4e392a45986D7cE889C95c086FB170E1e"}
      exchangeName: {in: ["Pancake", "Pancake v2"]}
      date: {after: "2021-04-28"}
    ) {
      transaction {
        hash
      }
      smartContract {
        address {
          address
        }
        contractType
        currency {
          name
        }
      }
      tradeIndex
      date {
        date
      }
      block {
        height
      }
      buyAmount
      buyAmountInUsd: buyAmount(in: USD)
      buyCurrency {
        symbol
        address
      }
      sellAmount
      sellAmountInUsd: sellAmount(in: USD)
      sellCurrency {
        symbol
        address
      }
      sellAmountInUsd: sellAmount(in: USD)
      tradeAmount(in: USD)
      transaction {
        gasValue
        gasPrice
        gas
      }
    }
  }
}
```

#Latest radiyum dex liquidity trading pools created
```
subscription {
  Solana {
    Instructions(
      where: {Transaction: {Result: {Success: true}}, Instruction: {Program: {Method: {is: "initializeUserWithNonce"}, Address: {is: "675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8"}}}}
    ) {
      Block {
        Time
        Date
      }
      Transaction {
        Signature
      }
      Instruction {
        AncestorIndexes
        CallerIndex
        Depth
        Data
        ExternalSeqNumber
        InternalSeqNumber
        Index
        Accounts {
          Address
          IsWritable
          Token {
            Mint
            Owner
            ProgramId
          }
        }
        CallPath
        Logs
        Program {
          AccountNames
          Method
          Json
          Name
          Arguments {
            Type
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
      }
    }
  }
}

# New token launches on pump.fun dex
```
subscription {
  Solana {
    Instructions(
      where: {Instruction: {Program: {Method: {is: "create"}, Name: {is: "pump"}}}}
    ) {
      Instruction {
        Accounts {
          Address
          IsWritable
          Token {
            Mint
            Owner
            ProgramId
          }
        }
        Logs
        Program {
          AccountNames
          Address
          Arguments {
            Name
            Type
            Value {
              ... on Solana_ABI_Json_Value_Arg {
                json
              }
              ... on Solana_ABI_Float_Value_Arg {
                float
              }
              ... on Solana_ABI_Boolean_Value_Arg {
                bool
              }
              ... on Solana_ABI_Bytes_Value_Arg {
                hex
              }
              ... on Solana_ABI_BigInt_Value_Arg {
                bigInteger
              }
              ... on Solana_ABI_Address_Value_Arg {
                address
              }
              ... on Solana_ABI_String_Value_Arg {
                string
              }
              ... on Solana_ABI_Integer_Value_Arg {
                integer
              }
            }
          }
          Method
          Name
        }
      }
      Transaction {
        Signature
      }
    }
  }
}
```

# Realtime dex trades on Solana
```
subscription {
  Solana {
    DEXTrades {
      Trade {
        Dex {
          ProgramAddress
          ProtocolFamily
          ProtocolName
        }
        Buy {
          Amount
          Account {
            Address
          }
          Currency {
            MetadataAddress
            Key
            IsMutable
            EditionNonce
            Decimals
            CollectionAddress
            Fungible
            Symbol
            Native
            Name
          }
          Order {
            LimitPrice
            LimitAmount
            OrderId
          }
        }
        Market {
          MarketAddress
        }
        Sell {
          Account {
            Address
          }
          Currency {
            MetadataAddress
            Key
            IsMutable
            EditionNonce
            Decimals
            CollectionAddress
            Fungible
            Symbol
            Native
            Name
          }
        }
      }
      Instruction {
        Program {
          Address
          AccountNames
          Method
          Parsed
          Name
        }
      }
    }
  }
}
```

# Live price of a token
```
subscription {
  Solana {
    DEXTradeByTokens(
      limit: {count: 1}
      orderBy: {descending: Block_Time}
      where: {Trade: {Currency: {MintAddress: {is: "So11111111111111111111111111111111111111112"}}, Side: {Currency: {MintAddress: {is: "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"}}}}}
    ) {
      Block {
        Time
      }
      Trade {
        Price
      }
    }
  }
}
```

# Solana dex pump.fun tokens by market cap
```
{
  Solana {
    DEXTrades(
      limitBy:{
        by:Trade_Buy_Currency_MintAddress
        count:1
      }
      orderBy: {descending: Trade_Buy_Price}
      where: {Trade: {Dex: {ProtocolName: {is: "pump"}}
      Buy:{
        Currency:{
          MintAddress:{
            notIn:["11111111111111111111111111111111"]
          }
        }
      }
      },
        Transaction: {Result: {Success: true}}}
    ) {
      # All pump fun token has 1 billion supply, 
      # so multiply price * 1 billion to get marketcap
      # results are already sorted by price
      Trade {
        Buy {
          Price
          PriceInUSD
          Currency {
            Name
            Symbol
            MintAddress
            Decimals
            Fungible
            Uri
          }
        }
      }
    }
  }
}
```

# Raydium DEX OHLC Solana 
```
{
  Solana {
    DEXTradeByTokens(
      orderBy: {descendingByField: "Block_Timefield"}
      where: {Trade: {Dex: {ProgramAddress: {is: "675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8"}}, Currency: {MintAddress: {is: "6D7NaB2xsLd7cauWu1wKk6KBsJohJmP2qZH9GEfVi5Ui"}}, Side: {Currency: {MintAddress: {is: "So11111111111111111111111111111111111111112"}}}}}
      limit: {count: 10}
    ) {
      Block {
        Timefield: Time(interval: {in: minutes, count: 1})
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

# Raydium dex Solana OHLC specific token
```
subscription {
  Solana {
    DEXTradeByTokens(
      orderBy: {descending: Block_Time}
      where: {Trade: {Currency: {
        MintAddress: 
        {is: "6D7NaB2xsLd7cauWu1wKk6KBsJohJmP2qZH9GEfVi5Ui"}}
        Side:{
          Currency:{
            MintAddress:{
              is:"So11111111111111111111111111111111111111112"
            }
          }
        }
        , Dex: {ProgramAddress: {is: "675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8"}}}}
    ) {
      Block {
        Time(interval: {in: seconds, count: 15})
      }
      volume: sum(of: Trade_Amount)
      Trade {
        Amount
        Currency {
          Name
          Symbol
          MintAddress
        }
        Account {
          Address
        }
        Price
        Side {
          Currency {
            Name
            Symbol
            MintAddress
          }
          Amount
        }
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