# DNS Configuration for Website Migration

## Overview

After migrating the website from `reccaller-ai/reccall` to `reccaller-ai/websites`, the DNS configuration may need to be verified or updated.

## Current Setup

- **Custom Domain**: `reccaller.ai`
- **CNAME File**: Present in `websites` repository
- **GitHub Organization**: `reccaller-ai`
- **Old Repository**: `reccaller-ai/reccall` (website archived)
- **New Repository**: `reccaller-ai/websites` (active website)

## DNS Configuration Requirements

### ✅ Good News: DNS Records Likely Don't Need Changes

Since both repositories are under the same GitHub organization (`reccaller-ai`), and the custom domain (`reccaller.ai`) remains the same, **Cloudflare DNS records typically don't need to be changed**.

### 📋 Verification Steps

#### 1. Check Current Cloudflare DNS Records

In Cloudflare dashboard, verify your DNS records for `reccaller.ai`:

**For Apex Domain (reccaller.ai):**
- **Type**: `A` records
- **IP Addresses**: Should point to GitHub Pages IPs:
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`
- **Proxy Status**: Should be "DNS only" (gray cloud) ☁️

**For WWW Subdomain (www.reccaller.ai):**
- **Type**: `CNAME`
- **Name**: `www`
- **Target**: `reccaller-ai.github.io` (or your GitHub Pages URL)
- **Proxy Status**: Should be "DNS only" (gray cloud) ☁️

#### 2. Configure GitHub Pages Custom Domain

**In the new repository** (`reccaller-ai/websites`):

1. Go to: https://github.com/reccaller-ai/websites/settings/pages
2. Under **"Custom domain"**, enter: `reccaller.ai`
3. Click **"Save"**
4. GitHub will verify the DNS configuration
5. Enable **"Enforce HTTPS"** (recommended)

#### 3. Verify DNS Propagation

After updating GitHub Pages settings:

```bash
# Check DNS records
dig reccaller.ai +short

# Check GitHub Pages verification
# GitHub will show verification status in repository settings
```

### ⚠️ When DNS Changes ARE Needed

DNS changes are only needed if:

1. **Different GitHub Account/Organization**: If you moved to a different GitHub account/org, the GitHub Pages URL changes, and DNS needs updating.

2. **Different Domain**: If you're using a different domain name.

3. **Previous DNS Pointed to Specific Repo**: If your DNS was configured to point to a specific repository path (unlikely for apex domains).

### ✅ What We Already Have Correct

1. ✅ `CNAME` file exists in `websites` repository with `reccaller.ai`
2. ✅ Same GitHub organization (`reccaller-ai`)
3. ✅ Same custom domain (`reccaller.ai`)

### 📝 Action Items

1. **In GitHub** (websites repository):
   - ✅ CNAME file already present
   - ⚠️ **REQUIRED**: Enable GitHub Pages with custom domain in repository settings
   - ⚠️ **REQUIRED**: Configure custom domain in Settings → Pages

2. **In Cloudflare**:
   - ✅ **Verify** DNS records are correct (likely no changes needed)
   - ✅ **Confirm** A records point to GitHub Pages IPs
   - ✅ **Confirm** Proxy status is "DNS only" (not proxied)

3. **Verification**:
   - Wait for DNS propagation (5-60 minutes)
   - Test `reccaller.ai` in browser
   - Verify HTTPS certificate is issued

## Summary

**Most Likely Scenario**: No Cloudflare DNS changes needed.

**Why**: Same organization, same domain, just different repository. GitHub Pages handles the routing based on the `CNAME` file and repository settings.

**Only Action Required**: 
- Enable GitHub Pages in `reccaller-ai/websites` repository settings
- Configure custom domain in GitHub Pages settings
- Verify DNS records in Cloudflare (should already be correct)

## Troubleshooting

If the site doesn't work after migration:

1. Check GitHub Pages deployment status
2. Verify CNAME file in repository
3. Check DNS records in Cloudflare match GitHub Pages requirements
4. Wait for DNS propagation (can take up to 48 hours, usually 5-60 minutes)
5. Check GitHub Pages domain verification status

## Reference Links

- GitHub Pages Custom Domain Docs: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
- Cloudflare DNS Settings: https://dash.cloudflare.com
- GitHub Pages Settings: https://github.com/reccaller-ai/websites/settings/pages

