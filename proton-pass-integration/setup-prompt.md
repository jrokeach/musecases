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
- The agent token must be created from MY authenticated Proton session on a computer. It is CLI-only; the mobile app does not expose it.
- First, log in to the CLI (`pass-cli login`), since the agent command requires an authenticated client.
- Make sure the "Muse" vault from step 2 already exists, since the agent is scoped to it.
- Then I will run:
  `pass-cli agent create Muse --expiration 1y --vault "Muse"`
  (Check `pass-cli agent --help` for the current expiration options and exact flags, since the CLI evolves.)
- The new agent may only get Viewer access by default, which is not enough to create logins or aliases. Grant it Editor access:
  `pass-cli agent access grant Muse --vault-name "Muse" --role editor`
- I will hand you the FULL output of that command. It includes the token plus Proton's usage instructions for the agent. Read them and follow them (step 4).
- Preferred handoff: share the token via a Proton Pass secure link instead of pasting it in chat. Create a secure link containing the agent token (set it to expire in ~30 days) and give me the link. I will open it in a browser, reveal the token, and use it transiently to log in, without storing the token anywhere: never in chat, notes, or memory files. I will remember the LINK (not the token) so I can pull a fresh copy from it the next time the session needs re-authentication. Note that secure links are view-limited, so I will re-pull sparingly; when the link expires or runs out of views, I will ask you for a fresh one. (If you prefer to paste the token in chat, I will still use it transiently and store nothing.)
- If the agent subcommand is unavailable in my installed version, fall back to a personal access token (`pass-cli login --pat`) and tell me the tradeoff: broader access, so prefer the agent token when available.

## 4. Establish the session and follow Proton's agent instructions (you do this part)
- Isolate the session from any other Proton sessions on the machine, using a path under your home directory so it survives restarts (do not use /tmp, which may be wiped):
  `export PROTON_PASS_SESSION_DIR="$HOME/.pass-agent-muse"`
- Export `PROTON_PASS_SESSION_DIR` before EVERY `pass-cli` invocation in every new shell. Without it, the CLI falls back to its default directory, where it will report no session and may force-logout stale state there.
- Log in with the token I gave you:
  `PROTON_PASS_PERSONAL_ACCESS_TOKEN="<token>" pass-cli login`
  then verify with `pass-cli info`.
- Do not save the token anywhere retrievable (never in chat, notes, or memory files). If you were given a secure link, remember the LINK, not the token; you will pull a fresh copy from it each time you need to re-authenticate.
- Expect the session to die every few days: Proton invalidates the server-side session after a short time (observed: about 3 days), even though the on-disk session files survive restarts and the agent token itself remains valid until its own expiry. This is normal. When it happens, get the token again (fresh pull from the secure link, or ask me to renew/share it) and repeat the login step above.
- Confirm your access: run `pass-cli vault list` and `pass-cli share list`, and report back the vaults you can see. The "Muse" vault must be among them.
- Session hygiene, every time:
  - Before running ANY pass-cli command, verify the session with `pass-cli info`. Re-authenticate immediately on auth errors or non-zero exit codes. Sessions can expire mid-task, so check periodically during long runs.
  - If a command fails with an authentication error: run `pass-cli logout --force`, re-run the login command above, verify with `pass-cli info`, then retry the original command.
  - If you see "Local encryption key not found but local data exists", the CLI will force-logout and clear the stale local state. This is expected when a previous session died. Let it finish, then re-run the login command, verify with `pass-cli info`, and retry.
  - If ANY command fails, read the entire output before retrying. Diagnose from the error message and exit code; don't guess.
- Access rules:
  - `item view`, `item create` (including `item create login`), `item update`, `item trash`, `item untrash`, and `vault update` REQUIRE the `PROTON_PASS_AGENT_REASON` environment variable, set to a brief description of why you are accessing the item or field. Example:
    `PROTON_PASS_AGENT_REASON="Creating login for new Acme account" pass-cli item view --vault-name "Muse" --item-title "Acme"`
  - Items can also be addressed directly with a `pass://SHARE_ID/ITEM_ID` URI, and single fields retrieved with `--field` (e.g. `--field password`).
  - Use `--output json` whenever you need to parse output programmatically (`vault list`, `share list`, `item list`).
- `pass-cli agent instructions` reprints Proton's instructions any time you need a refresher. `pass-cli info` and `pass-cli test` check session and connection health. Full CLI docs: <https://protonpass.github.io/pass-cli/>

## 5. Verify with a test
- Create a test login item with a generated password in the Muse vault, and a test email alias in the Muse vault. (Check `pass-cli item --help` for exact syntax; confirmed subcommands include `item create login` and `item alias create`. Remember `PROTON_PASS_AGENT_REASON` is required for `item create`.)
- Show me the alias and the login's title (never the password), then delete the test items unless I say to keep them.

## 6. Set the standing rules
From now on, without being asked each time:
- Whenever I ask you to create an account somewhere: generate a strong password, save it as a login item in the Muse vault, create a unique email alias for that account in the Muse vault, and tell me the alias you used. Never tell me the password.
- Ask me which email address(es) ("mailboxes") the aliases you create should forward to. I may choose ONE or SEVERAL, for example one of yours and one of mine. Do not assume a default and do not proceed on silence: get my explicit answer before creating any alias. All chosen mailboxes must be set up in Proton Pass _before_ you start creating aliases to forward to them.
- Never read a password back to me or anyone else. The vault is the system of record.
- Never create, read, or modify items outside the Muse vault.
- Always set `PROTON_PASS_AGENT_REASON` when creating, viewing, updating, or trashing items, and verify the session with `pass-cli info` before running commands (see step 4).
- If the agent token expires or stops working, tell me plainly and ask me to renew it (`pass-cli agent renew Muse --expiration 1y`) and share a fresh secure link. Do not work around it with another auth method on your own.

## 7. Ongoing
- If I report that an alias or login isn't working, check the item in the vault and help me fix or rotate it.
- Warn me before the token expires so we can renew it without a gap. Note the expiration date where you'll see it. Also warn me before the secure link expires (~30 days), since you can only re-pull the token while the link is live.
