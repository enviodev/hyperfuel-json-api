### Queries can be made by sending a json post request to the endpoint: `https://fuel.hypersync.xyz/query`

# contract id query example
This example draws inspiration from an issue with Fuel's old indexer: [https://github.com/FuelLabs/fuel-indexer/issues/1401](https://github.com/FuelLabs/fuel-indexer/issues/1401)

We query the entire chain (`"from_block": 0`, and no `"to_block"` specified) for the given `"contract_id"` and return the `id` field on blocks associated with that contract id and the `tx_id`, `block_height`, `ra`, `rb`, and `receipt_type` fields on all receipts where `contract_id` == given address.  In a real situation more fields than this will probably be needed.

In the result you will find some pairs of entries under `receipts` which are the two transactions that raised the original github issue because they weren't being picked up in the old indexer:

```json
{
    "tx_id": "0x8cf035a39411b9ac747452b7e59984bba2613300da4660db1c36bd8985bd9b4e",
    "block_height": 4293027,
    "ra": 0,
    "rb": 0,
    "receipt_type": 6 // 6 = LogData receipt type
},
{
    "tx_id": "0x8cf035a39411b9ac747452b7e59984bba2613300da4660db1c36bd8985bd9b4e",
    "block_height": 4293027,
    "ra": 0,
    "rb": 1,
    "receipt_type": 6 // 6 = LogData receipt type
},
```