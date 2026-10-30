# DuneSQL Solana Top Holders

A parameterized [Dune](https://dune.com) query that lists the top holders of any SPL token on Solana, along with each wallet's share of supply, USD value, and quick links to block explorers.

## What it does

Given a token mint address, the query:

1. Fetches the most recent hourly price Dune has recorded for the token (`prices_solana.hour`).
2. Sums the current balance of every owner of that token (`solana_utils.latest_balances`).
3. Filters out wallets whose holdings are worth less than a minimum USD value.
4. Returns the remaining holders sorted by balance, largest first.

## Output columns

| Column | Description |
| --- | --- |
| `token_balance_owner` | Wallet address that owns the tokens |
| `Total_Percntg` | Share of total supply held, as a fraction (e.g. `0.05` = 5%) |
| `total_balance` | Total token balance held by the wallet |
| `Value_in_usdt` | Holding value in USD, rounded up, based on the latest Dune price |
| `solfm_link` | Link to the wallet on [SolanaFM](https://solana.fm) |
| `solscan_link` | Link to the wallet on [Solscan](https://solscan.io) |

## Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `{{contract_address}}` | Text | The token's mint address |
| `{{total_tokens}}` | Number | Total token supply, used to compute `Total_Percntg` |
| `{{min_token_holding_in_usdt}}` | Number | Minimum USD value a wallet must hold to be included |

## Usage

1. Create a new query on [dune.com](https://dune.com) using the DuneSQL engine.
2. Paste the contents of [`dunesql-solana-top-holders-scanner-auto.txt`](dunesql-solana-top-holders-scanner-auto.txt) into the editor.
3. Dune will detect the `{{...}}` placeholders and create parameter widgets. Fill in the mint address, total supply, and minimum USD threshold.
4. Run the query. To make the explorer links clickable, display the results in a table visualization.

## Notes

- The token must have price data in `prices_solana.hour`; otherwise `Value_in_usdt` is null and the `HAVING` filter drops every row.
- The price is the latest hourly price Dune has recorded, so it may lag the live market price.
- `total_tokens` is supplied manually. If it doesn't match the real circulating or total supply, `Total_Percntg` will be off.

## License

[MIT](LICENSE)
