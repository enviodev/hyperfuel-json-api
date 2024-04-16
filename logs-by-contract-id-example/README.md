## Queries can be made by sending a json post request to the endpoint: `https://fuel-15.hypersync.xyz/query`

# logs by contract id example
Queries one block (8076516) for all `Log` and `LogData` receipts that were emitted by contract `0xff63ad3cdb5fde197dfa2d248330d458bffe631bda65938aa7ab7e37efa561d0`.

## query as curl request
You can paste this curl command into your terminal to execute the query from `logs-by-contract-id-example.json` as a curl request.

```bash
curl --request POST \
  --url https://fuel-15.hypersync.xyz/query \
  --header 'Content-Type: application/json' \
  --data '{
        "from_block": 8076516,
		"to_block":   8076517,
        "receipts": [
			{
				"root_contract_id": ["0xff63ad3cdb5fde197dfa2d248330d458bffe631bda65938aa7ab7e37efa561d0"],
				"receipt_type": [5, 6]
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

