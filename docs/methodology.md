# Methodology and field definitions

Canonical version: <https://whoami.xj1.fr/research/>. Data: <https://whoami.xj1.fr/research/data/>.

## Pipeline

1. **Domain lists:** zone files (ICANN CZDS for generic extensions, AFNIC for .fr, Internetstiftelsen for .se and .nu, the Estonian Internet Foundation for .ee), the Tranco top 1M, the Chrome UX Report top sites per country, and the names found in Certificate Transparency logs for extensions whose zone is not published.
2. **Resolution:** every name is resolved by a local recursive resolver (A, AAAA, NS, MX, TXT, DMARC records). Records are refreshed about every 90 days, the top 100K weekly; new names are resolved as they appear.
3. **Network and company:** each IP is mapped to the network (ASN) that announces it with GeoLite2 (MaxMind); ASNs of one company are grouped under one name.
4. **Aggregation:** per segment (popularity band, country, extension) the domains are counted once per company. Percentages are `100 * domains / sample_size`, two decimals.

## Common fields

| Field | Meaning |
|---|---|
| `snapshot_date` | Date of the data (`YYYY-MM-DD`): the day the snapshot was computed, or the date of the source list |
| `sample_size` | Number of domains the percentages are computed on |
| `provider` | Hosting company (network grouped by company), or DNS / email provider |
| `domains` | Number of domains attributed to the row |
| `share_percent` | `100 * domains / sample_size` |

## Files

- **`hosting-segments.csv`**: `snapshot_date, segment, segment_label, sample_size, segment_domains, rank, provider, domains, share_percent`. `sample_size` = domains resolved to a known network; `segment_domains` = known domains in the segment. The 25 largest providers per segment, plus an `Others` row.
- **`hosting-countries.csv`**: `snapshot_date, country_code, country_name, sample_size, rank, provider, domains, share_percent`. The five largest hosts per country (websites most visited from it), plus Cloudflare when it is not among them.
- **`dns-providers.csv`**, **`mail-providers.csv`**: `snapshot_date, sample_size, provider, slug, domains, share_percent`. Base: top 100K websites that have NS (respectively MX) records. A domain running its own is `Self-hosted`.
- **`email-security.csv`**: `snapshot_date, breakdown, group, sample_size, dmarc_reject, dmarc_quarantine, dmarc_none, dmarc_missing, dmarc_enforced_percent, spf_hard, spf_soft, spf_neutral, spf_missing`. `breakdown` is `all`, `popularity`, `mail_provider` or `tld`. DMARC: policy (`p=`) of the `_dmarc` record. SPF: qualifier ending the SPF record (`-all` hard, `~all` soft, neutral or open).
- **`ipv6-dnssec.csv`**: `snapshot_date, breakdown, group, sample_size, ipv6_domains, ipv6_percent, dnssec_sample_size, dnssec_signed, dnssec_percent`. Base: Tranco top 100K websites that resolve. `breakdown` is `all`, `popularity`, `hosting_company` or `tld`. IPv6: the domain, or its `www` name, resolves to at least one AAAA address. DNSSEC: the parent zone publishes a DS record for the domain (presence only, signature validity is not checked); `dnssec_sample_size` is the number of domains whose DS lookup answered.
- **`technologies.csv`**: `snapshot_date, segment, category, value, sample_size, domains, percent`. `segment` is `top-100k` or `top-1m` (Tranco) or `country-<cc>` (Chrome UX Report: the most visited sites of a country, top 50,000 of each list, for countries with at least 500 answered sites). Base (`sample_size`): the segment's websites whose home page answered and was not an anti-bot page, for `web_server`, `cdn`, `security_header` and `protocol`; for `cms`, `ecommerce`, `language` and `framework` only the pages that answered with an HTTP 2xx status (error pages, APIs and CDN hosts say nothing about a site's platform). The `coverage` rows give the counts of reachable, anti-bot and 2xx domains. Within `cms`, `custom_js_framework` counts sites with a JavaScript framework and no platform marker (custom builds) and `no_marker` the rest. `category` is `cms`, `ecommerce`, `web_server`, `cdn`, `language`, `framework`, `security_header` (`hsts`, `hsts_strong` = max-age of 6 months or more, `csp`, `xfo` = X-Frame-Options or CSP frame-ancestors, `xcto` = nosniff, `refpol`) or `protocol` (`https`, `tls13`, `h2` = HTTP/2 or 3, `h3`). Method: one `GET /` per domain, every 7 days for the top 100K and every 30 days for the others, identified as `xj1-whoami-research` (<https://whoami.xj1.fr/bot>), at most three redirects, no JavaScript run, only indicators stored. Technologies visible only after JavaScript runs are not detected, and a site that hides its markers counts as not detected, so shares are minimums.
- **`phishing-by-tld.csv`**: `snapshot_date, tld, listed_domains, zone_domains, listed_per_100k_zone_domains`. Domains listed in public blocklists per extension; `zone_domains` is the extension's size when its zone is known (0 otherwise, then no rate).

## Limits

See the README and <https://whoami.xj1.fr/research/#hosting>: CDN attribution, domains rather than traffic, incomplete .com/.net coverage (no public zone file: Certificate Transparency only, since 2026-10-03), popularity-list drift, phishing listings not verified by Whoami.
