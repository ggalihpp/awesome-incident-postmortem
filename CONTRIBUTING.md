---
layout: default
title: "Contributing to Awesome Incident Postmortems"
---

# Contributing to Awesome Incident Postmortems

We welcome contributions! This guide will help you add new incident reports or improve existing documentation.

## Types of Contributions

### 1. Adding New Incident Reports

- Find publicly available incident postmortems from technology companies
- Rewrite them in clear, well-formatted markdown
- Preserve the technical details and insights
- Add proper metadata and links to original sources

### 2. Improving Existing Reports

- Fix typos or formatting issues
- Add additional context or analysis
- Update broken links
- Improve readability and structure

### 3. Adding New Providers

- Create new provider directories for companies not yet covered
- Follow the existing directory structure and naming conventions

## How to Contribute

### Step 1: Fork and Clone

1. Fork this repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/awesome-incident-postmortem.git
   cd awesome-incident-postmortem
   ```

### Step 2: Create a New Branch

```bash
git checkout -b add-new-incident-PROVIDER-YYYY-MM-DD
```

### Step 3: Add Your Content

#### For New Incidents:

1. Navigate to the appropriate provider directory: `source/PROVIDER/`
2. Create a new file using the naming convention: `YYYY-MM-DD-brief-description.md`
3. Use this template:

```markdown
---
layout: incident
title: "Incident Title"
incident_date: "Date or Date Range"
duration: "Duration of incident"
severity: "critical|major|minor"
provider: "provider-name"
original_source: "https://original-report-url"
---

# Incident Title

**Incident Date**: Date  
**Duration**: Duration  
**Root Cause**: Brief description

## Executive Summary

Brief overview of what happened...

## Impact Overview

What services and users were affected...

## Root Cause Analysis

Technical details of what caused the incident...

## Timeline

Detailed timeline of events...

## Preventive Measures

What was done to prevent future occurrences...

## Lessons Learned

Key insights and takeaways...

---

_This document is a markdown conversion of the original incident report for preservation and accessibility purposes. The original report can be found at [Original Source](original-url)._
```

#### For New Providers:

1. Create a new directory: `source/new-provider/`
2. Add a `README.md` file with provider information:

```markdown
---
layout: provider
title: "Provider Name Incident Postmortems"
provider: "provider-name"
---

# Provider Name Incident Postmortems

Brief description of the provider and their incident reporting practices.

## Incidents

_No incidents documented yet. Contributions welcome!_

## About Provider Incident Reports

Information about how this provider handles incident reporting...

## Additional Resources

- [Provider Status Page](https://status.provider.com/)
- [Provider Blog](https://blog.provider.com/)
```

### Step 4: Update References

1. Update the main `README.md` if adding a new provider
2. Update the provider's `README.md` to include your new incident
3. Update `index.md` to reflect new incidents in the provider cards

### Step 5: Test Your Changes

If you want to test the Jekyll site locally:

```bash
bundle install
bundle exec jekyll serve
```

Visit `http://localhost:4000` to preview your changes.

### Step 6: Submit Your Changes

1. Commit your changes:
   ```bash
   git add .
   git commit -m "Add [Provider] incident: [Brief Description] ([Date])"
   ```
2. Push to your fork:
   ```bash
   git push origin add-new-incident-PROVIDER-YYYY-MM-DD
   ```
3. Create a Pull Request on GitHub

## Content Guidelines

### Quality Standards

- **Accuracy**: Ensure technical details are correct and sourced
- **Clarity**: Write for both technical and non-technical audiences
- **Completeness**: Include timeline, root cause, impact, and lessons learned
- **Attribution**: Always link to and credit original sources

### Formatting Requirements

- Use consistent markdown formatting
- Include proper YAML front matter
- Follow the established document structure
- Use clear headings and sections

### What to Include

- ✅ Publicly available incident reports
- ✅ Technical root cause analysis
- ✅ Timeline of events
- ✅ Impact assessment
- ✅ Prevention measures
- ✅ Links to original sources

### What NOT to Include

- ❌ Confidential or leaked information
- ❌ Personal opinions or speculation
- ❌ Unverified information
- ❌ Reports without proper attribution

## Provider Criteria

We include incident reports from:

- Major technology companies and service providers
- Companies that publish detailed, technical postmortems
- Services with significant user bases or industry impact
- Open source projects with substantial usage

## Getting Help

- Create an issue for questions or suggestions
- Check existing issues and pull requests
- Follow the existing patterns in the repository
- Ask questions in your pull request if unsure

## Code of Conduct

- Be respectful and constructive in all interactions
- Focus on the technical aspects of incidents
- Respect the companies that share their learnings publicly
- Help maintain the educational value of this resource

Thank you for contributing to the preservation and sharing of incident response knowledge! 🎯
