# Project overview

## Purpose

This project exposes Monarch Money data and actions through the Model Context Protocol. An MCP client can ask the server to retrieve financial data or perform supported Monarch account changes.

## Intended users

The intended user is an individual Monarch Money customer who runs an MCP client locally and understands that some tools can change or delete financial records. It is also useful to developers testing personal-finance workflows against a mocked Monarch client.

## Functionality and features

The server supports account lists, holdings, balance history, account refreshes, transactions, transaction search, categories, tags, transaction review, recurring transactions, merchant settings, transaction rules, splits, budgets, cash flow, spending summaries, and net-worth history.

It also exposes write operations. These include creating, editing, categorizing, splitting, and deleting transactions; changing budgets; uploading balance history; editing merchants; reviewing recurring streams; and creating, editing, or deleting transaction rules and tags.

Authentication can use a browser session cookie, Monarch email and password with verification, or a legacy token. The resulting session is stored in macOS Keychain when available. A local owner-only file is used as a fallback.

## Use cases

1. Ask an MCP client for a categorized spending summary for a selected date range, then review the returned totals without changing Monarch data.
2. Find transactions that need review, preview a proposed bulk category change, and approve the write only after checking the transaction IDs and target category.
3. Compare budgeted and actual spending for the current month, then adjust one category after confirming the amount and effective month.

## Operating boundary

Keep client approval enabled for every write operation. Start with read-only queries. Never send a password, MFA code, session token, or browser cookie through an AI chat message.
