# Proton Pass integration — setup prompt

Paste everything below into your Muse:

---

Set up a Proton Pass integration so you can create logins, generate passwords, and create email aliases for me in a dedicated vault, using Proton's official agent feature. Work through these steps in order, asking me for input where noted.

## 1. Install pass-cli
- Install Proton's official `pass-cli` on your machine using Proton's install script, verifying the download per Proton's instructions.
- Confirm it works with `pass-cli --version` and note the version and the binary location (it may not be on the default PATH).

## 2. Create a dedicated vault
- Ask me to create a new vault in Proton Pass named "Muse". I can do this in the Proton Pass app.
- This vault is the ONLY place you may create or read items. Never touch my other vaults, and never ask for access to them.

## 3. Create the agent token (I do this part)
- The agent token must be created from MY authenticated Proton session on a computer. It is CLI-only; the mobile app does not expose it. I will run:
  `pass-cli agent create Muse --expiration 1y --vault "Muse"`
  (Check `pass-cli agent --help` for the current expiration options and exact flags, since the CLI evolves.)
- I will hand you the resulting token. Establish the session with it and confirm you can list the vault's items.
- If the agent subcommand is unavailable in my installed version, fall back to a personal access token (`pass-cli login --pat`) and tell me the tradeoff: broader access, so prefer the agent token when available.

## 4. Verify with a test
- Create a test login item with a generated password in the Muse vault, and a test email alias in the Muse vault. (Check `pass-cli item --help` for exact syntax; confirmed subcommands include `item create login` and `item alias create`.)
- Show me the alias and the login's title (never the password), then delete the test items unless I say to keep them.

## 5. Set the standing rules
From now on, without being asked each time:
- Whenever I ask you to create an account somewhere: generate a strong password, save it as a login item in the Muse vault, create a unique email alias for that account in the Muse vault, and tell me the alias you used. Never tell me the password.
- Ask me which email address(es) ("mailboxes") the aliases you create should forward to. It may be just one, or several (for example, one of yours and one of mine). All mailboxes must be set up in Proton Pass _before_ you start creating aliases to forward to them.
- Never read a password back to me or anyone else. The vault is the system of record.
- Never create, read, or modify items outside the Muse vault.
- If the agent token expires or stops working, tell me plainly and ask me to renew it. Do not work around it with another auth method on your own.

## 6. Ongoing
- If I report that an alias or login isn't working, check the item in the vault and help me fix or rotate it.
- Warn me before the token expires so we can renew it without a gap. Note the expiration date where you'll see it.
