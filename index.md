---
layout: default
title: Awesome Incident Postmortems
---

<div class="search-container">
  <input type="text" id="search-input" placeholder="Search incidents by company, technology, or keyword...">
</div>

<div class="provider-grid" id="provider-grid">
  
  <div class="provider-card" data-provider="aws">
    <div class="provider-header">
      <h3><a href="{{ '/source/aws/' | relative_url }}">Amazon Web Services (AWS)</a></h3>
      <span class="incident-count">1 incident</span>
    </div>
    <div class="provider-description">
      Cloud computing platform with comprehensive incident reporting
    </div>
    <ul class="provider-incidents">
      <li>
        <span class="incident-date">2025-10-19</span>
        <a href="{{ '/source/aws/2025-10-19-dynamodb-us-east-1-disruption.html' | relative_url }}">
          DynamoDB Service Disruption in US-East-1
        </a>
      </li>
    </ul>
  </div>
  
  <div class="provider-card" data-provider="cloudflare">
    <div class="provider-header">
      <h3><a href="{{ '/source/cloudflare/' | relative_url }}">Cloudflare</a></h3>
      <span class="incident-count">1 incident</span>
    </div>
    <div class="provider-description">
      Global content delivery network and DDoS protection
    </div>
    <ul class="provider-incidents">
      <li>
        <span class="incident-date">2022-06-21</span>
        <a href="{{ '/source/cloudflare/2022-06-21-global-outage-bgp-policy.html' | relative_url }}">
          Global Outage - BGP Policy Configuration
        </a>
      </li>
    </ul>
  </div>
  
  <div class="provider-card" data-provider="github">
    <div class="provider-header">
      <h3><a href="{{ '/source/github/' | relative_url }}">GitHub</a></h3>
      <span class="incident-count">1 incident</span>
    </div>
    <div class="provider-description">
      Development platform with detailed database incident analysis
    </div>
    <ul class="provider-incidents">
      <li>
        <span class="incident-date">2018-10-21</span>
        <a href="{{ '/source/github/2018-10-21-database-network-partition.html' | relative_url }}">
          Database Network Partition Incident
        </a>
      </li>
    </ul>
  </div>
  
  <div class="provider-card" data-provider="google">
    <div class="provider-header">
      <h3><a href="{{ '/source/google/' | relative_url }}">Google Cloud</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Google Cloud Platform and services
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
  <div class="provider-card" data-provider="microsoft">
    <div class="provider-header">
      <h3><a href="{{ '/source/microsoft/' | relative_url }}">Microsoft Azure</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Microsoft cloud platform and enterprise services
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
  <div class="provider-card" data-provider="slack">
    <div class="provider-header">
      <h3><a href="{{ '/source/slack/' | relative_url }}">Slack</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Business communication platform
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
  <div class="provider-card" data-provider="facebook">
    <div class="provider-header">
      <h3><a href="{{ '/source/facebook/' | relative_url }}">Meta/Facebook</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Social media platform and metaverse company
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
  <div class="provider-card" data-provider="netflix">
    <div class="provider-header">
      <h3><a href="{{ '/source/netflix/' | relative_url }}">Netflix</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Video streaming service and chaos engineering pioneer
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
  <div class="provider-card" data-provider="stripe">
    <div class="provider-header">
      <h3><a href="{{ '/source/stripe/' | relative_url }}">Stripe</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Payment processing platform
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
  <div class="provider-card" data-provider="others">
    <div class="provider-header">
      <h3><a href="{{ '/source/others/' | relative_url }}">Other Providers</a></h3>
      <span class="incident-count">0 incidents</span>
    </div>
    <div class="provider-description">
      Various other technology companies and services
    </div>
    <div class="no-incidents">No incidents documented yet. Contributions welcome!</div>
  </div>
  
</div>

## Latest Incidents

<div class="recent-incidents">
  <div class="incident-card">
    <div class="incident-title">
      <a href="{{ '/source/aws/2025-10-19-dynamodb-us-east-1-disruption.html' | relative_url }}">
        Amazon DynamoDB Service Disruption in Northern Virginia (US-EAST-1)
      </a>
    </div>
    <div class="incident-meta">
      <span class="meta-item">
        <span class="status-indicator critical"></span>
        AWS
      </span>
      <span class="meta-item">📅 October 19-20, 2025</span>
      <span class="meta-item">⏱️ 14.5 hours</span>
    </div>
    <div class="incident-description">
      Race condition in DynamoDB DNS management system caused widespread service disruption affecting DynamoDB, EC2, Lambda, and other AWS services in US-East-1 region.
    </div>
  </div>
  
  <div class="incident-card">
    <div class="incident-title">
      <a href="{{ '/source/cloudflare/2022-06-21-global-outage-bgp-policy.html' | relative_url }}">
        Cloudflare Global Outage - BGP Policy Configuration Error
      </a>
    </div>
    <div class="incident-meta">
      <span class="meta-item">
        <span class="status-indicator major"></span>
        Cloudflare
      </span>
      <span class="meta-item">📅 June 21, 2022</span>
      <span class="meta-item">⏱️ 75 minutes</span>
    </div>
    <div class="incident-description">
      BGP policy term reordering during routine configuration update caused prefix withdrawal, taking 19 major data centers offline and impacting 50% of global requests.
    </div>
  </div>
  
  <div class="incident-card">
    <div class="incident-title">
      <a href="{{ '/source/github/2018-10-21-database-network-partition.html' | relative_url }}">
        GitHub Database Network Partition Incident
      </a>
    </div>
    <div class="incident-meta">
      <span class="meta-item">
        <span class="status-indicator critical"></span>
        GitHub
      </span>
      <span class="meta-item">📅 October 21-22, 2018</span>
      <span class="meta-item">⏱️ 24 hours 11 minutes</span>
    </div>
    <div class="incident-description">
      43-second network partition during maintenance triggered database failover creating data inconsistencies, leading to extended recovery prioritizing data integrity over availability.
    </div>
  </div>
</div>

## About This Collection

This repository aims to preserve and organize public incident postmortem reports, providing valuable insights into how major tech companies handle outages, security incidents, and system failures. All documents are rewritten in markdown format to ensure long-term accessibility and preservation.

### What You'll Learn

- **Root cause analysis methodologies** from leading tech companies
- **Incident response procedures** and decision-making processes
- **System architecture patterns** and common failure modes
- **Communication strategies** during critical incidents
- **Prevention and mitigation techniques** to avoid similar issues

### Contributing

We welcome contributions! Please see our [contributing guidelines]({{ '/CONTRIBUTING.html' | relative_url }}) for information on how to add new incidents or improve existing documentation.

<script>
// Simple search functionality
document.addEventListener('DOMContentLoaded', function() {
  const searchInput = document.getElementById('search-input');
  const providerCards = document.querySelectorAll('.provider-card');
  const incidentCards = document.querySelectorAll('.incident-card');
  
  searchInput.addEventListener('input', function() {
    const query = this.value.toLowerCase();
    
    // Search provider cards
    providerCards.forEach(card => {
      const provider = card.dataset.provider;
      const text = card.textContent.toLowerCase();
      
      if (text.includes(query) || provider.includes(query)) {
        card.style.display = 'block';
      } else {
        card.style.display = 'none';
      }
    });
    
    // Search incident cards
    incidentCards.forEach(card => {
      const text = card.textContent.toLowerCase();
      
      if (text.includes(query)) {
        card.style.display = 'block';
      } else {
        card.style.display = 'none';
      }
    });
  });
});
</script>
