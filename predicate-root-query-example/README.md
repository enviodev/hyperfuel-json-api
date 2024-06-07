## Queries can be made by sending a json post request to the endpoint: `https://fuel-testnet.hypersync.xyz/query`


# predicate root query example
This example query searches for all insances where a given address is the `owner`.  `owner` is ["The owning address or predicate root."](https://docs.fuel.network/docs/specs/tx-format/input/#inputcoin) of an InputCoin Input type.  We search for instances of this `owner` over the block range 0 (inclusive) to 1427625 (exclusive) and return the data of that coin input.

## query as curl request
You can paste this command into your terminal to execute the query from `predicate-root-query-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel-testnet.hypersync.xyz/query \
  --header 'User-Agent: insomnium/0.2.3-a' \
  --data '{
    "from_block": 0,
    "to_block": 1427625,
    "inputs": [
        {
            "owner": [
                "0x94a8e322ff02baeb1d625e83dadf5ec88870ac801da370d4b15bbd5f0af01169"
            ]
        }
    ],
    "field_selection": {
        "input": [
            "tx_id",
            "block_height",
            "input_type",
            "utxo_id",
            "owner",
            "amount",
            "asset_id",
            "predicate_gas_used",
            "predicate",
            "predicate_data"
        ]
    }
}'
```