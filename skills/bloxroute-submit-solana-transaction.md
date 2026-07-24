---
name: Submit a Solana transaction with low latency
description: Optimize and submit a signed Solana transaction through the bloXroute Solana Trader API for the fastest possible landing, using priority-fee and blockhash helpers.
api: grpc/bloxroute-solana-trader.proto
generated: '2026-07-18'
method: generated
operations: [GetRecentBlockHashV2, GetPriorityFee, PostSubmitV2, GetTransactionTrace, GetRateLimit]
---

# Submit a Solana transaction with low latency

Grounded in real `service Api` RPCs in `grpc/bloxroute-solana-trader.proto`. Regional gRPC/HTTP hosts: `uk.solana.dex.blxrbdn.com`, `ny.solana.dex.blxrbdn.com`. Every request must carry the `Authorization` header (account secret from the bloXroute Account Portal — see `authentication/bloxroute-authentication.yml`).

## Steps
1. **Build with a fresh blockhash.** Call `GetRecentBlockHashV2` to get a current blockhash for transaction construction.
2. **Size the priority fee.** Call `GetPriorityFee` to read the top-percentile recent priority fee for your project over the last 100 slots, and set your compute-unit price accordingly. Add a tip to a bloXroute tipping address for faster landing (see `solana/trader-api/introduction/tip-and-tipping-addresses`).
3. **Sign locally**, then submit with `PostSubmitV2` (single tx). For competing token buys use `PostSubmitSnipeV2`; for many txs in one call use `PostSubmitBatchV2`.
4. **Confirm / diagnose.** Poll `GetTransaction` for status, or `GetTransactionTrace` to see when bloXroute received/released the tx and whether it was delayed.
5. **Watch your budget.** `GetRateLimit` returns current Trader-API credit usage (see `rate-limits/bloxroute-rate-limits.yml`) — credit-based limiting applies to submission on rate-limited chains.

## Rules
- No idempotency-key header exists; on-chain signature dedup is the safeguard — do not blindly resubmit the same signed tx across regions.
- Choose the region nearest your infrastructure to minimize latency.
