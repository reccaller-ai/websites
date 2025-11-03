# Verify Website Migration - Which Repository is Serving?

## Quick Test

To verify which repository is currently serving `reccaller.ai`:

### Method 1: Check GitHub Pages Settings

1. **Check old repository** (`reccaller-ai/reccall`):
   - Go to: https://github.com/reccaller-ai/reccall/settings/pages
   - Check if custom domain `reccaller.ai` is configured
   - If YES → Old repo is still serving
   - If NO or Pages disabled → Good!

2. **Check new repository** (`reccaller-ai/websites`):
   - Go to: https://github.com/reccaller-ai/websites/settings/pages
   - Check if custom domain `reccaller.ai` is configured
   - Check if Pages is enabled with "GitHub Actions" source
   - If YES → New repo should be serving (after DNS propagation)

### Method 2: Check Website Content

Look for differences between old and new content:

1. **Check homepage for unique content:**
   - Old repo might have different version numbers
   - New repo has updated content with CLI demo, latest features

2. **Check Git commit timestamps:**
   - View page source
   - Check for any version indicators or timestamps

### Method 3: Check DNS and GitHub Pages Status

```bash
# Check DNS resolution
dig reccaller.ai +short

# Check GitHub Pages status via API
gh api repos/reccaller-ai/reccall/pages
gh api repos/reccaller-ai/websites/pages
```

## Current Status Analysis

Based on your Cloudflare config showing `contexts` subdomain, but we need to check:

### For Apex Domain (`reccaller.ai`):

**Required DNS Records in Cloudflare:**
- **Type**: `A` records (not CNAME for apex)
- **IP Addresses** (should have 4 records):
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`
- **Proxy Status**: Should be "DNS only" (gray cloud) for GitHub Pages

**If you see CNAME for apex domain:**
- That's incorrect - apex domains must use A records
- GitHub Pages will handle the routing based on repository settings

### For `contexts` Subdomain:

Your current config shows:
- **Type**: CNAME ✅
- **Name**: `contexts`
- **Target**: `reccaller.github.io` ✅
- **Proxy**: Proxied (orange cloud) - This is OK for subdomains

## Action Plan

### Step 1: Disable Pages in Old Repository

1. Go to: https://github.com/reccaller-ai/reccall/settings/pages
2. If custom domain `reccaller.ai` is set, **remove it**
3. Or disable Pages entirely if not needed

### Step 2: Enable Pages in New Repository

1. Go to: https://github.com/reccaller-ai/websites/settings/pages
2. Select "GitHub Actions" as source
3. Under "Custom domain", enter: `reccaller.ai`
4. Click "Save"
5. Enable "Enforce HTTPS"

### Step 3: Verify Cloudflare DNS

For `reccaller.ai` (apex domain):
- Should have **4 A records** (not CNAME)
- IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Proxy: **DNS only** (gray cloud ☁️)

For `www.reccaller.ai` (if used):
- Type: CNAME
- Target: `reccaller-ai.github.io` or your Pages URL
- Proxy: DNS only or Proxied (both work)

### Step 4: Wait and Verify

1. Wait 5-60 minutes for DNS/GitHub changes to propagate
2. Visit https://reccaller.ai
3. Check page source for version indicators
4. Verify HTTPS certificate is issued

## How to Tell Which Repository is Serving

### Old Repository Indicators:
- Older version numbers (v2.1.3 or earlier)
- Missing CLI demo section
- Different layout or content

### New Repository Indicators:
- Latest version (v2.1.4+)
- CLI demo section on homepage
- Updated content from recent PRs

## Troubleshooting

**If site still shows old content:**
1. Clear browser cache (Ctrl+Shift+R or Cmd+Shift+R)
2. Wait longer for DNS propagation
3. Verify GitHub Pages is enabled in new repo
4. Check custom domain is configured correctly

**If site is down:**
1. Check GitHub Pages deployment status
2. Verify DNS records are correct
3. Check custom domain verification in GitHub

## Cloudflare DNS Checklist

- [ ] Apex domain (`reccaller.ai`) has 4 A records (not CNAME)
- [ ] A records point to GitHub Pages IPs (185.199.108.153, etc.)
- [ ] Proxy status for A records is "DNS only" (gray cloud)
- [ ] WWW subdomain (if used) has CNAME to `reccaller-ai.github.io`
- [ ] `contexts` subdomain correctly configured (already done ✅)

