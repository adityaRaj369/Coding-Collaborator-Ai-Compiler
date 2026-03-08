# Deployment Guide - Coding Collaborator Server

This guide explains how to Dockerize and deploy your Node.js backend to the cloud using GitHub Actions.

## 📋 Prerequisites

- Docker installed locally (for testing)
- GitHub repository with Actions enabled
- Selected cloud provider account
- Environment variables/secrets configured

## 🐳 Docker Setup

### Local Testing

```bash
# Build the Docker image
docker build -t coding-collaborator-server ./server

# Run the container
docker run -p 5000:5000 \
  -e MONGO_URI="your-mongo-uri" \
  -e GOOGLE_API_KEY="your-api-key" \
  coding-collaborator-server
```

### Using Docker Compose

```bash
# Copy .env.example to .env and update values
cp .env.example .env

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f server

# Stop services
docker-compose down
```

## 🚀 GitHub Actions Deployment

### Step 1: Set GitHub Secrets

Go to **Settings → Secrets and variables → Actions** and add:

#### Required for all options:
- `MONGO_URI` - MongoDB connection string
- `GOOGLE_API_KEY` - Google Generative AI API key

#### For Google Cloud Run:
```bash
DEPLOY_TO=cloud-run  # Set this as a variable in Settings → Variables
```

Plus these secrets:
- `GCP_PROJECT_ID` - Your GCP project ID
- `GCP_SA_KEY` - Service account key (JSON as secret)

#### For AWS ECS:
```bash
DEPLOY_TO=aws-ecs  # Set this as a variable
```

Plus:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`

#### For DigitalOcean:
```bash
DEPLOY_TO=digitalocean  # Set this as a variable
```

Plus:
- `DO_API_TOKEN`
- `DO_APP_ID`

#### For VPS:
```bash
DEPLOY_TO=vps  # Set this as a variable
```

Plus:
- `VPS_HOST` - IP address or domain
- `VPS_USER` - SSH username
- `VPS_DEPLOY_KEY` - Private SSH key

---

## ☁️ Cloud Provider Configuration

### Option 1: Google Cloud Run (Recommended - Simplest)

#### Setup:
1. Create a GCP project at [console.cloud.google.com](https://console.cloud.google.com)
2. Enable Cloud Run and Container Registry APIs
3. Create a Service Account:
   - Go to IAM & Admin → Service Accounts
   - Create service account with "Cloud Run Admin" role
   - Create JSON key and add as `GCP_SA_KEY` secret

#### Workflow Features:
- Auto-scaling (0 to 100 instances)
- Pay-per-use pricing
- Built-in HTTPS
- Easy environment variables

#### Deploy:
```bash
# After pushing to main, workflow runs automatically
# View deployment: https://console.cloud.google.com/run
```

---

### Option 2: AWS ECS (Scalable)

#### Setup:
1. Create ECS cluster and task definition
2. Create Docker image repository in ECR
3. Create IAM user with ECS and ECR permissions
4. Add AWS credentials as GitHub secrets

#### Service Configuration:
- 2 vCPU, 4GB memory recommended
- Load balancer for traffic distribution
- Auto-scaling group (1-10 instances)

#### Deploy:
```bash
# Requires additional AWS CLI setup in workflow
# See AWS documentation for detailed ECS deployment
```

---

### Option 3: DigitalOcean App Platform

#### Setup:
1. Create DigitalOcean account and create an app
2. Generate API token (Settings → API → Tokens)
3. Add token as `DO_API_TOKEN` secret
4. Get app ID and add as `DO_APP_ID` secret

#### Features:
- Simple YAML-based deployment
- Built-in CI/CD integration
- Default HTTPS and custom domains
- $12/month minimum app tier

---

### Option 4: Deploy to Your VPS

#### Setup:
1. Generate SSH keypair:
   ```bash
   ssh-keygen -t rsa -b 4096 -f deploy_key -N ""
   ```
2. Add public key to VPS `~/.ssh/authorized_keys`
3. Add private key as `VPS_DEPLOY_KEY` secret
4. Add `VPS_HOST` and `VPS_USER` variables
5. SSH into VPS and create `/app` directory with `docker-compose.yml`

#### VPS docker-compose.yml:
```yaml
version: '3.8'
services:
  server:
    image: ghcr.io/YOUR_USERNAME/coding-collaborator-ai-compiler/server:latest
    ports:
      - "5000:5000"
    environment:
      - MONGO_URI=${MONGO_URI}
      - GOOGLE_API_KEY=${GOOGLE_API_KEY}
      - NODE_ENV=production
    restart: unless-stopped
```

---

## 📊 Workflow Behavior

The GitHub Actions workflow runs on:
- **Push to main** → Builds image and deploys
- **Pull requests** → Only builds (no deployment)
- **Changes to server/** folder → Triggers workflow

### Build Process:
1. ✅ Checkout code
2. 🐳 Build Docker image (multi-stage for optimization)
3. 📦 Push to GitHub Container Registry (ghcr.io)
4. 🚀 Deploy to selected cloud provider

### Image Tags:
```
ghcr.io/YOUR_USERNAME/coding-collaborator-ai-compiler/server:latest
ghcr.io/YOUR_USERNAME/coding-collaborator-ai-compiler/server:main
ghcr.io/YOUR_USERNAME/coding-collaborator-ai-compiler/server:sha-abc123
```

---

## 🔒 Environment Variables

### Required Secrets:
```env
MONGO_URI=mongodb+srv://user:pass@cluster.mongodb.net/dbname
GOOGLE_API_KEY=your-api-key-here
```

### Optional:
```env
NODE_ENV=production
LOG_LEVEL=info
```

---

## ✅ Verification

### Check deployment status:

**Google Cloud Run:**
```bash
gcloud run services list --platform=managed
gcloud run services describe coding-collaborator-server --platform=managed
```

**View logs:**
```bash
gcloud run services logs read coding-collaborator-server --limit=50
```

**Test the endpoint:**
```bash
curl https://coding-collaborator-server-xxxxx.a.run.app/health
```

---

## 🛠️ Troubleshooting

### Docker build fails:
```bash
# Test locally first
cd server
docker build -t test .
docker run -e MONGO_URI=xxx test
```

### Deployment not starting:
1. Check GitHub Actions logs in the workflow tab
2. Verify all secrets are set correctly
3. Check cloud provider logs
4. Ensure environment variables are accessible

### Performance issues:
- Increase memory/CPU allocation
- Enable caching in the workflow
- Use a CDN for static assets
- Monitor database performance

---

## 📈 Monitoring & Scaling

### Google Cloud Run:
- Auto-scales based on traffic
- Monitor via Cloud Console
- Set max instances to control costs

### Self-hosted VPS:
- Use Prometheus + Grafana for monitoring
- Configure load balancer for multiple instances
- Use nginx as reverse proxy

---

## 🔄 CI/CD Best Practices

1. **Always test locally first**
   ```bash
   docker build -t test ./server
   docker run test npm test
   ```

2. **Use semantic versioning for tags**
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

3. **Monitor deployment logs**
   - Check GitHub Actions tab
   - Check cloud provider logs
   - Set up alerts for failures

4. **Keep secrets secure**
   - Never commit `.env` files
   - Rotate secrets regularly
   - Use minimal IAM permissions

---

## 🎯 Next Steps

1. Choose your deployment target
2. Set up GitHub secrets (see above)
3. Update `DEPLOY_TO` variable in Actions settings
4. Push changes and monitor the workflow
5. Verify deployment and test endpoints

Happy deploying! 🚀
