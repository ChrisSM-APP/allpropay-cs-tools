# AllProPay CS Tools

Internal customer-success tools for the AllProPay team. Single-file static HTML, served via GitHub Pages.

## Tools

- **CS Call Guide** (`index.html`) — Step-by-step script for check-in calls. Walks the rep through opening, check-in, Google review ask, referral ask, and warm close. Adapts based on whether the client is available, lukewarm, or has a problem.

## Live URL

After GitHub Pages enables, this will be served at:
**https://chrissm-app.github.io/allpropay-cs-tools/**

## Configuration

Edit constants at the top of the `<script>` block in `index.html`:

- `REP_LIST` — names that appear in the rep dropdown
- `REVIEW_URL` / `REVIEW_TEXT` — the Google review link and its display label
- `LOG_ENDPOINT` — Apps Script web app URL that receives call logs (see `CS-Call-Log-Setup.md`)
