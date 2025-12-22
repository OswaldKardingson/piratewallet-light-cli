# Qortal ARRR Lightwallet CLI Integration Notes

This fork tracks Pirate's current lightwalletd gRPC protocol (`pirate.wallet.sdk.rpc`) and drops the older
`cash.z.wallet.sdk.rpc` namespace used by `lightd.pirate.black`.

## Known Mainnet Lightwalletd Servers (new proto)
- https://lightd1.pirate.black:443
- https://piratelightd1.cryptoforge.cc:443
- https://piratelightd2.cryptoforge.cc:443

## JSON Schema Changes (Core/JNI parsers)
### `syncstatus`
Legacy key changes:
- Removed: `syncing` (replaced by `in_progress`)
- `synced_blocks` and `total_blocks` are still emitted, but only while syncing

New fields when syncing:
`sync_id`, `in_progress`, `last_error`, `start_block`, `end_block`,
`synced_blocks`, `trial_decryptions_blocks`, `txn_scan_blocks`,
`total_blocks`, `batch_num`, `batch_total`

New fields when not syncing:
`sync_id`, `in_progress`, `last_error`, `scanned_height`

### `list`
- Always includes `incoming_metadata` and `outgoing_metadata`.
- Adds `incoming_metadata_change` and `outgoing_metadata_change`.
- Includes `unconfirmed` (false for confirmed transactions).
- Mempool entries are no longer appended to the list output.
- `amount` and `fee` now account for both shielded and transparent totals.

### `balance`
Keys are unchanged, but with t-address support disabled by default:
- `t_addresses` is empty and `tbalance` is zero unless t-addresses are explicitly added.
