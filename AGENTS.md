# GlobalStack docs — agent and contributor guide

This repo is the Mintlify site served at https://docs.globalstack.io.

- Pages are MDX files with YAML frontmatter. Navigation, redirects and theme live in `docs.json`.
- Preview locally with `mint dev` (Mintlify needs Node 24 or lower; it refuses Node 25+).

## Where things live

| Folder | What goes there | Menu group |
| --- | --- | --- |
| `how-it-works.mdx`, `quickstart.mdx` | The concepts overview and the first-steps guide | Get Started |
| `guides/` | Task-oriented walkthroughs: virtual accounts, collecting and paying out, KYC, beneficiaries, auto-conversion | Guides |
| `essentials/` | Things every integration needs: auth, idempotency, pagination, rate limits, currency access | Essentials |
| `errors.mdx` | Every request-level error code. **Stays at `/errors`**: the API links to its anchors in `meta.next_steps.docs_url`. | Essentials |
| `webhooks/` | Event catalogue and signature verification. **`/webhooks/events` is linked from the API.** | Webhooks |
| `reference/` | Lookup tables: failure reasons, customer restrictions | Reference |
| `changelog.mdx` | Dated `<Update>` entries for anything that changes an integration | Changelog |
| `api-reference/openapi.json` | The API reference. **Don't edit it here.** | API Reference |

When you move or rename a page, add a redirect in `docs.json`. The API returns some doc URLs
(`/errors#…`, `/webhooks/events`, `/reference/customer-restrictions` via a redirect from
`/api-reference/customer-restrictions`, `/guides/virtual-accounts`), so those must always resolve.

## The API reference comes from sentinel

`api-reference/openapi.json` is generated in the `sentinel` repo from its controllers and the
markdown in `src/docs/api/` (`yarn openapi:export-public` writes `src/public-openapi.json`), then
synced here. To change any endpoint description, field description or the API overview, edit
sentinel and regenerate. Edits made here are overwritten on the next sync.

## Terminology

- **Virtual account**: a fiat **wallet** plus a **bank credential** on it. The API has no
  "virtual account" resource. Always explain it in those terms, because partners search for
  "virtual account" and won't find the words wallet or credential.
- **Wallet**: a balance in one currency, owned by a customer. **Credential**: a way to pay into a
  wallet. **Beneficiary**: a place to pay out to.
- **Transfer**: same-currency payout, fiat or crypto. **Onramp**: local currency into stablecoins.
  **Offramp**: stablecoins out to local currency. Don't describe transfers as crypto-only.
- **Customer**: the person or business you act for. **Default customer**: the merchant's own
  customer, used when `customer_id` is omitted.

## Content rules

- **Never name upstream providers or internal services** in published pages: no Busha, VALR,
  Yellowcard, Fincra, OpenPayd, Paystack (as a rail) or "FX Engine". Describe behaviour per currency
  instead. Bank names a payer or beneficiary actually sees (Absa, Nedbank, Wema) are fine. Check
  before opening a PR:

  ```bash
  grep -rniE "busha|valr|yellow ?card|fincra|openpayd|paystack|fx ?engine" --include='*.mdx' .
  ```

- Mark features that aren't live yet as **Coming soon**. Don't document them as available.
- Verify claims against the code (sentinel, fx-engine) before publishing. Field names in examples
  must match real responses. For example, credential `details` keys are camelCase.
- Internal links use page paths (`/api-reference/transfers/execute-a-transfer`), never
  `/api-reference/openapi#tag/...` anchors: Mintlify doesn't resolve those.

## Style

- Active voice, second person ("you"). One idea per sentence.
- Sentence-case headings.
- Code formatting for fields, endpoints, values and file names. Bold for UI elements.
- Prefer a table for anything compared across currencies, statuses or fields.
