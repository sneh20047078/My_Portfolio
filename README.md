# Static Portfolio — AWS S3 + CloudFront

A polished single-page portfolio built with HTML and CSS, designed for AWS static hosting and CDN deployment.

## Live URL
> `https://<your-cloudfront-id>.cloudfront.net`

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
Push this repo to GitHub so version history is visible.

### 3. Create S3 bucket
1. Open AWS S3 and create a new bucket.
2. Use a unique name like `sneha-portfolio`.
3. Disable block public access for the bucket so static website hosting can work.
4. Enable **Static website hosting** and set the index document to `index.html`.

### 4. Add public read policy
Use a bucket policy like this under **Permissions → Bucket Policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

### 5. Upload files
```bash
aws s3 sync . s3://your-bucket-name --exclude ".git/*"
```

### 6. Verify S3 hosting
Open the S3 website endpoint shown in the bucket hosting settings.

### 7. Create CloudFront distribution
1. In CloudFront, create a distribution using the S3 website endpoint as the origin.
2. Set **Redirect HTTP to HTTPS**.
3. Set **Default root object** to `index.html`.
4. Choose TLSv1.2_2021 or later.

### 8. Optional custom domain
- Request an ACM certificate in `us-east-1`.
- Configure CloudFront with your domain and add DNS records in Route 53.

---

## What this portfolio includes
- Hero section with CTA buttons
- About section with skills and description
- 3 project cards with GitHub icons
- Contact/footer section with email button and social links
- Smooth scrolling and responsive layout

---


*© 2026 Sneha Suresh*
