---
layout: incident
title: "Summary of the Amazon DynamoDB Service Disruption in Northern Virginia (US-EAST-1) Region"
incident_date: "October 19-20, 2025"
duration: "~14.5 hours (11:48 PM PDT Oct 19 - 2:20 PM PDT Oct 20)"
region: "US-East-1 (Northern Virginia)"
severity: "critical"
provider: "aws"
original_source: "https://aws.amazon.com/message/101925/"
---

# Summary of the Amazon DynamoDB Service Disruption in Northern Virginia (US-EAST-1) Region

**Incident Date**: October 19-20, 2025  
**Duration**: ~14.5 hours (11:48 PM PDT Oct 19 - 2:20 PM PDT Oct 20)  
**Region**: US-East-1 (Northern Virginia)  
**Original Source**: [AWS Message 101925](https://aws.amazon.com/message/101925/?utm_source=substack&utm_medium=email)

## Executive Summary

We experienced a service disruption in the N. Virginia (us-east-1) Region on October 19 and 20, 2025. The event lasted from 11:48 PM PDT on October 19 and ended at 2:20 PM PDT on October 20, with three distinct periods of impact:

1. **11:48 PM - 2:40 AM**: Amazon DynamoDB experienced increased API error rates
2. **5:30 AM - 2:09 PM**: Network Load Balancer (NLB) experienced increased connection errors
3. **2:25 AM - 1:50 PM**: EC2 instance launches failed and connectivity issues for new instances

## Impact Overview

### Primary Services Affected

- **Amazon DynamoDB**: DNS resolution failures preventing new connections
- **Amazon EC2**: Instance launch failures and connectivity issues for new instances
- **Network Load Balancer (NLB)**: Increased connection errors due to health check failures
- **AWS Lambda**: API errors, latencies, and processing delays
- **Amazon ECS/EKS/Fargate**: Container launch failures and cluster scaling delays
- **Amazon Connect**: Elevated errors handling calls, chats, and cases
- **AWS STS**: API errors and latency
- **AWS Management Console**: Authentication failures for IAM users
- **Amazon Redshift**: API errors and query processing issues

### Customer Impact

- Customers unable to establish new connections to DynamoDB
- New EC2 instance launches failed with "request limit exceeded" or "insufficient capacity" errors
- Existing EC2 instances remained healthy throughout the event
- Global DynamoDB tables experienced replication lag but remained accessible in other regions

## Root Cause Analysis

### DynamoDB DNS Management System

The root cause was a **latent race condition** in the DynamoDB DNS management system that resulted in an incorrect empty DNS record for the service's regional endpoint (`dynamodb.us-east-1.amazonaws.com`).

#### Architecture Overview

The DynamoDB DNS management system consists of two independent components:

1. **DNS Planner**: Monitors health and capacity of load balancers, creates DNS plans for service endpoints
2. **DNS Enactor**: Operates redundantly across three AZs, enacts DNS plans by applying changes in Route53

#### The Race Condition

The race condition involved an unlikely interaction between two DNS Enactors:

1. **DNS Enactor 1** experienced unusually high delays while updating DNS endpoints
2. **DNS Enactor 2** began applying a newer plan and rapidly completed all endpoint updates
3. **DNS Enactor 2** invoked the plan cleanup process to delete older plans
4. **DNS Enactor 1** (still delayed) applied its much older plan to the regional DDB endpoint, overwriting the newer plan
5. The stale check that ensures plans are newer failed due to processing delays
6. **DNS Enactor 2's** cleanup process deleted the older plan, removing all IP addresses for the regional endpoint
7. The system was left in an inconsistent state preventing subsequent plan updates

## Timeline

### October 19, 2025

**11:48 PM PDT**: DynamoDB DNS issue begins - all systems unable to connect to DynamoDB service in us-east-1

### October 20, 2025

**12:38 AM**: Engineering teams identified DynamoDB's DNS state as the source of the outage

**1:15 AM**: Temporary mitigations applied, enabling some internal services to connect to DynamoDB

**1:19 AM**: STS service recovered after restoration of internal DynamoDB endpoints

**1:25 AM**: AWS Management Console authentication issues resolved

**2:25 AM**: All DynamoDB DNS information restored

**2:32 AM**: All global tables replicas fully caught up

**2:40 AM**: DynamoDB endpoint resolution and connections fully restored

**4:14 AM**: Engineers throttled incoming work and began selective DWFM host restarts for EC2 recovery

**5:28 AM**: DWFM established leases with all droplets, new EC2 launches starting to succeed

**5:30 AM**: NLB connection errors begin due to health check failures

**6:00 AM**: Lambda SQS message backlogs fully processed

**6:21 AM**: Network Manager experiencing increased latencies in network propagation

**6:52 AM**: NLB health check issues detected by monitoring systems

**9:36 AM**: Engineers disabled automatic health check failovers for NLB

**9:59 AM**: STS recovered from NLB-related issues

**10:36 AM**: Network configuration propagation times returned to normal

**11:23 AM**: Engineers began relaxing EC2 request throttles

**11:27 AM**: Lambda capacity sufficiently restored

**1:20 PM**: Amazon Connect service availability restored

**1:50 PM**: All EC2 APIs and instance launches operating normally

**2:09 PM**: NLB automatic DNS health check failover re-enabled

**2:15 PM**: Lambda normal service operations resumed

**2:20 PM**: ECS/EKS/Fargate services fully recovered

### October 21, 2025

**4:05 AM**: Redshift cluster availability fully restored

## Secondary Failures and Cascading Effects

### EC2 - DWFM Congestive Collapse

- **Root Cause**: DWFM state checks failed due to DynamoDB dependency
- **Impact**: Droplet leases timed out, causing "insufficient capacity" errors
- **Recovery Challenge**: Large number of droplets caused DWFM to enter congestive collapse
- **Resolution**: Selective DWFM host restarts to clear queues and reduce processing times

### Network Load Balancer Health Checks

- **Root Cause**: Health check subsystem brought new EC2 instances into service before network state fully propagated
- **Impact**: Alternating health check results caused nodes to be removed/returned to DNS repeatedly
- **Resolution**: Disabled automatic health check failovers, manually brought healthy nodes back into service

### Lambda Event Processing

- **Root Cause**: Internal subsystem responsible for polling SQS queues failed and didn't recover automatically
- **Impact**: SQS queue processing remained impaired even after DynamoDB recovery
- **Resolution**: Manual restoration of subsystem at 4:40 AM

## Preventive Measures

### Immediate Actions Taken

1. **Disabled DynamoDB DNS automation worldwide** pending fixes
2. **Enhanced testing**: Building additional test suite for DWFM recovery workflows
3. **NLB improvements**: Adding velocity control mechanism to limit capacity removal during health check failures
4. **EC2 throttling improvements**: Implementing queue-size-based rate limiting for data propagation systems

### Long-term Improvements

1. **Fix the race condition** in DynamoDB DNS management system
2. **Add additional protections** to prevent application of incorrect DNS plans
3. **Enhanced scale testing** to exercise DWFM recovery workflows
4. **Cross-service resilience review** to identify and mitigate similar failure modes

## Lessons Learned

### Architecture Insights

- **DNS as a critical dependency**: The extensive reliance on DNS for large-scale services requires robust automation and failsafe mechanisms
- **Race conditions in distributed systems**: Complex timing interactions can create rare but catastrophic failure modes
- **Cascade failure prevention**: Single service failures can cascade through dependent services in unexpected ways

### Operational Insights

- **Manual intervention capabilities**: Critical systems need well-defined manual recovery procedures
- **Monitoring and alerting**: Early detection systems successfully identified the root cause within ~50 minutes
- **Gradual recovery processes**: Throttling and controlled recovery helped prevent secondary failures during restoration

### Engineering Insights

- **Redundancy complexity**: Multiple redundant components can interact in unexpected ways during edge cases
- **State consistency**: Distributed state management requires careful handling of cleanup operations and timing
- **Dependency mapping**: Understanding service interdependencies is crucial for predicting failure modes

## Regional Impact Considerations

This incident highlighted the importance of:

- **Multi-region architecture** for critical workloads
- **DynamoDB Global Tables** provided continuity for customers with multi-region setups
- **Regional isolation** - other AWS regions remained unaffected
- **Cross-region dependencies** - some services had unexpected dependencies on us-east-1

---

_This document is a markdown conversion of the original AWS incident report for preservation and accessibility purposes. The original report can be found at the AWS Post-Event Summaries page._
