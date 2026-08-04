# Proposal to fund Glyph wallet and dApp ecosystem for four months

## Proposal

Send 20.88 billion QUBIC (approximately 9,500 USD at $455 per billion) to `PJWENKHXJMPMPFNHFVJADDUWJHTADMHKMVKCIRHJXEBARJOFZXUHATGELXDJ` to fund development of the Glyph wallet, the connect SDK, and the relay service from August through November 2025.

> Option 0: no

> Option 1: yes, 20.88 billion QUBIC


## What Glyph is

Glyph is an independent organization building open-source software for the Qubic network. Its main product is Glyph Wallet, a desktop wallet that runs on Windows, macOS, and Linux from a single Tauri + React codebase. The wallet handles vault creation, seed import, encrypted seed storage, multi-account wallets, transaction history, balance tracking, and contact management.

The ecosystem also includes three other open-source components:

- @glyph-oss/connect is an npm package that lets web dApps request transaction signing from Glyph Wallet. It supports deep-link requests via the glyph:// protocol and a relay-based SSE transport for web dApps that need to wait for a response without a browser extension.
- Glyph Relay is a serverless relay that brokers the connection between a dApp and the wallet. It receives signed results from the wallet and streams them to the dApp. It is deployed at relay.glyphq.org.
- A starter dApp that shows a working integration of @glyph-oss/connect, intended as a copy-paste starting point for other developers.

All repositories are under the glyphq GitHub organization. Connect is MIT-licensed. The wallet uses a custom source-available license.


## What has been built so far

All of the above was built without CCF funding. The current state of the wallet:

- Create and import wallets from seed phrases
- Lock/unlock with passphrase-protected seed storage (Tauri secure store)
- Multi-vault and multi-account support with active-account switching
- Transaction send and receive with tick-based confirmation tracking
- Transaction history with filtering by direction, contract type, and tick range
- Balance display with 24h price change from stored price snapshots
- Contact management with identity-based address book
- dApp request approval: the wallet receives a structured intent from the connect SDK, displays it to the user, and returns a signed result or rejection through the relay
- Search across accounts, contacts, transactions, and known contracts
- Settings for appearance (font selection, dark/light theme), security (auto-lock timer), and network

Both the connect SDK and the wallet compile and build cleanly. The relay is deployed and functional, with a Durable Object per nonce, 5-minute TTL, max 5 SSE subscribers per nonce, and result persistence through DO storage.


## What the funding covers

Four months of one full-time developer. The goal: ship a signed, stable 1.0.0 release with a documented dApp integration path.

| Month | Focus | Deliverables |
|-------|-------|-------------|
| 1 | Wallet core | Transaction signing improvements. Auto-lock hardening. Seed memory cleanup. Code signing certificates. 1.0.0 release with auto-update. |
| 2 | dApp ecosystem | Relay rate limiting and logging. Connect SDK 2.x with typed API docs. Starter dApp deployed as live reference. |
| 3 | Features | Scheduled and recurring transfers. Batch send. Token/asset management. Staking UX improvements. Address book import/export. |
| 4 | Launch prep | External security review of seed handling and signing. Accessibility pass. i18n groundwork. Documentation site. First external dApp integrations. |


## What comes after (if scope finishes early)

If the four-month scope is completed ahead of schedule, the remaining time will go toward these next priorities (in order):

1. Glyph Explorer, a web-based view of Qubic network activity (ticks, transactions, contract state). Already in early development.
2. Glyph SDK, a coherent set of TypeScript libraries for Qubic applications (account management, transaction building, contract interaction). In development.
3. Glyph Docs, a single documentation system covering all Glyph products. In development.
4. Glyph CLI, repeatable command-line workflows for Qubic development (key generation, transaction submission). Planned.
5. Glyph API, consistent HTTP access to Qubic data for third-party integrations. Planned.
6. Glyph Trade, a QX/QSwap wrapper interface. Planned.

These are shown on the website at glyphq.org with their current status. None of them require a separate CCF request if surplus funding covers the work, but I will confirm with the community before starting each one.


## What this gives the Qubic ecosystem

A desktop wallet that works on all three major platforms from a single codebase, with a native installer and auto-update. The Qubic web wallet exists but runs in a browser and does not offer a native desktop experience with local signing isolation.

A reusable SDK for dApp-wallet communication. Any project building on Qubic can use @glyph-oss/connect to request signing from their users without building their own wallet integration. The relay handles the transport, so the dApp developer just calls a function and waits for a result.

A reference implementation (the starter dApp) that shows exactly how to wire this up, reducing the time for a new Qubic dApp developer from "reading docs and guessing" to "copying a working example."


## Cost breakdown

| Item | Monthly | Total (4 months) |
|------|---------|-------------------|
| Development (1 FTE) | $2,000 | $8,000 |
| Infrastructure (relay hosting, test VMs, code signing certs) | $200 | $800 |
| Security review (one-time, month 4) | - | $700 |
| Total | | $9,500 |

At $455 per billion QUBIC, this is 20.88 billion QUBIC.

The developer rate is low because QUBIC's market price has been low lately, which makes the dollar equivalent smaller than it would be at a higher token price. All infrastructure costs are passed through at cost.

If QUBIC appreciates during the funding period and the 20.88 billion covers more than four months of work, I will continue development into the next items on the roadmap (see "What comes after" above) with community approval. I will not spend surplus funds without a new community vote or explicit CCF approval. If the community does not approve continuation, the unspent portion goes back to the CCF.


## Governance

The identity PJWENKHXJMPMPFNHFVJADDUWJHTADMHKMVKCIRHJXEBARJOFZXUHATGELXDJ receives the funds. I will publish monthly progress updates in the Qubic Discord and as GitHub repository activity. The CCF or an independent community reviewer can request access to invoices and expenditure documentation at any time. Connect is open source (MIT) and the wallet is source available. Both can be audited without asking.

Any unspent funds from token appreciation will either continue development into the items listed above (with community approval) or be returned to the CCF.


## References

- Wallet repository: https://github.com/glyphq/wallet
- Connect SDK: https://github.com/glyphq/connect
- Relay: https://github.com/glyphq/relay
- Starter dApp: https://github.com/glyphq/starter-dapp
- Website: https://glyphq.org
- Download: https://glyphq.org/download/
