# Customized NFT

A CosmWasm NFT contract based on `cw721-base`, extended with collection details and richer token metadata for an NFT marketplace.

It implements the full CW721 standard, so transfers, approvals and queries work with any wallet or marketplace that understands cw721. On top of that it adds:

- Collection info set at instantiation: a collection name, description, logo URL and banner URL, plus the address that created it. Read it back with the `collection_info {}` query.
- Token metadata with a name, description, external link, royalty recipients and rates, an initial price and supply counts (`num_nfts` and `num_real_repr`).
- A `burn { token_id }` message for the owner or an approved operator.

## Messages

Instantiate:

```json
{
  "name": "My Collection",
  "symbol": "MYC",
  "minter": "juno1...",
  "collection_name": "My Collection",
  "collection_description": "A short description",
  "logo_url": "https://...",
  "banner_url": "https://..."
}
```

Mint (minter only):

```json
{
  "mint": {
    "token_id": "1",
    "owner": "juno1...",
    "token_uri": "https://.../1.json",
    "extension": {
      "name": "First piece",
      "description": "...",
      "royalties": [{ "address": "juno1...", "royalty_rate": "0.05" }],
      "init_price": "1000000"
    }
  }
}
```

Everything else follows the CW721 spec in `packages/cw721`: `transfer_nft`, `send_nft`, `approve`, `revoke`, `approve_all`, `revoke_all`, and queries like `owner_of`, `nft_info`, `all_nft_info`, `tokens` and `all_tokens`. The full JSON schemas are in `schema/`.

## Not finished yet

- `mint_number_limit` is accepted at instantiation but isn't stored or enforced, so there's no supply cap yet.
- `update_minter` checks that the caller is the current minter, but the line that saves the new minter is commented out, so the minter doesn't actually change.

## Building

You'll need Rust with the `wasm32-unknown-unknown` target.

```bash
git clone https://github.com/AI-pro017/customized-nft.git
cd customized-nft
cargo test
cargo wasm
```

Run `cargo schema` to regenerate the JSON schemas after changing the messages.

## Credits

Based on `cw721-base` from [cw-nfts](https://github.com/CosmWasm/cw-nfts) by Ethan Frey and Orkun Külçe. Apache 2.0, see [NOTICE](NOTICE).
