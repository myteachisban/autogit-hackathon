---
title: Sovereign Sentinel — Omnichain Liquidity & Autonomous Risk Agent
app_type: sovereign-sentinel-risk-agent
wallet: 0x4d9dc6f2bfcd4fddfba76b4e36b2d2995d1a45ac
---

Build an autonomous multi-chain liquidity monitor and automated risk management agent dashboard. The system continuously tracks capital efficiency, positions, and impending bad debt across major DeFi primitives, automatically executing defensive rebalancing, hedging, or yield-optimization logic when market risk thresholds are crossed.

## Target Users

On-chain asset managers, Web3 treasuries, active DeFi participants, and automated yield aggregators who need continuous risk auditing and automated execution to preserve capital across fluctuating market conditions.

## Pages and Features

### 1. Home — Risk Command Center

- Hero section showing aggregate portfolio health metrics: Real-Time Value at Risk (VaR) in USD with 24h change percentage and dollar delta (green/red).
- Wallet input field at the top: paste any `0x` address or ENS name to load a portfolio layout (read-only, no active wallet connection required).
- Support up to 5 watched protocol smart contracts simultaneously, stored in localStorage.
- Chain selector tabs: All Chains / Ethereum / Base / Arbitrum / Optimism / Polygon.
- Monitored Liquidity & Position Table:
  - Columns: Protocol logo, Pool Name, Target Asset Pair, Total Liquidity, Current Yield/APY, Risk Score, Health Factor.
  - Sortable by any column.
  - Color-coded risk indicators (green for stable, yellow for warning, red for critical).
  - Show small balances toggle (hide positions worth less than $100).
- Dynamic Sentinel State Indicator: A highly visible operational status badge showing live states: `MONITORING`, `EVALUATING_THREAT`, `EXECUTING_HEDGE`, or `SAFE`.

### 2. Transactions Page

- Full transaction and analytical history for the automated agent operations.
- Filter bar: All / Collateral Migrations / Hedging Actions / Emergency Liquidations / Yield Rebalancing.
- Date range picker for auditing specific historical periods.
- Each row: Transaction hash (truncated, links directly to Etherscan/Basescan), action type badge, from/to protocol layers, asset type, execution gas used, and precise timestamp.
- Infinite scroll pagination.

### 3. Analytics Page

- Portfolio Value & Risk Exposure over time line chart (1D / 1W / 1M / 3M / 1Y / All).
- Asset Allocation & Liquidity Distribution donut chart categorized by protocol, token, and chain ecosystem.
- Deep Diagnostics Panel showing the Agent Cognitive Reason Paths (Chain-of-Thought terminal) for selected execution blocks.
- Realized and unrealized protection gains tracker showing capital saved by automated intervention.

### 4. Alerts Page

- Form-based playground interface to define custom threshold rules for the Autonomous Agent.
- Trigger configuration: If Pool Liquidity drops over 25% within 10 minutes OR Lending Pool Health Factor drops below 1.15.
- Agent Execution Prompt Workflow Builder (e.g., "If threshold is tripped -> Execute Emergency Flash Loan -> Repay Vulnerable Debt Position").
- Form to create standard alert configurations: pool metric targets, condition (above/below), target notification method (webhook feed or browser push).

## UI Layout and Components

- Left sidebar navigation (collapsible on mobile): icons + labels for Home, Transactions, Analytics, Alerts.
- Top bar: monitored core address wallet display, global multi-chain network indicators, active gas fee trackers, and a smooth dark/light theme switch.
- Bento Box Architecture: All data structures are housed in clean grid cards utilizing tight borders with zero heavy shadows.
- High-contrast color scheme: Deep charcoal default dark mode heavily highlighted with **Electric Lime Green (`#c0fd5c`)** neon accents for extreme data hierarchy visibility.
- Skeleton loading states for high-frequency logs and live analytics streams.
- Responsive: sidebar collapses to bottom tab bar on mobile screens (< 768px).

## Data and State

- Ingest live protocol and asset tracking points from open-source indices, automated market makers (AMMs), and credit protocols (mock the responses with highly structured realistic dummy JSON data models if endpoints are unreachable at build time).
- Cache real-time responses in localStorage using a strict 30-second Time-To-Live (TTL) limitation framework.
- Global state managed natively via a lightweight architecture like React Context or Zustand with automated data hydration.
- All monetary positions and USD deltas formatted meticulously using `Intl.NumberFormat`.

## Tech Stack

- TypeScript, React, Tailwind CSS (base stack configuration).
- Recharts for high-density historical yield, token variance, and automated risk analysis graphs.
- React Query for fast data fetching, automatic background polling, and cache synchronization.
- React Router for smooth single-page page navigation workflows.
- Viem for secure on-chain data verification, address validation metrics, and ENS name resolution.

## Edge Cases and Error States

- Invalid Protocol/Wallet Address entered: display immediate inline text message "Invalid 0x address format" without triggering fatal component crashes.
- Execution Slippage Reversal: display persistent workspace notification stating "Transaction reverted due to front-running protection metrics" while maintaining background operational health.
- Oracle Feed Timeout: trigger top banner stating "Primary data feed unresponsive; falling back to secondary data cache structures" without interrupting standard rendering.
- Zero Active Assets Managed: show an actionable empty state reading "Sentinel operational but no active positions found. Deposit capital or map a new threshold configuration to begin defense."

## Error Boundaries

- Wrap each individual layout card block (Telemetry, Core Settings Feed, Analytics Charts) into standalone React error boundary wrappers.
- Broken pipeline modules fail gracefully isolated inside their own card containers, displaying a minimal "Thread Interrupted — click to reload" warning banner with a localized retry action.
