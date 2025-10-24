---
layout: provider
title: "AWS Incident Postmortems"
provider: "aws"
---

# AWS Incident Postmortems

This directory contains incident postmortem reports from Amazon Web Services (AWS).

## Incidents

### 2024

- **[2025-10-19]** [Summary of the Amazon DynamoDB Service Disruption in Northern Virginia (US-EAST-1) Region](2025-10-19-dynamodb-us-east-1-disruption.md)
  - **Duration**: ~14.5 hours
  - **Services Affected**: DynamoDB, EC2, Network Load Balancer, Lambda, ECS/EKS/Fargate, Connect, STS, Redshift
  - **Root Cause**: Race condition in DynamoDB DNS management system
  - **Original Source**: [AWS Message 101925](https://aws.amazon.com/message/101925/)

## About AWS Incident Reports

AWS publishes detailed incident reports as part of their transparency commitment to customers. These reports typically include:

- Detailed timeline of events
- Technical root cause analysis
- Impact assessment on services
- Remediation steps taken
- Preventive measures implemented

## Additional Resources

- [AWS Post-Event Summaries](https://aws.amazon.com/premiumsupport/technology/pes/)
- [AWS Service Health Dashboard](https://status.aws.amazon.com/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
