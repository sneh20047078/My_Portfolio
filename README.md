# Static Portfolio — AWS S3 + CloudFront

A polished single-page portfolio built with HTML and CSS, designed for AWS static hosting and CDN deployment.

## Live URL
> **CloudFront:** https://d30ll7dxewuby5.cloudfront.net

> **S3 Website Endpoint:** http://snehasuresh-portfolio-022266408605-ap-south-1-an.s3-website.ap-south-1.amazonaws.com

---

## GitHub Repository
> https://github.com/sneh20047078/My_Portfolio

---

## Project Files
- `index.html`
- `styles.css`
- `README.md`

---

## Tech Stack
- HTML5 / CSS3
- Google Fonts: Syne + DM Sans
- FontAwesome 6 icons
- Responsive layout with hero, about, projects, and contact sections
- Social links for GitHub, LinkedIn, and X

---

## Deployment Steps

### 1. Local development
```bash
git init
git add .
git commit -m "Initial portfolio website"
```

### 2. GitHub
```bash
git remote add origin https://github.com/sneh20047078/My_Portfolio.git
git push -u origin main
```

### 3. Create S3 bucket
1. Open AWS S3 and create a new bucket: `snehasuresh-portfolio-022266408605-ap-south-1-an`
2. Disable block public access.
3. Enable **Static website hosting** → Index document: `index.html`.

### 4. Add bucket policy for public read access
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::snehasuresh-portfolio-022266408605-ap-south-1-an/*"
    }
  ]
}
```

### 5. Upload files to S3
```bash
aws s3 cp index.html s3://snehasuresh-portfolio-022266408605-ap-south-1-an/
aws s3 cp styles.css s3://snehasuresh-portfolio-022266408605-ap-south-1-an/
```

### 6. Verify S3 website endpoint
Open: `http://snehasuresh-portfolio-022266408605-ap-south-1-an.s3-website.ap-south-1.amazonaws.com`

### 7. Create CloudFront distribution
1. Origin domain: S3 website endpoint (not the bucket ARN)
2. Origin protocol policy: **HTTP Only**
3. Viewer protocol policy: **Redirect HTTP to HTTPS**
4. Default root object: `index.html`
5. Minimum TLS: `TLSv1.2_2021`
6. Create and wait for deployment

### 8. Test CloudFront
```
https://d30ll7dxewuby5.cloudfront.net
```

## What this portfolio includes
- Hero section with CTA buttons
- About section with skills and description
- 3 project cards with GitHub icons
- Contact/footer section with email button and social links
- Smooth scrolling and responsive layout

---


## Screenshots

- **CloudFront distribution (Deployed):**
  ![CloudFront status](assets/images/cloudfront.png)
- **S3 bucket policy:**
  ![S3 policy](assets/images/s3-policy.png)
- **Live site (HTTPS):**
  ![Live site](assets/images/live-site.png)

Add screenshots locally then push to GitHub:

PowerShell:
```powershell
cd "C:\Users\PC\Documents\AWS1"
mkdir -Force assets\images
Copy-Item "C:\path\to\cloudfront.png" assets\images\cloudfront.png
Copy-Item "C:\path\to\s3-policy.png" assets\images\s3-policy.png
Copy-Item "C:\path\to\live-site.png" assets\images\live-site.png
git add assets/images/* README.md
git commit -m "Add screenshots to README"
git push
```

Git Bash / WSL:
```bash
cd /c/Users/PC/Documents/AWS1
mkdir -p assets/images
cp /c/path/to/cloudfront.png assets/images/cloudfront.png
cp /c/path/to/s3-policy.png assets/images/s3-policy.png
cp /c/path/to/live-site.png assets/images/live-site.png
git add assets/images/* README.md
git commit -m "Add screenshots to README"
git push
```

*© 2026 Sneha Suresh*
