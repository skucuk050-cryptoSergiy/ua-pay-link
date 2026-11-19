# ⚡ UA Pay Link

**Create a QR invoice for USDC or SOL payments in 10 seconds — in Ukrainian or English.**
No sign-up, no backend, no custody: payments go straight from the payer's wallet to yours via [Solana Pay](https://docs.solanapay.com/spec).

Built for Ukrainian freelancers, creators and small businesses who get paid in stablecoins and need a simple, trustworthy way to request and confirm a payment.

## Features

- **Invoice in seconds** — enter your wallet address, amount, currency (USDC / SOL), your name and what the payment is for.
- **Solana Pay QR + deep link** — the payer scans the QR or taps "Open in wallet" (Phantom, Solflare and other Solana Pay wallets).
- **Automatic payment confirmation** — every invoice gets a unique `reference` key; the page polls the Solana RPC and shows ✅ *Paid* only when a confirmed transaction actually delivered the full amount of the right token to your address.
- **Shareable invoice link** — the whole invoice lives in the URL, so you can send it in Telegram/Viber/email and both sides see the live payment status.
- **Ukrainian-first UI** with an English switch.
- **Local history** — your invoices are stored only in your browser (localStorage). Nothing is sent to any server.
- **Custom RPC** — paste your own RPC endpoint (e.g. Helius) if the public one is rate-limited.

## How it works

1. A random 32-byte `reference` is generated in the browser for each invoice.
2. A Solana Pay transfer request URL is built:
   `solana:<recipient>?amount=<amount>&spl-token=<USDC mint>&reference=<ref>&label=...&message=...`
3. The page calls `getSignaturesForAddress(reference)` every 5 seconds.
4. For each found signature it loads the transaction (`getTransaction`, `jsonParsed`) and checks:
   - the transaction did not fail (`meta.err == null`);
   - **SOL**: the recipient's lamport balance increased by at least the invoice amount;
   - **USDC**: the recipient's USDC token balance (mint `EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`, owner = recipient) increased by at least the invoice amount.
5. All amounts are handled as integers in base units (`BigInt`) — no floating-point rounding.

## Tech

- One static `index.html` (HTML + CSS + vanilla JS), no build step, no framework.
- QR codes: [`qrcode-generator`](https://github.com/kazuhikoarase/qrcode-generator) (MIT) via jsDelivr.
- Hosted on GitHub Pages.

## Run locally

Open `index.html` in a browser. That's it.

## Roadmap

- [ ] Telegram bot: create an invoice and get a "Paid" notification in chat
- [ ] PDF / image export of the invoice for clients
- [ ] Multi-invoice dashboard with CSV export (for bookkeeping)
- [ ] More stablecoins (USDT, EURC) and Token-2022 support
- [ ] Ukrainian-language guides: "How to get paid in USDC on Solana"

## License

MIT
