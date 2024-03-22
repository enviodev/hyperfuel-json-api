## Queries can be made by sending a json post request to the endpoint: `https://fuel.hypersync.xyz/query`


# predicate root query example

Previously it was mentioned that there would be a need to query for a given predicate root.  This example query searches for all insances where a given address is the `owner`.  `owner` is ["The owning address or predicate root."](https://docs.fuel.network/docs/beta-4/graphql/reference/objects/#inputcoin) of an InputCoin Input type.  We search for instances of this `owner` over the block range 4,105,960 (inclusive) to 4,106,000 (exclusive) and return the data of that coin input.

## query as curl request
You can paste this command into your terminal to execute the query from `predicate-root-query-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnium/0.2.3-a' \
  --data '{
        "from_block": 4105960,
            "to_block": 4106000,
        "transactions": [
            {
                "owner": ["0x48a0f31c78e1c837ff6a885785ceb7c2090f86ed93db3ed2d8821d13739fe981"]
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