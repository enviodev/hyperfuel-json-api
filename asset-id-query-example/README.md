### Queries can be made by sending a json post request to the endpoint: `https://fuel-next.hypersync.xyz/query`

# asset id query example
We query from blocks 7980000 (inclusive) to 7980100 (exclusive) for all `Inputs` where the address `0x0000000000000000000000000000000000000000000000000000000000000000` matches on the `asset_id` field.

## query as curl request
You can paste this command into your terminal to execute the query from `asset-id-query-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel-next.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnium/0.2.3-a' \
  --data '{
        "from_block": 7980000,
        "to_block":   7980100,
        "inputs": [
            {
            "asset_id": ["0x0000000000000000000000000000000000000000000000000000000000000000"]
            }
        ],
        "field_selection": {
            "input": [
                "block_height",
                "tx_id",
                "owner",
                "amount",
                "asset_id"
            ]
        }
    }'
```