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
- **`phishing-by-tld.csv`**: `snapshot_date, tld, listed_domains, zone_domains, listed_per_100k_zone_domains`. Domains listed in public blocklists per extension; `zone_domains` is the extension's size when its zone is known (0 otherwise, then no rate).

## Limits

See the README and <https://whoami.xj1.fr/research/#hosting>: CDN attribution, domains rather than traffic, missing .com/.net zones, popularity-list drift, phishing listings not verified by Whoami.
