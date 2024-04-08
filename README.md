## Queries can be made by sending a json post request to the endpoint: `https://fuel-next.hypersync.xyz/query`

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

    /// List of receipt selections, the query will return receipts that match any of these selections and
    ///  it will return receipts that are related to the returned objects.
    "receipts": [{ReceiptSelection}], // Optional

    /// List of input selections, the query will return inputs that match any of these selections and
    ///  it will return inputs that are related to the returned objects.
    "inputs": [{InputSelection}], // Optional

    /// List of output selections, the query will return outputs that match any of these selections and
    ///  it will return outputs that are related to the returned objects.
    "outputs": [{OutputSelection}], // Optional

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

# ReceiptSelection
Query takes an array of ReceiptSelections objects and returns receipts that match any of the selections.  All fields are optional

Below is an exhaustive list of all fields.
```
    // address that emitted the receipt
    root_contract_id: [String],

    // The recipient address
    to_address: [String],

    // The asset id of the coins transferred.
    asset_id: [String],

    // the type of receipt
    // 0 = Call
    // 1 = Return,
    // 2 = ReturnData,
    // 3 = Panic,
    // 4 = Revert,
    // 5 = Log,
    // 6 = LogData,
    // 7 = Transfer,
    // 8 = TransferOut,
    // 9 = ScriptResult,
    // 10 = MessageOut,
    // 11 = Mint,
    // 12 = Burn,
    receipt_type: [Number],

    // The address of the message sender.
    sender: [String],

    // The address of the message recipient.
    recipient: [String],

    // The contract id of the current context if in an internal context. null otherwise
    contract_id: [String],
```

# InputSelection
Query takes an array of InputSelections objects and returns inputs that match any of the selections.  All fields are optional

Below is an exhaustive list of all fields.
```
    // The owning address or predicate root.
    owner: [String],

    // The asset ID of the coins.
    asset_id: [String],

    // The input contract.
    contract: [String],

    // The sender address of the message.
    sender: [String],

    // The recipient address of the message.
    recipient: [String],

    // The type of input
    // 0 = InputCoin,
    // 1 = InputContract,
    // 2 = InputMessage,
    input_type: Vec<u8>,
```

# OutputSelection
Query takes an array of OutputSelections objects and returns outputs that match any of the selections.  All fields are optional

Below is an exhaustive list of all fields.
```
    // The address the coins were sent to.
    to: [String],

    // The asset id for the coins sent.
    asset_id: [String],

    // The contract that was created.
    contract: [String],

    // the type of output
    // 0 = CoinOutput,
    // 1 = ContractOutput,
    // 2 = ChangeOutput,
    // 3 = VariableOutput,
    // 4 = ContractCreated,
    output_type: [Number],
```

# FieldSelection
Query takes a FieldSelection object where the user specifies what they want returned from data matched by their `ReceiptSelection`, `OutputSelection`, and `InputSelection`.

Important note: all fields draw inspiration from Fuel's [graphql schema](https://docs.fuel.network/docs/graphql/reference/).  Mainly Blocks, Transactions, Receipts, Inputs, and Outputs.  Enums of each type (ex: Receipt has 12 different types, two of which are Log and LogData, Input has 3: InputCoin, InputContract, InputMessage, and Output has 5: CoinOutput, ContractOutput, ChangeOutput, VariableOutput, ContractCreated) are flattened into the parent type.  This is why so multiple fields on any returned Receipt, Input, or Output might be null; it's not a field on all possible enums of that type, so null is inserted.

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
    "root_contract_id",
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