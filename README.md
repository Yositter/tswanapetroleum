# tswanapetroleum

A website for Tswana Petroleum Company, a petroleum products distributor supplying
diesel, petrol, Jet A-1, illuminating paraffin and lubricants across the SADC region.

## Structure

Static, dependency-free HTML. The homepage is self-contained; content pages share
`styles.css` and `site.js`.

| File | Purpose |
|---|---|
| `index.html` | Homepage: hero, corridor map, products, sectors, delivery custody, pricing, responsibility, procurement, quote form. |
| `csr.html` | Corporate social responsibility: pillars, commitments, metrics, governance. |
| `code-of-conduct-employees.html` | Employee Code of Conduct. |
| `code-of-conduct-suppliers.html` | Supplier Code of Conduct. |
| `chairmans-message.html` | Chairman's message. |
| `investors.html` | Investor case, indicators and reporting. |
| `apply-for-credit.html` | Trade credit account application with POPIA consent. |
| `styles.css` | Shared design tokens, header, footer and content-page components. |
| `site.js` | Shared header and mobile-menu behaviour. |

## Notes

- Forms confirm in the browser only. Wire the serverless endpoint and in-region email
  from `obsidian-vault/Technology Stack.md` before going live.
- Leadership, investor and credit pages carry illustrative details. Confirm names,
  figures and dates before publication.
