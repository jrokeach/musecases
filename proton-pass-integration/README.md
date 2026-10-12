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
- If the token expires or stops working, renew it. Don't work around it.
