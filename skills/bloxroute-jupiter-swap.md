---
name: Quote and execute a Jupiter swap
description: Get a best-route price quote from Jupiter through the bloXroute Solana Trader API, build the unsigned swap transaction, then sign and submit it for fast landing.
api: grpc/bloxroute-solana-trader.proto
generated: '2026-07-18'
method: generated
operations: [GetJupiterQuotes, PostJupiterSwap, PostJupiterRouteSwap, GetPriorityFee, PostSubmitV2]
---

# Quote and execute a Jupiter swap

Grounded in real `service Api` RPCs in `grpc/bloxroute-solana-trader.proto`. Requires the `Authorization` header on every call.

## Steps
1. **Quote.** Call `GetJupiterQuotes` with the input/output mints and amount to get best-route pricing. (Use `GetJupiterPrices` for spot prices.)
2. **Build the swap.** Call `PostJupiterSwap` to receive an unsigned swap transaction for the chosen quote, or `PostJupiterRouteSwap` to swap along a specific route. For advanced composition use `PostJupiterSwapInstructions` to get raw instructions you assemble into your own transaction.
3. **Price the priority fee.** Call `GetPriorityFee` and set your compute-unit price; add a tip for faster landing.
4. **Sign and submit.** Sign the returned transaction locally and submit with `PostSubmitV2` (or `PostSubmitBatchV2` for multiple).
5. **Confirm.** Poll `GetTransaction`; use `GetTransactionTrace` to diagnose delays.

## Rules
- The swap RPCs return **unsigned** transactions — you sign; bloXroute never holds keys.
- Re-quote if the quote is stale before signing; slippage settings belong in the quote/swap request.
- Pump.fun analogues exist (`GetPumpFunQuotes` → `PostPumpFunSwap`/`PostPumpFunSwapSol`/`PostPumpFunAmmSwap`) for the same pattern.
