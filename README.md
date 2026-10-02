# whoami-internet-research

Open datasets and methodology from **Whoami Internet Research**: hosting, DNS, email infrastructure and phishing statistics derived from 90M+ domains.

> **Canonical source and latest datasets: https://whoami.xj1.fr/research/**
> This repository is a transparent, versioned mirror. The live page always has the most recent figures.

## What it measures

[whoami.xj1.fr](https://whoami.xj1.fr/) resolves tens of millions of domain names and records, for each one, the addresses it points to, the network and company that host it, its DNS and email providers, and its email security records (DMARC, SPF). The barometers built on it answer questions like *who hosts the web?*, *who runs the world's DNS and email?* and *how many popular sites protect their domain against spoofing?*

- **Domains analysed:** 90M+ (zone files, popularity lists, Certificate Transparency)
- **Sources:** ICANN CZDS, AFNIC (.fr), Internetstiftelsen (.se, .nu), Estonian Internet Foundation (.ee), [Tranco](https://tranco-list.eu/) top 1M, [Chrome UX Report](https://developer.chrome.com/docs/crux) by country, Certificate Transparency logs, public phishing blocklists; network ownership from GeoLite2 (MaxMind)
- **Aggregates only.** No zone file and no per-domain record is published here.

## Datasets

| File | Content | Live statistics |
|---|---|---|
| [`data/hosting-segments.csv`](data/hosting-segments.csv) | Hosting providers by segment (top 1M, top 10K, per country, per extension) | [/hosting/](https://whoami.xj1.fr/hosting/) |
| [`data/hosting-countries.csv`](data/hosting-countries.csv) | Main hosts for the sites most visited from each country | [/hosting/countries](https://whoami.xj1.fr/hosting/countries) |
| [`data/dns-providers.csv`](data/dns-providers.csv) | DNS provider market share, top 100K websites | [/dns/](https://whoami.xj1.fr/dns/) |
| [`data/mail-providers.csv`](data/mail-providers.csv) | Email provider market share, top 100K websites | [/mail/](https://whoami.xj1.fr/mail/) |
| [`data/email-security.csv`](data/email-security.csv) | DMARC and SPF adoption, top 100K websites | [/email-security/](https://whoami.xj1.fr/email-security/) |
| [`data/phishing-by-tld.csv`](data/phishing-by-tld.csv) | Domains listed in public phishing blocklists, per extension | [/phishing/](https://whoami.xj1.fr/phishing/) |

CSV, UTF-8, comma separated, one header row. Common columns: `snapshot_date`, `sample_size` (the base the percentages are computed on), `provider`, `domains`, `share_percent`. Field definitions per file are in [`docs/methodology.md`](docs/methodology.md).

<!-- SNAPSHOT:START -->
## Latest snapshot

| File | `snapshot_date` | Rows |
|---|---|---|
| `data/hosting-segments.csv` | 2026-10-02 | 13,819 |
| `data/hosting-countries.csv` | 2026-10-02 | 1,025 |
| `data/dns-providers.csv` | 2026-10-01 | 1,253 |
| `data/mail-providers.csv` | 2026-10-01 | 443 |
| `data/email-security.csv` | 2026-10-01 | 32 |
| `data/phishing-by-tld.csv` | 2026-10-02 | 60 |
<!-- SNAPSHOT:END -->

## Main limits

- **Hosted by** = the company whose network announces the IP a name resolves to. Behind a CDN (Cloudflare, Fastly, Akamai…), that is the CDN, not the origin server.
- Counts are **domains, not sites or traffic**: parked and dormant domains count in the extension segments. Popularity segments are closer to real use.
- Extensions whose zone is not public (.com and .net are not yet included) are only seen through Certificate Transparency.
- Popularity lists change over time: a share can move because the list moved.
- Phishing figures are **what public blocklists say**, not verified by us, and reflect an extension's size as much as its abuse handling.

Full method and limits: <https://whoami.xj1.fr/research/>.

## Licence

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE): use, share and adapt, including commercially, with attribution. Third-party data keeps its own terms (GeoLite2 by MaxMind; Chrome UX Report, CC BY-SA 4.0; Tranco).

## How to cite

> Whoami Internet Research, whoami.xj1.fr. Dataset snapshot: *date from the `snapshot_date` column*. https://whoami.xj1.fr/research/

GitHub's "Cite this repository" button uses [`CITATION.cff`](CITATION.cff). For specific statistics, please cite the corresponding dataset.

## Updates

The files are refreshed weekly from <https://whoami.xj1.fr/research/data/>; each change is a dated commit, which gives the history of the datasets. URLs on whoami.xj1.fr never carry a date.
