---
layout: provider
title: "GitHub Incident Postmortems"
provider: "github"
---

# GitHub Incident Postmortems

This directory contains incident postmortem reports from GitHub.

## Incidents

### 2018

- **[2018-10-21]** [October 21, 2018 GitHub Database Incident](2018-10-21-database-network-partition.md)
  - **Duration**: 24 hours and 11 minutes
  - **Services Affected**: GitHub.com, Webhooks, GitHub Pages, Authentication
  - **Root Cause**: Network partition leading to database failover with data inconsistencies
  - **Original Source**: [GitHub Blog](https://github.blog/2018-10-30-oct21-post-incident-analysis/)

## About GitHub Incident Reports

GitHub publishes comprehensive incident analyses that focus on:

- Detailed technical architecture explanations
- Decision-making processes during incidents
- Trade-offs between availability and data integrity
- Long-term architectural improvements
- Organizational learning and process changes

## Additional Resources

- [GitHub Blog](https://github.blog/) - Company news and incident reports
- [GitHub Status](https://www.githubstatus.com/)
- [GitHub Availability Reports](https://github.blog/news-insights/company-news/) - Monthly availability summaries
