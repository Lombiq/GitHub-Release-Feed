# Project notes for future sessions

- Purpose: Single-page `index.html` that fetches GitHub releases for an organization via GraphQL and shows a flattened feed for quick scanning and one-click opening; built as a self-contained file.
- Data flow: Uses GitHub GraphQL (`https://api.github.com/graphql`) with a bearer PAT. Fetches org repositories paginated (100 per page) ordered by `PUSHED_AT` DESC. Then fetches releases per repo paginated (`first: 50` per page) ordered by `CREATED_AT` DESC, collecting all pages. Release fetching is parallelized with up to 8 concurrent workers. Results are flattened and sorted newest-first by publishedAt.
- UI/filters: Org + PAT inputs, max items, include mode (published/prerelease/drafts), search, since (defaults to last 30 days), sort (published date/repo). No persistence; token kept in memory only.
- Actions: Cards now only have the “Open release” primary link; snippet generation and copy-link were removed per refactor.
- Notes/limits: Query now includes both public and private repos (no privacy filter). Releases are fully paginated per repo (50 per page) but still limited by GitHub GraphQL permissions and rate limits.
