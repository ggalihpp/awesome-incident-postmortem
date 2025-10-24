# GitHub Pages Deployment Guide

This guide will help you deploy the Awesome Incident Postmortems repository to GitHub Pages.

## Quick Setup (Recommended)

### Step 1: Push to GitHub

1. Create a new repository on GitHub named `awesome-incident-postmortem`
2. Push your local repository:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/awesome-incident-postmortem.git
   git branch -M main
   git push -u origin main
   ```

### Step 2: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** tab
3. Scroll down to **Pages** section in the left sidebar
4. Under **Source**, select **Deploy from a branch**
5. Choose **main** branch and **/ (root)** folder
6. Click **Save**

### Step 3: Wait for Deployment

- GitHub will automatically build and deploy your site
- The build process takes 1-3 minutes
- Your site will be available at: `https://YOUR_USERNAME.github.io/awesome-incident-postmortem/`

## Custom Domain Setup (Optional)

### Step 1: Purchase a Domain

- Buy a domain from any registrar (e.g., `awesome-postmortems.com`)

### Step 2: Configure DNS

Add these DNS records at your domain registrar:

```
Type: CNAME
Name: www
Value: YOUR_USERNAME.github.io

Type: A
Name: @
Value: 185.199.108.153

Type: A
Name: @
Value: 185.199.109.153

Type: A
Name: @
Value: 185.199.110.153

Type: A
Name: @
Value: 185.199.111.153
```

### Step 3: Configure GitHub Pages

1. In your repository **Settings** > **Pages**
2. Under **Custom domain**, enter your domain: `awesome-postmortems.com`
3. Check **Enforce HTTPS** (wait for SSL certificate to be issued)

## Repository Configuration

The repository is already configured with:

### Jekyll Configuration (`_config.yml`)

- **Theme**: Minima (clean, professional)
- **Plugins**: SEO, sitemap, feed
- **Custom layouts**: For providers and incidents
- **Navigation structure**

### Custom Styling (`_sass/custom.scss`)

- **Responsive design** for mobile/desktop
- **Provider cards** with hover effects
- **Incident cards** with status indicators
- **Search functionality**
- **Dark mode support**

### Site Structure

```
├── index.md              # Main landing page with provider grid
├── source/               # Incident reports organized by provider
├── _layouts/            # Custom Jekyll layouts
├── _sass/               # Custom styles
└── assets/              # Compiled CSS
```

## Build Process

GitHub Pages automatically:

1. **Detects Jekyll** configuration
2. **Installs** required gems and plugins
3. **Compiles** SCSS to CSS
4. **Generates** static HTML from Markdown
5. **Deploys** to CDN for global distribution

## Monitoring and Analytics

### Site Performance

- Monitor site speed with [PageSpeed Insights](https://pagespeed.web.dev/)
- GitHub Pages includes global CDN for fast loading

### Analytics (Optional)

Add Google Analytics to `_config.yml`:

```yaml
google_analytics: "GA_TRACKING_ID"
```

### Search Console (Optional)

1. Verify domain ownership in [Google Search Console](https://search.google.com/search-console/)
2. Submit sitemap: `https://your-site.com/sitemap.xml`

## Troubleshooting

### Build Failures

- Check the **Actions** tab for build logs
- Common issues:
  - Invalid YAML front matter
  - Missing dependencies
  - Liquid template errors

### Slow Builds

- Jekyll builds are typically under 30 seconds
- Large repositories may take longer
- Consider using Jekyll plugins for optimization

### Content Not Updating

- Check git push was successful
- GitHub Pages builds automatically on push to main branch
- Clear browser cache if changes aren't visible

## Advanced Configuration

### Custom 404 Page

Create `404.md`:

```markdown
---
layout: default
permalink: /404.html
---

# Page Not Found

The incident you're looking for might have been moved or deleted.

[← Back to Home]({{ '/' | relative_url }})
```

### Search Engine Optimization

The site includes:

- **SEO plugin** for meta tags
- **Sitemap generation** for search engines
- **Structured data** for rich snippets
- **Open Graph** tags for social sharing

### Performance Optimization

- **Minified CSS** in production
- **Compressed images** (add images to `assets/images/`)
- **Lazy loading** for provider cards
- **CDN delivery** via GitHub Pages

## Maintenance

### Regular Updates

- Add new incidents as they become available
- Update broken links in existing reports
- Refresh provider information and statistics
- Monitor for security updates to Jekyll/plugins

### Content Guidelines

- Follow the established markdown structure
- Include proper YAML front matter
- Maintain consistent formatting
- Always attribute original sources

## Support

- **GitHub Issues**: For bugs and feature requests
- **Discussions**: For questions and community input
- **Jekyll Docs**: [jekyllrb.com/docs](https://jekyllrb.com/docs/)
- **GitHub Pages Docs**: [docs.github.com/pages](https://docs.github.com/en/pages)

---

Your site should now be live and automatically updating with every push to the main branch! 🎉
