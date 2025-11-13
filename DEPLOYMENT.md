# Coolify Deployment Guide

This guide will help you deploy the Odd Omens Documentation site to your Coolify server using GitHub.

## Prerequisites

- Coolify server set up and running
- GitHub repository connected to Coolify
- Docker support enabled on Coolify

## Deployment Steps

### 1. Push to GitHub

Make sure all your changes are committed and pushed:

```bash
git add .
git commit -m "Add Coolify deployment configuration"
git push origin main
```

### 2. Configure in Coolify

1. Log into your Coolify dashboard
2. Click "New Resource" → "Application"
3. Select "Public Repository" and enter: `https://github.com/OddOmens/Documentation.git`
4. Choose the branch (usually `main`)
5. Set the build pack to "Dockerfile"
6. Configure the following settings:
   - **Port**: 80
   - **Build Command**: (leave empty, handled by Dockerfile)
   - **Start Command**: (leave empty, handled by Dockerfile)

### 3. Environment Variables (Optional)

No environment variables are required for this static site.

### 4. Domain Configuration

1. In Coolify, go to your application settings
2. Add your custom domain (e.g., `docs.oddomens.com`)
3. Enable automatic SSL/TLS certificate

### 5. Deploy

Click "Deploy" in Coolify. The deployment process will:
1. Clone your repository
2. Build the Docker image using the Dockerfile
3. Build the VitePress site
4. Serve it with Nginx
5. Make it available on your configured domain

## Build Process

The Dockerfile uses a multi-stage build:
- **Stage 1**: Builds the VitePress site using Node.js
- **Stage 2**: Serves the static files using Nginx

## Automatic Deployments

To enable automatic deployments on push:
1. In Coolify, go to your application settings
2. Enable "Automatic Deployment"
3. Select the branch to watch (e.g., `main`)
4. Every push to that branch will trigger a new deployment

## Troubleshooting

### Build fails
- Check Coolify logs for specific error messages
- Ensure all dependencies are listed in package.json
- Verify the Dockerfile syntax

### Site not loading
- Check that port 80 is correctly exposed
- Verify nginx configuration
- Check Coolify application logs

### 404 errors on routes
- The nginx.conf includes SPA fallback configuration
- Ensure the nginx.conf file is being copied correctly

## Local Testing

Test the Docker build locally before deploying:

```bash
# Build the image
docker build -t docs-test .

# Run the container
docker run -p 8080:80 docs-test

# Visit http://localhost:8080
```

## Support

For issues specific to:
- **Coolify**: Check Coolify documentation or Discord
- **VitePress**: See VitePress documentation
- **This project**: Open an issue on GitHub
