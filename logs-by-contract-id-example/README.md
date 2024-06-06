## Queries can be made by sending a json post request to the endpoint: `https://fuel-testnet.hypersync.xyz/query`

# logs by contract id example
Queries from block 0 to 1300000 for all `LogData` receipts that were emitted by contract `0x4a2ce054e3e94155f7092f7365b212f7f45105b74819c623744ebcc5d065c6ac`.

## query as curl request
You can paste this curl command into your terminal to execute the query from `logs-by-contract-id-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel-testnet.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --data '{
    "from_block": 0,
    "to_block": 1300000,
    "receipts": [
        {
            "root_contract_id": [
                "0x4a2ce054e3e94155f7092f7365b212f7f45105b74819c623744ebcc5d065c6ac"
            ],
            "receipt_type": [
                6
            ]
        }
    ],
    "field_selection": {
        "receipt": [
            "tx_id",
            "block_height",
            "root_contract_id",
            "ra",
            "rb",
            "rc",
            "rd",
            "data",
            "receipt_type"
        ]
    }
}'
```

