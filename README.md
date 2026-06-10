# Sneha Suresh — Portfolio

A polished single-page portfolio built with HTML and CSS, designed for AWS static hosting, Docker containerization, and automated CI/CD deployment.

![Docker CI Pipeline](https://github.com/sneh20047078/My_Portfolio/actions/workflows/docker-ci.yml/badge.svg)

---

## Live URLs

- **CloudFront (Task 1):** https://d30ll7dxewuby5.cloudfront.net
- **S3 Website Endpoint (Task 1):** http://snehasuresh-portfolio-022266408605-ap-south-1-an.s3-website.ap-south-1.amazonaws.com
- **Docker Hub (Task 3):** https://hub.docker.com/r/snehsp/portfolio-website

---

## GitHub Repository

> https://github.com/sneh20047078/My_Portfolio

---

## Tech Stack

**Frontend**
- HTML5 / CSS3
- Google Fonts: Syne + DM Sans
- FontAwesome 6 icons
- Responsive layout with hero, about, projects, and contact sections
- Social links for GitHub, LinkedIn, and X

**Cloud & DevOps (Tasks 1–3)**
- AWS S3 — static website hosting
- AWS CloudFront — CDN and HTTPS
- AWS EC2 — Docker container deployment
- Docker + Nginx — containerized portfolio
- Docker Hub — image registry
- GitHub Actions — CI/CD pipeline (build and push on every push to `main`)
- Git — version control and GitHub hosting

---

## Task 1 — Static Hosting on AWS S3 + CloudFront

### Deployment Steps

#### 1. Local development

```bash
git init
git add .
git commit -m "Initial portfolio website"
```

#### 2. Push to GitHub

```bash
git remote add origin https://github.com/sneh20047078/My_Portfolio.git
git push -u origin main
```

#### 3. Create S3 bucket

1. Open AWS S3 and create a new bucket: `snehasuresh-portfolio-022266408605-ap-south-1-an`
2. Disable block public access.
3. Enable **Static website hosting** → Index document: `index.html`.

#### 4. Add bucket policy for public read access

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

#### 5. Upload files to S3

```bash
aws s3 cp index.html s3://snehasuresh-portfolio-022266408605-ap-south-1-an/
aws s3 cp styles.css s3://snehasuresh-portfolio-022266408605-ap-south-1-an/
```

#### 6. Create CloudFront distribution

1. Origin domain: S3 website endpoint (not the bucket ARN)
2. Origin protocol policy: **HTTP Only**
3. Viewer protocol policy: **Redirect HTTP to HTTPS**
4. Default root object: `index.html`
5. Minimum TLS: `TLSv1.2_2021`
6. Create and wait for deployment

#### 7. Test CloudFront

https://d30ll7dxewuby5.cloudfront.net

### Screenshots

**1. CloudFront distribution — S3 origin configured**
[![CloudFront distribution — S3 origin configured](assets/Screenshot%202026-05-30%20214915.png)](https://github.com/sneh20047078/My_Portfolio/blob/main/assets/Screenshot%202026-05-30%20214915.png)

**2. S3 bucket — objects in portfolio bucket**
[![S3 bucket — objects in portfolio bucket](assets/Screenshot%202026-05-30%20214953.png)](https://github.com/sneh20047078/My_Portfolio/blob/main/assets/Screenshot%202026-05-30%20214953.png)

**3. Live portfolio on CloudFront**
[![Live portfolio on CloudFront](assets/Screenshot%202026-05-30%20213339.png)](https://github.com/sneh20047078/My_Portfolio/blob/main/assets/Screenshot%202026-05-30%20213339.png)

---

## Task 2 — Docker Containerization on AWS EC2

This repository includes a `Dockerfile` to run the portfolio inside an Nginx container.

### Dockerfile

```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```

### Build and run locally

```bash
docker build -t portfolio-website .
docker run -d -p 8080:80 portfolio-website
```

Open your browser at `http://localhost:8080`

### EC2 Deployment

1. Provision an AWS EC2 instance (t3.micro, Amazon Linux 2023, ap-south-1)
2. Install Docker on the instance
3. Copy project files to the VM
4. Build and run the container:

```bash
docker build -t portfolio-website .
docker run -d -p 80:80 portfolio-website
```

5. Open the EC2 public IP in a browser

---

### Screenshots

**1. Docker image build — portfolio-website**
[![Docker image build](assets/Screenshot%202026-06-01%20215722.png)](https://github.com/sneh20047078/My_Portfolio/blob/main/assets/Screenshot%202026-06-01%20215722.png)

**2. Live portfolio on EC2 public IP**
[![Live portfolio on EC2](assets/Screenshot%202026-06-06%20165100.png)](https://github.com/sneh20047078/My_Portfolio/blob/main/assets/Screenshot%202026-06-06%20165100.png)

**3. EC2 instance running — My portfolio**
[![EC2 instance running](assets/Screenshot%202026-06-06%20171134.png)](https://github.com/sneh20047078/My_Portfolio/blob/main/assets/Screenshot%202026-06-06%20171134.png)

---

## Task 3 — CI/CD Automation with GitHub Actions

This repository includes a GitHub Actions CI/CD pipeline that automatically builds and pushes the Docker image to Docker Hub on every push to the `main` branch — no manual build or push needed.

### Workflow file: `.github/workflows/docker-ci.yml`

```yaml
name: Docker CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  docker-build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker Image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/portfolio-website:latest .

      - name: Push Docker Image
        run: docker push ${{ secrets.DOCKER_USERNAME }}/portfolio-website:latest
```

### How it works

| Step | Action |
|------|--------|
| Trigger | Every `git push` to `main` branch |
| Runner | GitHub-hosted Ubuntu (ubuntu-latest) |
| Checkout Code | Downloads repo source onto the runner VM |
| Login to Docker Hub | Authenticates via GitHub Secrets (never hardcoded) |
| Build Docker Image | Builds and tags as `snehsp/portfolio-website:latest` |
| Push Docker Image | Uploads image to Docker Hub automatically |

### GitHub Secrets configured

| Secret | Purpose |
|--------|---------|
| `DOCKER_USERNAME` | Docker Hub username (`snehsp`) |
| `DOCKER_PASSWORD` | Docker Hub Access Token (Read, Write, Delete) |

### Pull and run from Docker Hub

```bash
docker pull snehsp/portfolio-website:latest
docker run -d -p 8080:80 snehsp/portfolio-website:latest
```

**Docker Hub:** https://hub.docker.com/r/snehsp/portfolio-website

### Screenshots

**1. GitHub Secrets — Docker Hub credentials stored in GitHub Secrets**
![GitHub Secrets](assets/task3-secrets.png)

**2. GitHub Actions — successful workflow run on the main branch**
![GitHub Actions Runs](assets/task3-actions-runs.png)

**3. Workflow summary — build and push completed successfully**
![Run Summary](assets/task3-run-summary.png)

**4. Pipeline steps — checkout, login, build, and push all passed**
![Pipeline Steps](assets/task3-pipeline-steps.png)

**5. Docker Hub — latest image tag pushed successfully**
![Docker Hub Tags](assets/task3-dockerhub-tags.png)

**6. Docker Hub image details — 24.83 MB, linux/amd64**
![Docker Hub Detail](assets/task3-dockerhub-detail.png)

**7. Local validation — container running on localhost:8080**
![Local Validation](assets/task3-local-validation.png)

---

## Project Structure

```
My_Portfolio/
├── index.html
├── styles.css
├── Dockerfile
├── .dockerignore
├── README.md
├── assets/
│   └── (screenshots)
└── .github/
    └── workflows/
        └── docker-ci.yml
```

---

## What this portfolio includes

- Hero section with CTA buttons
- About section with skills and description
- 3 project cards with GitHub icons
- Contact/footer section with email button and social links
- Smooth scrolling and responsive layout

---

## Notes

> EC2 instance stopped due to AWS cost.

---

*© 2026 Sneha Suresh*
