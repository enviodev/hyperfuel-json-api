### Queries can be made by sending a json post request to the endpoint: `https://fuel.hypersync.xyz/query`

# predicate root query example

Previously it was mentioned that there would be a need to query for a given predicate root.  This example query searches for all insances where a given address is the `owner` field on an Input type.  `owner` is ["The owning address or predicate root."](https://docs.fuel.network/docs/beta-4/graphql/reference/objects/#inputcoin) of an InputCoin Input type.  We search for instances of this `owner` over the block range 20,001 (inclusive) to 30,000 (exclusive) and return the block `id` field along with receipt `tx_id`, `owner`, and `block_height` fields where there are instances of that `owner` address.