# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Pravin Mishra's portfolio website** — a static HTML/CSS site deployed to AWS S3 + CloudFront via GitHub Actions. Designed for DevOps Micro Internship (DMI) Week 1 as a teaching example of deploying static sites on Linux/Nginx, but now hosted on AWS with OIDC-based CI/CD.

## Architecture

### Application (Static Site)
- **index.html** (613 lines) — Single-page portfolio with navbar, hero, about, services, books, courses, contact sections
- **style.css** (1145 lines) — All styling, mobile-first responsive (breakpoints: 900px, 768px, 600px); uses CSS Grid and Flexbox
- **privacy.html / terms.html** — Standalone pages with inline styles
- **images/** — Static assets (logo, profile photos, course thumbnails, backgrounds)
- Pure HTML5 + CSS3, no JavaScript, no build step, no dependencies
- Hamburger menu for mobile navigation

### Deployment
- **CI/CD (.github/workflows/deploy.yml)** — Triggers on push to `main` (GitHub Actions)
  - Uses AWS OIDC for keyless authentication (assumes IAM role: `arn:aws:iam::533267262133:role/github-actions-deploy`)
  - Syncs all files to S3 bucket `pravinmishradmi-site-production` (excludes `.git`, `.github`, `.claude`, `*.md`)
  - Invalidates CloudFront cache to serve fresh content immediately
  - AWS region: `eu-north-1`

## Commands

```bash
# Local preview (opens in default browser)
open index.html

# Alternative: use a local web server
python3 -m http.server 8000
# then navigate to http://localhost:8000

# View deployment logs
git log --oneline main
```

## Important Notes

### Ownership Proof (DMI Requirement)
The README.md specifies that students deploying this for DMI Week 1 must edit the footer in **index.html** to add their ownership proof:
- Original: `<p>Crafted with <span>cloud</span> excellence by Pravin Mishra</p>`
- Add a line like: `<p><strong>Deployed by:</strong> [Name] | [Group] | [Date]</p>`
- This must be visible in browser screenshots for assignment submission

### Content Edits
- **Portfolio content** (About, Services, Books, Courses, Contact): Edit **index.html**
- **Styling changes**: Edit **style.css** (well-organized, can search by section comments)
- **Links**: Check external links to `university.pravinmishra.in`, `blog.pravinmishra.com` — update if needed

### Deployment Flow
1. Make changes to HTML/CSS/images
2. Commit to `main` branch
3. GitHub Actions automatically syncs to S3 and invalidates CloudFront
4. Site live within seconds (CloudFront cache invalidation is instant)

## Future Enhancements

These skills exist but are not currently used (aspirational for infrastructure-as-code future):
- `/scaffold-terraform` — Generate Terraform for S3/CloudFront (when ready to manage as code)
- `/deploy` — Manual deployment skill (CI/CD handles this automatically)
- `tf-plan`, `tf-apply` — For future Terraform-managed infrastructure

If infrastructure evolves beyond S3 + CloudFront, migrate deployment definitions to Terraform and use these skills.