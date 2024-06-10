## Queries can be made by sending a json post request to the endpoint: `https://fuel-testnet.hypersync.xyz/query`

# hyperfuel-json-api

HyperFuel supports a json api mainly for debugging.  It's recommended to use the clients because their binary data transfer is much more efficient than json serialization.  The clients also have more idiomatic response structuring, supporting functions, data filtering, and receipt ordering.
- [python client](https://github.com/enviodev/hyperfuel-client-python)
- [rust client](https://github.com/enviodev/hyperfuel-client-rust)
- [node js client](https://github.com/enviodev/hyperfuel-client-node)

# Example curl query
In this example, we query from blocks 0 (inclusive) to 1300000 (exclusive) for all `Inputs` where the address `0x2a0d0ed9d2217ec7f32dcd9a1902ce2a66d68437aeff84e3a3cc8bebee0d2eea` matches on an input's `asset_id` field.

Paste the example curl request into your terminal!
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
