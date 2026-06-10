[![Docker CI Pipeline](https://github.com/sneh20047078/My_Portfolio/actions/workflows/docker-ci.yml/badge.svg)](https://github.com/sneh20047078/My_Portfolio/actions/workflows/docker-ci.yml)

# Static Portfolio — AWS S3 + CloudFront + EC2 Docker

A polished single-page portfolio built with HTML and CSS, designed for AWS static hosting, CDN distribution, and Docker deployment on an EC2 VM.

## Live URL
> **CloudFront:** https://d30ll7dxewuby5.cloudfront.net

> **S3 Website Endpoint:** http://snehasuresh-portfolio-022266408605-ap-south-1-an.s3-website.ap-south-1.amazonaws.com

> **Live EC2 Docker Deployment:** http://13.232.79.210

> **Deployment status:** EC2 Docker deployment is running and serving the portfolio on the public IP above.

---

## GitHub Repository
> https://github.com/sneh20047078/My_Portfolio

---

## Project Files
- `index.html`
- `styles.css`
- `Dockerfile`
- `.dockerignore`
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

## Docker Deployment (Task 2)
This repository now includes a `Dockerfile` to run the portfolio inside an Nginx container.

### Build the Docker image
```bash
docker build -t portfolio-website .
```

### Run the website locally in Docker
```bash
docker run -d -p 8080:80 portfolio-website
```

Open your browser at:
```text
http://localhost:8080
```

### Cloud VM deployment
1. Provision a Linux VM (AWS EC2, Azure VM, Google Cloud VM).
2. Install Docker on the VM.
3. Copy the project files to the VM.
4. Build and run the container on the VM:
```bash
docker build -t portfolio-website .
docker run -d -p 80:80 portfolio-website
```
5. Open the VM public IP in a browser.

This portfolio is already deployed on EC2 at:
```text
http://13.232.79.210
```

## What this portfolio includes
- Hero section with CTA buttons
- About section with skills and description
- 3 project cards with GitHub icons
- Contact/footer section with email button and social links
- Smooth scrolling and responsive layout

---

## Screenshots

### Task 1: CloudFront + S3
![CloudFront status](assets/Screenshot%202026-05-30%20214915.png)
![S3 policy](assets/Screenshot%202026-05-30%20214953.png)
![Task 1 live portfolio](assets/Screenshot%202026-05-30%20213339.png)

### Task 2: EC2 Docker deployment
![Task 2 Docker run](assets/Screenshot%202026-06-01%20215722.png)
![Task 2 live site verification](assets/Screenshot%202026-06-06%20165100.png)
![Task 2 container status](assets/Screenshot%202026-06-06%20171134.png)

---

*© 2026 Sneha Suresh*
