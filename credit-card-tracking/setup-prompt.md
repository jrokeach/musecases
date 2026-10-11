# Credit card tracking — setup prompt

Paste everything below into your Muse:

---

Set up a credit card tracking system for me. Work through these steps in order, asking me for input where noted.

## 1. Choose where to keep the files
- Ask me whether to use Google Drive or Dropbox (use whichever I pick; default to Google Drive if I have no preference).
- Create a folder named `Credit Card Tracking` there. This is where the reference files will live; the storage service's built-in version history serves as our version tracking, so never delete old versions.

## 2. Learn my wallet
- Ask me which credit cards I currently hold. For each, ask the exact product name and roughly when I opened it (for annual-fee timing).
- Ask if there are cards I'm considering adding.

## 3. Build the reference document
- Research each card's CURRENT earn rates, benefits, and limitations from the issuer's official pages plus reputable trackers (e.g. NerdWallet, Doctor of Credit). Note the date you checked.
- Create `credit_cards.md` in the folder as the single source of truth for my wallet, with these sections:
  - Quick-pick cheat sheet: best card per spend category (dining, groceries, gas, travel, etc.)
  - One section per card: earn rates, key benefits, limitations, annual fee
  - Rotating bonus category tables (e.g. Discover, Chase Freedom Flex) with activation deadlines and rules (e.g. whether earning is retroactive)
  - Transfer partner notes and any recent devaluations
  - Card pairing strategies (e.g. pooling points between cards from the same issuer)
  - Benefits comparison table, annual fee summary, renewal notes
  - Quarterly activation reminders and a "what NOT to do" list
- Start the file with a "Last Updated" line noting the month and what changed.
- Only include facts backed by a cited source. Where sources conflict or a fact is unconfirmed, keep a short "verify" note instead of guessing.
- Keep the file private: it shows which cards I hold. Never put account numbers or credentials in it.

## 4. Add a benefits tracker
- Create `benefits.md` in the same folder tracking every time-limited benefit across my cards: annual travel credits, travel-portal reimbursements, airline fee credits, streaming credits, hotel credits, etc.
- For each benefit record: which card, what the benefit is, the dollar amount, the reset date (annual, semi-annual, monthly), and status (unused / partially used / used).
- Ask me which of these I've already used in the current period and fill in the statuses. When I later tell you I've used a benefit, update the file.

## 5. Set up the monthly update
- Create a scheduled task that runs on the 1st of each month to:
  a. Re-research my cards' earn rates, benefits, and rotating categories from issuer pages and reputable trackers.
  b. Update `credit_cards.md` in place (including the "Last Updated" line); the storage version history keeps prior copies.
  c. Roll quarterly category content forward in January, April, July, and October.
  d. Send me a short summary of what changed that month.
- Confirm the schedule with me before you create it.

## 6. Use the docs to answer my questions
- Whenever I ask which card to use for a purchase, first pull the latest `credit_cards.md` from the folder, then answer from it: name the card, the earn rate, and the one-line reason.
- Whenever I ask what benefits I still have available (e.g. "do I have any travel credits left?"), check `benefits.md` and answer from the tracked statuses. If a benefit's status is unknown, say so and ask me.
- If I report a new card, a closed card, or a used benefit, update the relevant file.
