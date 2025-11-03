# RecCall Website

This repository contains the RecCall website (https://reccaller.ai).

## 🚀 Deployment

The website is automatically deployed to GitHub Pages on every push to `main` branch.

### Manual Deployment

```bash
# Push changes to main
git push origin main
```

GitHub Actions will automatically build and deploy to GitHub Pages.

## 📁 Structure

```
.
├── index.html              # Homepage
├── pages/                  # Additional pages
│   ├── getting-started.html
│   ├── how-it-works.html
│   ├── integrations.html
│   ├── use-cases.html
│   └── ...
├── assets/                 # Static assets
│   ├── css/
│   └── js/
├── CNAME                   # Custom domain configuration
└── README.md              # This file
```

## 🔗 Links

- Live Site: https://reccaller.ai
- Main Repository: https://github.com/reccaller-ai/reccall
- Issues: https://github.com/reccaller-ai/websites/issues

## 📝 Contributing

1. Make changes to HTML/CSS files
2. Test locally by opening HTML files in browser
3. Commit and push to `main` branch
4. GitHub Pages will automatically deploy

## 🔄 Migration Notes

This repository was created by moving the website from the main `reccall` repository to maintain separation of concerns. The website deployment is now independent of the main RecCall application.
