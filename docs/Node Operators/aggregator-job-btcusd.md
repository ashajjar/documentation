---
layout: nodes.liquid
date: Last Modified
title: "Aggregator Job (BTC/USD)"
permalink: "docs/aggregator-job-btcusd/"
hidden: true
---
In order to contribute to the price aggregator contract, you will need to run one of the jobs on this page. The Chainlink team will let you know which API to provide, and you can use this page as a guide for adding jobs for each provider to your node.

First, add a test job to your node which uses the Web initiator so that you can validate that the job will run successfully. If the API requires an API key, replace `YOUR_API_KEY` with your own.

If using an <a href="https://chainlinkadapters.com/" target="_blank">external adapter</a>, follow [these instructions](../node-operators) for adding that bridge to your node. Replace `YOUR_BRIDGE_NAME` with the name of your bridge in the job spec.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://bravenewcoin-v1.p.rapidapi.com/convert?qty=1&from=btc&to=usd\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"x-rapidapi-key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t],\n          \"x-rapidapi-host\": [\n          \t\"bravenewcoin-v1.p.rapidapi.com\"\n          ]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonParse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"to_quantity\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test BNC"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://rest.coinapi.io/v1/exchangerate/BTC/USD?apikey=YOUR_API_KEY\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"rate\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test CoinApi"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.cryptoapis.io/v1/exchange-rates/BTC/USD\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"X-API-Key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonParse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"payload\",\n\t\t\t\t\t\"weightedAveragePrice\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test CryptoAPIs"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"bitcoin\",\n\t\t\t\t\t\"usd\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test CoinGecko"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://pro-api.coinmarketcap.com/v1/cryptocurrency/quotes/latest?symbol=BTC&convert=USD\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"X-CMC_PRO_API_KEY\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonParse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"data\",\n\t\t\t\t\t\"BTC\",\n\t\t\t\t\t\"quote\",\n\t\t\t\t\t\"USD\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test CoinMarketCap"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.coinpaprika.com/v1/tickers/btc-bitcoin\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"quotes\",\n\t\t\t\t\t\"USD\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test Coinpaprika"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://min-api.cryptocompare.com/data/price?fsym=BTC&tsyms=USD&apikey=YOUR_API_KEY\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"USD\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test CryptoCompare"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://us.market-api.kaiko.io/v1/data/trades.v1/spot_direct_exchange_rate/btc/usd/recent\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"X-Api-Key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonParse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"data\",\n\t\t\t\t\t\"0\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test Kaiko"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://web3api.io/api/v2/market/prices/btc/latest?quote=usd\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"x-api-key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"payload\",\n          \"btc_usd\",\n          \"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test Amberdata"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://www.alphavantage.co/query?function=CURRENCY_EXCHANGE_RATE&from_currency=BTC&to_currency=USD&apikey=YOUR_API_KEY\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"Realtime Currency Exchange Rate\",\n          \"5. Exchange Rate\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test Alpha Vantage"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"web\"\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.nomics.com/v1/currencies/ticker?key=YOUR_API_KEY&ids=BTC&interval=1d&convert=USD\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"0\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Test Nomics"
    },
    {
      "code": "{\n    \"initiators\": [\n        {\n            \"type\": \"web\"\n        }\n    ],\n    \"tasks\": [\n        {\n            \"params\": {\n                \"from\": \"BTC\",\n                \"to\": \"USD\"\n            },\n            \"type\": \"YOUR_BRIDGE_NAME\"\n        },\n        {\n            \"params\": {\n                \"copyPath\": [\n                    \"result\"\n                ]\n            },\n            \"type\": \"copy\"\n        },\n        {\n            \"params\": {\n                \"times\": 100000000\n            },\n            \"type\": \"multiply\"\n        },\n        {\n            \"type\": \"ethint256\"\n        }\n    ]\n}",
      "language": "json",
      "name": "Test Bridge"
    }
  ]
}
[/block]
On the Job Spec Detail page for the job, click the Run button. You should see a green bar at the top that the node Successfully created job run <JobRunID>. Click on that Job Run ID and verify that all tasks have been Completed with green check marks.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8cfc185-Screenshot_from_2019-06-21_08-29-07.png",
        "Screenshot from 2019-06-21 08-29-07.png",
        1894,
        806,
        "#fafafb"
      ]
    }
  ]
}
[/block]
If the test job has ran successfully, add the following job to your node, replacing `YOUR_ORACLE_CONTRACT_ADDRESS` with your oracle contract address, and if required, replacing `YOUR_API_KEY` with yours as well.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://bravenewcoin-v1.p.rapidapi.com/convert?qty=1&from=btc&to=usd\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"x-rapidapi-key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t],\n          \"x-rapidapi-host\": [\n          \t\"bravenewcoin-v1.p.rapidapi.com\"\n          ]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonParse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"to_quantity\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "BNC"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://rest.coinapi.io/v1/exchangerate/BTC/USD?apikey=YOUR_API_KEY\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"rate\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "CoinApi"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"bitcoin\",\n\t\t\t\t\t\"usd\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "CoinGecko"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://pro-api.coinmarketcap.com/v1/cryptocurrency/quotes/latest?symbol=BTC&convert=USD\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"X-CMC_PRO_API_KEY\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"data\",\n\t\t\t\t\t\"BTC\",\n\t\t\t\t\t\"quote\",\n\t\t\t\t\t\"USD\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "CoinMarketCap"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.coinpaprika.com/v1/tickers/btc-bitcoin\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"quotes\",\n\t\t\t\t\t\"USD\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Coinpaprika"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.cryptoapis.io/v1/exchange-rates/BTC/USD\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"X-API-Key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonParse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"payload\",\n\t\t\t\t\t\"weightedAveragePrice\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "CryptoAPIs"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://min-api.cryptocompare.com/data/price?fsym=BTC&tsyms=USD&apikey=YOUR_API_KEY\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"USD\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "CryptoCompare"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://web3api.io/api/v2/market/prices/btc/latest?quote=usd\",\n\t\t\t\t\"headers\": {\n\t\t\t\t\t\"x-api-key\": [\n\t\t\t\t\t\t\"YOUR_API_KEY\"\n\t\t\t\t\t]\n\t\t\t\t}\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"payload\",\n          \"btc_usd\",\n          \"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Amberdata"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://www.alphavantage.co/query?function=CURRENCY_EXCHANGE_RATE&from_currency=BTC&to_currency=USD&apikey=YOUR_API_KEY\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"Realtime Currency Exchange Rate\",\n          \"5. Exchange Rate\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Alpha Vantage"
    },
    {
      "code": "{\n\t\"initiators\": [\n\t\t{\n\t\t\t\"type\": \"runlog\",\n\t\t\t\"params\": {\n\t\t\t\t\"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n\t\t\t}\n\t\t}\n\t],\n\t\"tasks\": [\n\t\t{\n\t\t\t\"type\": \"httpget\",\n\t\t\t\"params\": {\n\t\t\t\t\"get\": \"https://api.nomics.com/v1/currencies/ticker?key=YOUR_API_KEY&ids=BTC&interval=1d&convert=USD\"\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"jsonparse\",\n\t\t\t\"params\": {\n\t\t\t\t\"path\": [\n\t\t\t\t\t\"0\",\n\t\t\t\t\t\"price\"\n\t\t\t\t]\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"multiply\",\n\t\t\t\"params\": {\n\t\t\t\t\"times\": 100000000\n\t\t\t}\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethint256\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"ethtx\"\n\t\t}\n\t]\n}",
      "language": "json",
      "name": "Nomics"
    },
    {
      "code": "{\n    \"initiators\": [\n        {\n            \"params\": {\n                \"address\": \"YOUR_ORACLE_CONTRACT_ADDRESS\"\n            },\n            \"type\": \"runlog\"\n        }\n    ],\n    \"tasks\": [\n        {\n            \"params\": {\n                \"from\": \"BTC\",\n                \"to\": \"USD\"\n            },\n            \"type\": \"YOUR_BRIDGE_NAME\"\n        },\n        {\n            \"params\": {\n                \"copyPath\": [\n                    \"result\"\n                ]\n            },\n            \"type\": \"copy\"\n        },\n        {\n            \"params\": {\n                \"times\": 100000000\n            },\n            \"type\": \"multiply\"\n        },\n        {\n            \"type\": \"ethint256\"\n        },\n        {\n            \"type\": \"ethtx\"\n        }\n    ]\n}",
      "language": "json",
      "name": "Bridge"
    }
  ]
}
[/block]
Once added, give the team the Job ID associated with the job above.

Make sure that your `MINIMUM_CONTRACT_PAYMENT` environment variable is low enough to accept the payment amount from the aggregator contract on mainnet.