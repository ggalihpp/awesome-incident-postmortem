# Awesome Incident Postmortems [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of incident postmortem documents from major technology companies and service providers.

This repository aims to preserve and organize public incident postmortem reports, providing valuable insights into how major tech companies handle outages, security incidents, and system failures. All documents are rewritten in markdown format to ensure long-term accessibility and preservation.

## Contents

- [What is this?](#what-is-this)
- [Why collect these?](#why-collect-these)
- [Structure](#structure)
- [Contributing](#contributing)
- [Incidents by Provider](#incidents-by-provider)

## What is this?

This collection focuses on **public incident postmortem reports** from major technology companies and service providers. These documents provide valuable insights into:

- Root cause analysis methodologies
- Incident response procedures
- System architecture patterns and failure modes
- Communication strategies during incidents
- Prevention and mitigation techniques

## Why collect these?

- **Learning**: Understanding how large-scale systems fail and recover
- **Preservation**: Ensuring these valuable documents remain accessible
- **Reference**: Having a centralized location for incident response best practices
- **Transparency**: Celebrating companies that share their learnings publicly

## Structure

```
source/
├── aws/                 # Amazon Web Services
├── google/              # Google Cloud Platform & Services
├── microsoft/           # Microsoft Azure & Services
├── cloudflare/          # Cloudflare
├── github/              # GitHub
├── slack/               # Slack
├── facebook/            # Meta/Facebook
├── netflix/             # Netflix
├── stripe/              # Stripe
└── others/              # Other providers
```

Each provider folder contains:

- Individual incident postmortem documents in markdown format
- Original links and metadata
- A README.md with the incident list for that provider

## Contributing

Contributions are welcome! Please:

1. Add the original source URL in the document metadata
2. Rewrite content in clear, well-formatted markdown
3. Use the naming convention: `YYYY-MM-DD-brief-description.md`
4. Update the provider's README.md with the new incident entry
5. Follow the existing document structure and formatting

## Incidents by Provider

### Amazon Web Services (AWS)

- [2025-10-19] [Summary of the Amazon DynamoDB Service Disruption in Northern Virginia (US-EAST-1) Region](source/aws/2025-10-19-dynamodb-us-east-1-disruption.md)

### Cloudflare

- [2022-06-21] [Cloudflare Outage on June 21, 2022](source/cloudflare/2022-06-21-global-outage-bgp-policy.md)

### GitHub

- [2018-10-21] [October 21, 2018 GitHub Database Incident](source/github/2018-10-21-database-network-partition.md)

### Google

_No incidents documented yet. Contributions welcome!_

### Microsoft

_No incidents documented yet. Contributions welcome!_

### Slack

_No incidents documented yet. Contributions welcome!_

### Meta/Facebook

_No incidents documented yet. Contributions welcome!_

### Netflix

_No incidents documented yet. Contributions welcome!_

### Stripe

_No incidents documented yet. Contributions welcome!_

### Others

_No incidents documented yet. Contributions welcome!_

---

## License

This project is licensed under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/). The content of individual incident reports remains under their original licenses as specified by the source companies.

## Disclaimer

This repository is not affiliated with any of the companies mentioned. All incident reports are publicly available documents that have been reformatted for better accessibility and preservation.
