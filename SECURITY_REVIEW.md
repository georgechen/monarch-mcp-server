# Security review

Review date: 2026-08-19

Reviewed upstream: `https://github.com/robcerda/monarch-mcp-server`

Downloaded commit: `ca6c1598c99a0ff1f7b7effbb2c9f518224eab44`

## Result

The source did not show shell execution, dynamic code evaluation, hidden services, or unexpected outbound endpoints. The project talks to Monarch's API through `monarchmoneycommunity`. That dependency also contains optional upload endpoints for Monarch and Cloudinary. The MCP server runs over standard input and output and does not open a network listener in its normal entry point.

The upstream lockfile had 29 published vulnerabilities across 13 packages when scanned. The local copy updates the dependency lock and requires MCP `>=1.28.1,<2.0.0`. MCP 2.0 is excluded because it removed the import path used by this code. The updated environment passed its tests and dependency scan.

Validation completed:

- `pytest`: 222 passed
- `pip-audit`: no known vulnerabilities found in installed third-party packages
- `bandit`: no medium- or high-severity findings; the sole low-severity swallowed keyring deletion error was changed to a warning log
- source review: credential storage, authentication, network targets, file writes, deletion paths, and Monarch mutation tools checked
- live MCP check: 49 tools discovered and a read-only `get_accounts` call completed successfully

## Risks and limits

This is an unofficial, alpha-stage integration that depends on an undocumented community API client. A clean scan does not prove the software is harmless or that future Monarch API changes will preserve behavior.

The server exposes destructive and high-impact tools. Transaction deletion, bulk edits, budget changes, rule changes, merchant changes, and balance-history uploads can alter real account data. Require manual approval for these tools in the MCP client.

The server imports text from merchant names, transaction notes, and other account fields. Treat that data as untrusted content because it can contain instructions aimed at an AI client.

macOS Keychain is the preferred credential store. If Keychain access fails, the complete token or browser cookie set is written to `~/.monarch-mcp-server/token` with mode `600`. Confirm Keychain use before authenticating. Browser cookies grant broad account access and should be handled like a password.

Authentication was completed on 2026-08-19 with the user's existing Chrome session after explicit approval. The session is stored in macOS Keychain. A live read-only `get_accounts` call verified the MCP connection; its account data was not printed or written to project files. No Monarch write operation was run.

## Safe first-use rules

1. Keep approval prompts enabled for every write tool.
2. Authenticate locally with `uv run python login_setup.py` or a browser cookie captured directly from the user's logged-in Chrome session.
3. Confirm the log says it is using the system keyring before saving a session.
4. Begin with `check_auth_status`, `get_accounts`, and other read operations.
5. Back up or export important Monarch data before testing any write operation.
