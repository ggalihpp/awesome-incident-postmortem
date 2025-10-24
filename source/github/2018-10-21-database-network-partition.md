---
layout: incident
title: "October 21, 2018 GitHub Database Incident"
incident_date: "October 21-22, 2018"
duration: "24 hours and 11 minutes (22:52 UTC Oct 21 - 23:03 UTC Oct 22)"
root_cause: "Network partition leading to database failover with data inconsistencies"
severity: "critical"
provider: "github"
original_source: "https://github.blog/2018-10-30-oct21-post-incident-analysis/"
---

# October 21, 2018 GitHub Database Incident

**Incident Date**: October 21-22, 2018  
**Duration**: 24 hours and 11 minutes (22:52 UTC Oct 21 - 23:03 UTC Oct 22)  
**Root Cause**: Network partition leading to database failover with data inconsistencies  
**Original Source**: [GitHub Blog Post-Incident Analysis](https://github.blog/2018-10-30-oct21-post-incident-analysis/)

## Executive Summary

On October 21, 2018, GitHub experienced a major incident that resulted in degraded service for over 24 hours. The incident was triggered by a 43-second network partition during routine maintenance, which caused automated database failover systems to promote database primaries across geographic regions. This created data inconsistencies that required extensive manual recovery procedures, prioritizing data integrity over service availability.

## Impact Overview

### Service Impact

- **GitHub.com**: Severely degraded performance and inconsistent data display
- **Webhook delivery**: Paused for ~24 hours (5+ million events queued)
- **GitHub Pages**: Builds paused (80,000+ builds queued)
- **Git operations**: Available throughout (repository data unaffected)
- **Authentication & API**: Significantly degraded performance

### User Experience

- Slow page loads for operations requiring database writes
- Inconsistent information displayed across the platform
- Users seeing outdated data due to read replica delays
- Complete inability to receive webhooks or deploy Pages sites
- Cross-country database latency causing timeouts

### Data Impact

- **No user data lost**
- Small number of writes (e.g., 954 writes on busiest cluster) required manual reconciliation
- ~200,000 webhook payloads exceeded TTL and were dropped
- Database consistency restored through backup restoration process

## Root Cause Analysis

### Architecture Background

GitHub operates multiple MySQL clusters varying from hundreds of gigabytes to nearly 5 terabytes, with dozens of read replicas per cluster. The infrastructure uses:

- **Orchestrator**: MySQL cluster topology management with automated failover
- **Raft consensus**: For Orchestrator leader election and decision making
- **Geographic distribution**: Primary data center (US East Coast) with West Coast backup
- **Read/write split**: Applications write to primaries, read from local replicas

### The Triggering Event

At 22:52 UTC on October 21, **routine maintenance to replace failing 100G optical equipment** resulted in a **43-second network partition** between the US East Coast network hub and primary data center.

### The Cascade Failure

#### Step 1: Orchestrator Failover (22:52 UTC)

- Network partition isolated East Coast Orchestrator instance
- West Coast and cloud Orchestrator nodes established quorum via Raft consensus
- Automated failover promoted West Coast databases as new primaries
- Applications immediately began writing to West Coast databases

#### Step 2: Split-Brain Condition (22:52 - 23:35 UTC)

- **43 seconds** of writes existed in East Coast that hadn't replicated West
- **40+ minutes** of new writes accumulated in West Coast
- Both data centers now contained writes not present in the other
- Impossible to safely fail back to East Coast without data loss

#### Step 3: Cross-Country Latency (23:35+ UTC)

- Applications in East Coast writing to West Coast databases
- **Cross-country round trips** for every database operation
- Severe performance degradation due to latency
- Read replicas in East Coast hours behind West Coast primary

## Timeline

### October 21, 2018

**22:52 UTC**: **INCIDENT STARTS** - Network partition occurs during maintenance, triggers Orchestrator failover to West Coast

**22:54 UTC**: Internal monitoring systems generate alerts indicating widespread faults

**23:02 UTC**: First responders determine database cluster topologies in unexpected state

**23:07 UTC**: Decision made to manually lock deployment tooling

**23:09 UTC**: Site status changed to **Yellow** (automatic incident escalation)

**23:11 UTC**: Incident coordinator joins

**23:13 UTC**: Site status changed to **Red**

- Database engineering team paged
- Decision made to **fail-forward** to preserve data integrity
- Cross-country latency makes service unusable but protects data

**23:19 UTC**: **Webhook delivery and Pages builds paused** to prevent further data inconsistencies

### October 22, 2018

**00:05 UTC**: Recovery plan developed:

1. Restore from backups
2. Synchronize replicas in both sites
3. Fall back to stable topology
4. Resume processing queued jobs

**00:41 UTC**: Backup restoration initiated for all affected MySQL clusters

**06:51 UTC**: Several clusters completed restoration, began replicating from West Coast

- **ETA published**: 2 hours based on linear interpolation of replication progress

**07:46 UTC**: GitHub blog post published (delayed due to Pages build pause)

**11:12 UTC**: **All database primaries re-established in East Coast**

- Site becomes far more responsive
- Read replicas still hours delayed, causing inconsistent data display

**13:15 UTC**: **Peak traffic approaching** - replication delays increasing instead of decreasing

- Additional MySQL read replicas provisioned in East Coast cloud
- Spreading read load allows replication to catch up

**16:24 UTC**: **Replicas synchronized** - failover to original topology completed

**16:45 UTC**: **Backlog processing begins**

- 5+ million webhook events queued
- 80,000+ Pages builds queued
- ~200,000 webhook payloads exceeded TTL and dropped

**23:03 UTC**: **INCIDENT ENDS** - All backlogs processed, site status returns to Green

## Technical Deep Dive

### MySQL Cluster Architecture

```
Normal Topology:
┌─────────────────┐    ┌─────────────────┐
│   US East Coast │    │   US West Coast │
│                 │    │                 │
│ Primary DB ●────┼────┼────● Replica    │
│ Replicas ●●●    │    │                 │
│ Apps ●●●●       │    │                 │
└─────────────────┘    └─────────────────┘

Post-Failover Topology:
┌─────────────────┐    ┌─────────────────┐
│   US East Coast │    │   US West Coast │
│                 │    │                 │
│ Stale Replicas  │    │ ● Primary DB    │
│ Apps ●●●● ──────┼────┼──── (writes)    │
│         (writes)│    │                 │
└─────────────────┘    └─────────────────┘
```

### Orchestrator Configuration Issue

- Orchestrator was **configured to allow cross-regional failover**
- Applications were **not designed to handle cross-country database latency**
- **Configuration gap**: Orchestrator could create topologies applications couldn't support

### Recovery Challenges

#### Backup Restoration Bottlenecks

- **Multiple terabytes** of backup data stored in remote cloud blob storage
- **Transfer time**: Hours to download from remote storage
- **Processing time**: Decompress, checksum, prepare, and load onto new MySQL servers
- **Tested daily** but never needed full cluster rebuilds before

#### Replication Catch-up Complexity

- **Power decay function**: Recovery time followed non-linear trajectory, not linear estimate
- **Increasing write load**: European and US users waking up slowed recovery
- **Read replica delays**: Users seeing inconsistent data for hours

## Detection and Response

### Monitoring and Alerting

- **2 minutes**: First alerts generated (22:54 UTC)
- **8 minutes**: Problem scope identified (23:02 UTC)
- **21 minutes**: Full incident declared (23:13 UTC)

### Decision Making Process

- **Data integrity prioritized** over service availability
- **Fail-forward approach** chosen to prevent data loss
- **Extended degradation accepted** to ensure data consistency

### Communication Challenges

- **Inaccurate ETAs** due to linear recovery time estimates
- **GitHub Pages dependency**: Own internal Pages builds were paused, delaying blog post
- **Status granularity**: Only Red/Yellow/Green available, didn't reflect partial availability

## Preventive Measures

### Immediate Technical Changes

1. **Orchestrator Configuration**: Prevent promotion of database primaries across regional boundaries
2. **Status System Migration**: Accelerated migration to component-based status reporting
3. **Active/Active/Active Initiative**: Accelerated multi-data center project for N+1 redundancy

### Long-term Technical Initiatives

1. **Failure Scenario Testing**: Systematic validation of failure scenarios before they affect users
2. **Fault Injection**: Investment in chaos engineering tooling
3. **Assumption Testing**: More proactive stance in testing architectural assumptions

### Organizational Changes

1. **Site Reliability Mindset Shift**: Beyond operational controls to systemic failure validation
2. **Chaos Engineering**: Formal practice of validating failure scenarios
3. **Knowledge Transfer**: Better capture and transfer of historical context and trade-offs

## Lessons Learned

### Architecture Insights

- **Automated failover systems** can create topologies that applications can't handle
- **Cross-regional database latency** is often incompatible with application performance requirements
- **Configuration alignment** between infrastructure automation and application capabilities is critical

### Operational Insights

- **Data integrity vs. availability** trade-offs require clear decision frameworks
- **Backup restoration** from remote storage can be a significant bottleneck during incidents
- **Linear extrapolation** of recovery times can be highly inaccurate during real incidents

### Communication Insights

- **Component-level status** reporting provides much better user experience than monolithic red/yellow/green
- **Internal tooling dependencies** can prevent incident communication (e.g., Pages builds paused preventing blog posts)
- **Time estimates** should factor in all variables, not just primary recovery metrics

### Engineering Insights

- **System complexity** makes it increasingly difficult to capture and transfer historical context
- **Fast growth** can outpace institutional knowledge about architectural trade-offs
- **Testing assumptions** becomes more critical as systems grow in complexity

## Data Recovery Process

### Write Reconciliation

- **954 writes** on busiest cluster during partition window
- **Manual analysis** of MySQL binary logs for non-replicated writes
- **Automatic reconciliation** for writes that were later repeated by users
- **User outreach** planned for writes requiring manual intervention

### Webhook Recovery

- **5+ million webhook events** queued during outage
- **200,000 payloads** exceeded TTL and were permanently lost
- **TTL increased** to prevent future payload loss during recovery
- **Gradual processing** to avoid overwhelming ecosystem partners

## Industry Impact

This incident became a significant case study for:

1. **Database Failover Complexity**: Highlighted challenges of automated failover in distributed systems
2. **Cross-Region Latency**: Demonstrated application performance impacts of geographic database distribution
3. **Data Integrity Trade-offs**: Showed importance of prioritizing data consistency over availability
4. **Communication During Incidents**: Illustrated need for granular status reporting and realistic time estimates

---

_This document is a markdown conversion of the original GitHub incident analysis for preservation and accessibility purposes. The original report can be found on the GitHub blog._
