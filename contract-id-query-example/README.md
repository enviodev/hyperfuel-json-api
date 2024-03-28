## Queries can be made by sending a json post request to the endpoint: `https://fuel.hypersync.xyz/query`

# contract id query example
Queries one block (8076516) for transactions and receipts that match on the `contract_id` field.  This returns all receipts that are from the contract `0xff63ad3cdb5fde197dfa2d248330d458bffe631bda65938aa7ab7e37efa561d0`.  In the response, receipts with type `5` and `6` are `Log` and `LogData`, respectively.

## query as curl request
You can paste this curl command into your terminal to execute the query from `contract-id-query-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --data '{
        "from_block": 8076516,
		"to_block":   8076517,
        "transactions": [
			{
				"contract_id": ["0xff63ad3cdb5fde197dfa2d248330d458bffe631bda65938aa7ab7e37efa561d0"]
			}
		],
        "field_selection": {
			"receipt": [
				"tx_id",
				"asset_id",
				"amount",
				"param1",
				"param2",
				"ra",
				"rb",
				"rc",
				"rd",
				"to",
				"result",
				"data",
				"receipt_type",
				"contract_id"
			]
		}
    }'
```

