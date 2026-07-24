---
name: Subscribe to a bloXroute mempool or event stream
description: Open a subscription to bloXroute BDN streams (newTxs, pendingTxs, txReceipts, newBlocks) or Solana Trader gRPC streams and process notifications, then cancel cleanly.
api: asyncapi/bloxroute-streams-asyncapi.yml
generated: '2026-07-18'
method: generated
operations: [GetPumpFunNewTokensStream, GetPumpFunSwapsStream, GetPriorityFeeStream, GetRecentBlockHashStream, GetTradesStream]
---

# Subscribe to a bloXroute mempool or event stream

Grounded in the documented stream surface (`asyncapi/bloxroute-streams-asyncapi.yml`) and the server-streaming RPCs in `grpc/bloxroute-solana-trader.proto`. Every subscription carries the `Authorization` header.

## Steps
1. **Pick a transport.** BDN mempool/block streams (EVM chains: `newTxs`, `pendingTxs`, `txReceipts`, `newBlocks`, `bdnBlocks`, `ethOnBlock`) are consumed over the Cloud-API WebSocket (JSON-RPC subscribe). Solana Trader streams (`GetPumpFunNewTokensStream`, `GetPumpFunSwapsStream`, `GetPriorityFeeStream`, `GetBundleTipStream`, `GetRecentBlockHashStream`, `GetTradesStream`) are server-streaming gRPC.
2. **Create the subscription.** Send `subscribe` with the stream name and any filters (see `eth/streams/working-with-streams/creating-a-subscription`); for gRPC, open the streaming RPC.
3. **Handle notifications** as they arrive; apply filters server-side where supported to cut bandwidth (`newtxs-and-pendingtxs/filter`).
4. **Cancel** the subscription when done (`cancelling-a-subscription`) or close the gRPC stream to stop billing/credit consumption.

## Rules
- Use `bdnBlocks`/OFR streams when you need data ahead of your local node; validate `pendingTxs` against a local node when correctness matters.
- Streams are read-only; act on them via the submission skills.
