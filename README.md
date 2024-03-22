## Queries can be made by sending a json post request to the endpoint: `https://fuel.hypersync.xyz/query`


# hypersync-fuel-docs
HyperSync is [Envio's](https://envio.dev/) high performance database and accelerated data query layer that powers Envio’s Indexing framework (HyperIndex) for 100x faster data retrieval than standard RPC methods. 

HyperSync is a real-time indexed layer of the Fuel network and is exposed as a low-level API for developers and data analysts to create niche, flexible, high-speed queries for all on-chain data. 

Users can interact with the HyperSync API using the json api (shown below) or Rust client and extract data. Hypersync can also support Python and JS packages if those are easier to integrate with your stack.

Using HyperSync, application developers do not need to worry about RPC URLs, rate-limiting, or managing infrastructure, and can easily sync large datasets in a few minutes, something that would usually take hours or days via RPC.

Hypersync supports Fuel's beta-5 network.

# Query structure
The query structure is shown in json because the examples will be of the json api.
```
{
    // The block to start the query from
    "from_block": Number,

    // The block to end the query at. If not specified, the query will go until the
    //  end of data. Exclusive, the returned range will be [from_block..to_block).
    //
    // The query will return before it reaches this target block if it hits the time limit
    //  configured on the server. The user should continue their query by putting the
    //  next_block field in the response into from_block field of their next query. This implements
    //  pagination.
    "to_block": Number, // Optional, defaults to latest block

    // List of transaction selections, the query will return transactions that match any of these selections.
    "transactions": Array [{TransactionSelection}],

    // Weather to include all blocks regardless of if they are related to a returned transaction or log Normally
    //  the server will return only the blocks that are related to the transaction or logs in the response. But if this
    //  is set to true, the server will return data for all blocks in the requested range [from_block, to_block).
    "include_all_blocks": bool, // Optional, defaults to false

    // The user selects which fields they want returned. Requesting less fields will improve
    //  query execution time and reduce the payload size so the user should always use a minimal number of fields.
    "field_selection": {FieldSelection},

    // Maximum number of blocks that should be returned, the server might return more blocks than this number but
    //  it won't overshoot by too much.
    "max_num_blocks": Number, // Optional, defaults to no maximum

    // Maximum number of transactions that should be returned, the server might return more transactions than this number but
    //  it won't overshoot by too much.
    "max_num_transactions": Number, // Optional, defaults to no maximum
}
```

# TransactionSelection
Query takes an array of TransactionSelection objects and returns transactions that match any of the selections.

Note: A TransactionSelection must be specified, even if it is empty: `"transactions": [{}]`, or else no data will match and the query will return nothing.  An empty TransactionSelection essentially says "get all data".

Below is an exhaustive list of all fields.
```
// transaction, receipt, and input field.
"contract_id": Array [String],

// transaction, receipt, input, and output field.
"asset_id": Array [String],

// transaction field
"tx_type": Array [TransactionType],

// receipt field. The recipient contract (and output)
"to": Array [String],

// receipt field. The recipient address
"to_address": Array [String],

// receipt field.  The type of receipt numbered 0-12 in order Call, Return, ReturnData, Panic, Revert, Log, LogData, Transfer, TransferOut, ScriptResult, MessageOut, Mint, Burn
"receipt_type": Array [ReceiptType],

// Input and receipt field.  The address of the recipient.
"recipient": Array [String],

// input field.  The owning address or predicate root.
"owner": Array [String],

// input and receipt field.  The sender address of the message.
"sender": Array [String],

// Transaction field.  numbered 0-3 in order Submitted, Success, SqueezedOut, and Failure
"status": Number,

```

# FieldSelection
Query takes a field selection object where the user specifies which fields on the data matched by the TransactionSelection they want data from.

Important note: all fields draw inspiration from Fuel's [graphql schema](https://docs.fuel.network/docs/beta-4/graphql/reference/objects/).  Mainly Blocks, Transactions, Receipts, Inputs, and Outputs.  Enums of each type (ex: Receipt has 12 different types, two of which are Log and LogData, Input has 3: InputCoin, InputContract, InputMessage, and Output has 5: CoinOutput, ContractOutput, ChangeOutput, VariableOutput, ContractCreated) are flattened into the parent type.  This is why so multiple fields on any returned Receipt, Input, or Output might be null; it's not a field on all possible enums of that type, so null is inserted.

Below is an exhaustive list of all fields.
```
"block": [
    "id",
    "da_height",
    "transactions_count",
    "message_receipt_count",
    "transactions_root",
    "message_receipt_root",
    "height",
    "prev_root",
    "time",
    "application_hash"
],
"transaction": [
    "block_height",
    "id",
    "input_asset_ids",
    "input_contracts",
    "input_contract_utxo_id",
    "input_contract_balance_root",
    "input_contract_state_root",
    "input_contract_tx_pointer_block_height",
    "input_contract_tx_pointer_tx_index",
    "input_contract",
    "gas_price",
    "gas_limit",
    "maturity",
    "mint_amount",
    "mint_asset_id",
    "tx_pointer_block_height",
    "tx_pointer_tx_index",
    "tx_type",
    "output_contract_input_index",
    "output_contract_balance_root",
    "output_contract_state_root",
    "witnesses",
    "receipts_root",
    "status",
    "time",
    "reason",
    "script",
    "script_data",
    "bytecode_witness_index",
    "bytecode_length",
    "salt",
],
"receipt": [
    "tx_id",
    "block_height",
    "pc",
    "is",
    "to",
    "to_address",
    "amount",
    "asset_id",
    "gas",
    "param1",
    "param2",
    "val",
    "ptr",
    "digest",
    "reason",
    "ra",
    "rb",
    "rc",
    "rd",
    "len",
    "receipt_type",
    "result",
    "gas_used",
    "data",
    "sender",
    "recipient",
    "nonce",
    "contract_id",
    "sub_id",
],
"input": [
    "tx_id",
    "block_height",
    "input_type",
    "utxo_id",
    "owner",
    "amount",
    "asset_id",
    "tx_pointer_block_height",
    "tx_pointer_tx_index",
    "witness_index",
    "predicate_gas_used",
    "predicate",
    "predicate_data",
    "balance_root",
    "state_root",
    "contract",
    "sender",
    "recipient",
    "nonce",
    "data",
],
"output": [
    "tx_id",
    "block_height",
    "output_type",
    "to",
    "amount",
    "asset_id",
    "input_index",
    "balance_root",
    "state_root",
    "contract",
]

```