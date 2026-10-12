# musecases

Shared Muse use cases: copy-paste setup prompts and playbooks. Each folder is one self-contained use case — paste its prompt into your Muse to set it up for yourself.

## Key architectural strategy: data segmentation with dual-delivery aliases

The pattern everything else rests on: every merchant gets its own email alias, and each alias delivers to two mailboxes — yours and your assistant's. Merchants never learn a real address, the blast radius of any leak is exactly one merchant, and your assistant can act on order email without ever seeing your personal inbox. Read the full write-up in [Dual-delivery aliases](dual-delivery-aliases/), and set up the scoped vault and alias creation first with [Proton Pass integration](proton-pass-integration/).

## Use cases

- [Credit card tracking](credit-card-tracking/) — cloud-tracked credit card reference (`credit_cards.md`, with time-limited benefits tracked inline per card), refreshed monthly, used to answer "which card should I use?" and "what benefits do I have left?"
- [Proton Pass integration](proton-pass-integration/) — scoped Proton Pass agent setup: a dedicated vault plus an agent token so your Muse can create logins, generate passwords, and spin up email aliases without ever seeing your other vaults
- [Dual-delivery aliases](dual-delivery-aliases/) — one unique email alias per merchant, delivered to both your mailbox and your assistant's: the data-segmentation strategy that keeps merchants away from real addresses while letting your assistant work from order email
