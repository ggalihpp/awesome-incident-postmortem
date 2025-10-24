---
layout: incident
title: "Cloudflare Outage on June 21, 2022"
incident_date: "June 21, 2022"
duration: "75 minutes (06:27 UTC - 07:42 UTC)"
affected_locations: "19 Multi-Colo PoP (MCP) data centers"
severity: "major"
provider: "cloudflare"
original_source: "https://blog.cloudflare.com/cloudflare-outage-on-june-21-2022"
---

# Cloudflare Outage on June 21, 2022

**Incident Date**: June 21, 2022  
**Duration**: 75 minutes (06:27 UTC - 07:42 UTC)  
**Affected Locations**: 19 Multi-Colo PoP (MCP) data centers  
**Original Source**: [Cloudflare Blog](https://blog.cloudflare.com/cloudflare-outage-on-june-21-2022)

## Executive Summary

On June 21, 2022, Cloudflare experienced a significant outage affecting 19 of its busiest data centers. The incident lasted from 06:27 UTC to 07:42 UTC (75 minutes) and was caused by a configuration change that inadvertently withdrew critical network prefixes. Despite affecting only 4% of Cloudflare's total network locations, the outage impacted 50% of total requests due to the critical nature of these data centers.

## Impact Overview

### Service Impact

- **50% reduction** in successful HTTP requests globally
- Websites and services relying on Cloudflare became **inaccessible** in affected regions
- **Internal load balancing system** (Multimog) stopped working
- **Origin server connectivity** lost in affected locations

### Geographic Impact

- **19 major data centers** across multiple continents:
  - **Europe**: Amsterdam, Frankfurt, London, Madrid, Manchester, Milan
  - **Americas**: Atlanta, Ashburn, Chicago, Los Angeles, Miami, Newark, São Paulo, San Jose
  - **Asia-Pacific**: Mumbai, Osaka, Singapore, Sydney, Tokyo

### User Experience

- Complete inability to access Cloudflare-protected websites and services
- Regional variations - some locations continued operating normally
- No degradation for users in unaffected regions

## Root Cause Analysis

### Background Architecture

The incident occurred during the rollout of **Multi-Colo PoP (MCP)** architecture - a new, more resilient design based on a **Clos network** topology with an additional routing layer (spines) creating a mesh of connections.

### The Configuration Error

The root cause was an **unintended reordering of BGP policy terms** during a routine configuration update to standardize BGP communities on site-local prefixes.

#### What Was Intended

- Adding informational BGP communities to site-local prefixes
- Standardizing infrastructure configuration across locations
- No functional change to routing behavior

#### What Actually Happened

```
# Policy terms were reordered during the change:
!    term REJECT-THE-REST { ... }
!    term 4-ADV-SITE-LOCALS { ... }  # Moved from top to bottom
!    term 6-ADV-SITE-LOCALS { ... }  # Moved from top to bottom
```

The critical **4-ADV-SITE-LOCALS** and **6-ADV-SITE-LOCALS** terms moved from the top of the policy to below the **REJECT-THE-REST** term:

```
term REJECT-THE-REST {
    then reject;
}
```

This meant site-local prefixes were now **explicitly rejected** instead of being advertised.

### BGP Policy Processing

- BGP policies are **evaluated sequentially**
- Once a **reject** term matches, no further terms are processed
- Moving the site-local terms below the reject term effectively **withdrew all site-local prefixes**

## Timeline

### June 21, 2022

**03:56 UTC**: Change deployed to first location (older architecture) - no impact

**06:17 UTC**: Change deployed to busiest locations without MCP architecture - no impact

**06:27 UTC**: **INCIDENT STARTS** - Change reaches MCP-enabled locations and is deployed to spine routers, immediately taking 19 locations offline

**06:32 UTC**: Internal Cloudflare incident declared

**06:51 UTC**: First diagnostic change made on a router to verify root cause

**06:58 UTC**: **Root cause identified and understood** - work begins to revert the problematic change

**07:42 UTC**: **Last revert completed** - incident resolution delayed as network engineers' changes conflicted with each other, causing sporadic re-appearances of the problem

**08:00 UTC**: Incident formally closed

## Technical Deep Dive

### Site-Local Prefixes

- Enable communication between servers within a data center
- Required for connecting to customer origin servers
- Critical for internal load balancing operations

### Cascade Effects

1. **Direct Connectivity Loss**: Servers lost ability to reach origin servers
2. **Load Balancer Failure**: Multimog (internal load balancer) stopped functioning
3. **Capacity Imbalance**: Smaller compute clusters received same traffic as larger ones
4. **Cluster Overload**: Smaller clusters became overwhelmed with traffic

### MCP Architecture Vulnerability

The new Multi-Colo PoP architecture, designed to improve resilience, inadvertently created a **single point of failure** when the spine layer configuration was incorrectly modified across all MCP locations simultaneously.

## Detection and Response

### Monitoring and Alerting

- Incident detected **immediately** when locations went offline at 06:27 UTC
- Internal monitoring systems quickly identified the scope
- Incident declared within **5 minutes** (06:32 UTC)

### Diagnosis Process

- **24 minutes** to identify root cause (06:27 UTC - 06:51 UTC)
- **7 minutes** to confirm root cause (06:51 UTC - 06:58 UTC)
- **44 minutes** to complete remediation (06:58 UTC - 07:42 UTC)

### Remediation Challenges

- **Network engineer coordination**: Multiple engineers working simultaneously caused conflicts
- **Change collision**: Engineers inadvertently reverted each other's fixes
- **Access difficulties**: Prefix withdrawal made remote access to affected locations challenging
- **Backup procedures**: Had to rely on out-of-band management for recovery

## Preventive Measures

### Process Improvements

1. **Enhanced Stagger Policy**: Update rollout procedures to include MCP-specific test deployments earlier in the process
2. **MCP-Aware Change Procedures**: Create specialized change management for MCP locations
3. **Smaller Step Sizes**: Reduce the scope of individual changes to catch errors before widespread impact

### Architecture Improvements

1. **Policy Statement Redesign**: Restructure BGP policies to prevent unintentional term reordering
2. **Configuration Validation**: Implement stronger pre-deployment validation for routing policies
3. **Fail-Safe Mechanisms**: Add safeguards against complete prefix withdrawal

### Automation Enhancements

1. **Automated Commit-Confirm**: Implement automatic rollback if changes cause connectivity loss
2. **Enhanced Monitoring**: Real-time validation of BGP advertisement correctness
3. **Staged Rollout Automation**: Automated enforcement of improved stagger policies

## Lessons Learned

### Change Management

- **Sequential policy evaluation** requires extreme care in term ordering
- **Diff review processes** need to highlight term reordering more prominently
- **Stagger procedures** must account for architecture-specific risks

### Architecture Considerations

- **Centralized spine layers** can create unexpected single points of failure
- **New architectures** require specialized change management procedures
- **Resilience improvements** can introduce new failure modes

### Operational Insights

- **BGP prefix withdrawal** can have immediate and widespread impact
- **Configuration changes** in critical infrastructure need extensive validation
- **Remote access dependencies** can complicate recovery during network issues

### Human Factors

- **Multiple engineers** working on recovery can conflict without coordination
- **Time pressure** can lead to mistakes that prolong incidents
- **Communication protocols** are essential during high-stress recovery

## Industry Impact

This incident highlighted several important considerations for large-scale network operators:

1. **Centralized Architecture Risks**: Even resilience-focused designs can introduce new failure modes
2. **BGP Configuration Fragility**: Small configuration changes can have massive impact
3. **Change Management at Scale**: Traditional procedures may not account for new architectural patterns
4. **Recovery Coordination**: Multiple responders need clear coordination protocols

---

_This document is a markdown conversion of the original Cloudflare incident report for preservation and accessibility purposes. The original report can be found on the Cloudflare blog._
