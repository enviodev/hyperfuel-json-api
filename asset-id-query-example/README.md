### Queries can be made by sending a json post request to the endpoint: `https://fuel-testnet.hypersync.xyz/query`

# asset id query example
We query from blocks 0 (inclusive) to 1300000 (exclusive) for all `Inputs` where the address `0x2a0d0ed9d2217ec7f32dcd9a1902ce2a66d68437aeff84e3a3cc8bebee0d2eea` matches on an input's `asset_id` field.

## query as curl request
You can paste this command into your terminal to execute the query from `asset-id-query-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel-testnet.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --data '{
        "from_block": 0,
        "to_block":   1300000,
        "inputs": [
            {
            "asset_id": ["0x2a0d0ed9d2217ec7f32dcd9a1902ce2a66d68437aeff84e3a3cc8bebee0d2eea"]
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