# Proton Pass integration

Give your Muse its own scoped corner of Proton Pass: a dedicated vault plus an agent token, so it can create logins, generate strong passwords, and spin up email aliases for new accounts, without ever seeing the rest of your passwords.

## Why this instead of sharing your password

- **Scoped:** the agent token only sees the vault you choose (e.g. "Muse"). Your other vaults stay invisible.
- **Auditable:** Proton's agent feature includes monitoring, so you can see what the agent did.
- **Revocable:** delete the agent or let the token expire and access ends.
- **Your master password never leaves your hands:** you create the token from your own logged-in Proton session and hand over only the token.

## What you need

- A Proton Pass account and Proton's official `pass-cli` installed on the machine your Muse runs on.
- A computer with your own authenticated Proton session, for the one step only you can do: creating the agent token. (The vault itself can be created in the Proton Pass mobile app; the agent token is CLI-only.)
- Note: hide-my-email alias limits depend on your Proton plan. Check your plan if you expect to create many aliases.

## Files

- `setup-prompt.md` — paste into your Muse to set this up step by step.

## Standing rules worth keeping

Once the integration is live, these rules keep it safe and useful:

- The Muse vault is the only place your assistant may create or read items. Never your other vaults.
- Passwords are generated, never chosen or typed. Nobody reads them back, not even you, unless you're rotating one.
- Every new account gets its own unique email alias.
- Ask which email address(es) ("mailboxes") aliases should forward to. The user may choose one or several. Never assume a default or proceed on silence; get an explicit answer first. All mailboxes must be set up in Proton Pass _before_ creating aliases.
- Always set `PROTON_PASS_AGENT_REASON` when creating, viewing, updating, or trashing items, and verify the session (`pass-cli info`) before running commands.
- Sessions are short-lived by Proton's design: the on-disk session survives machine restarts, but the server-side session is invalidated after a few days (observed: about 3 days in Oct 2026). Don't try to preserve a session indefinitely; plan for re-login instead. The agent token itself lasts until its own expiry (e.g. 1 year), so re-login just needs the token again.
- Hand the token over via a Proton Pass secure link, not chat: create a secure link containing the token and share the link. The assistant opens it in a browser, reveals the token, and uses it transiently without ever storing it. The assistant remembers the LINK (not the token) and pulls a fresh copy from it whenever the session needs re-authentication. Set secure links to expire in ~30 days; they are view-limited, so re-pull sparingly and share a fresh link when one runs out.
- If the token expires or stops working, renew it (`pass-cli agent renew Muse --expiration 1y`) and share a fresh secure link. Don't work around it.
- Known CLI gap (verified on pass-cli v2.4.2): the CLI cannot view or change an alias's mailboxes. `item view` returns no alias details, and `item update --field mailboxes=...` just creates a useless custom field instead of changing the real mailbox mapping (tested on a throwaway alias). The docs list Mailboxes as an alias item property, but the CLI doesn't implement it. Mailbox changes need the Proton Pass app/extension or the SimpleLogin API.
