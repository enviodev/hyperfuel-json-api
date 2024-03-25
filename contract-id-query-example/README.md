## Queries can be made by sending a json post request to the endpoint: `https://fuel.hypersync.xyz/query`

# contract id query example
Queries one block (7988155) for transactions and receipts that match on the `contract_id` field.  This returns all transactions that have `transaction.contract_id == 0xd2a93abef5c3f45f48bb9f0736ccfda4c3f32c9c57fc307ab9363ef7712f305f` and `transaction.contract_ids.includes(0xd2a93abef5c3f45f48bb9f0736ccfda4c3f32c9c57fc307ab9363ef7712f305f)` along with all receipt objects in the returned transactions.

## query as curl request
You can paste this curl command into your terminal to execute the query from `contract-id-query-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnium/0.2.3-a' \
  --data '{
        "from_block": 7988155,
				"to_block":   7988156,
        "transactions": [
					{
					"contract_id": ["0xd2a93abef5c3f45f48bb9f0736ccfda4c3f32c9c57fc307ab9363ef7712f305f"]
					}
				],
        "field_selection": {
       		"transaction": [
						"input_contracts",
						"input_contract",
						"id"
					],
					"receipt": [
						"tx_id",
						"asset_id",
						"amount",
						"param1",
						"param2",
						"to",
						"result",
						"receipt_type"
					]
        }
    }'
```

