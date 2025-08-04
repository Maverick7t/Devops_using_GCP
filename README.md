
# CI/CD Pipeline with Cloud Build & Cloud Deploy for GKE

This project demonstrates a full CI/CD setup on Google Cloud using native services:
- Google Kubernetes Engine (GKE) for container orchestration
- Cloud Build for building Docker images and triggering workflows
- Cloud Deploy for managing multi-stage deployments with approvals

## Project Highlights

- Automated deployments from GitHub to GKE clusters
- Separate environments for dev and prod
- Manual approval before production deployment
- Two microservices (App1 and App2) deployed with separate configurations

## Workflow Overview

1. Developer pushes code to GitHub
2. Cloud Build trigger fires
3. Docker images for App1 and App2 are built and pushed to Artifact Registry
4. Cloud Deploy pipeline is created or updated
5. Deployment is made to the Dev GKE cluster
6. Manual approval is required to promote to Prod
7. Upon approval, Cloud Deploy promotes the release to the Prod GKE cluster

## Applications

- App1: Prints "Hello World App 1" on port 8080
- App2: Prints "Hello World App 2" on port 8081

## Architecture

GitHub Push
   |
   v
Cloud Build Trigger
   |
   |-- Build Docker images
   |-- Push to Artifact Registry
   |-- Create/Update Cloud Deploy pipeline
   |-- Deploy to Dev cluster
   |
   v
Manual Approval (for Prod)
   |
   v
Deploy to Prod cluster

## Directory Structure

.
├── app1/
│   └── Dockerfile
├── app2/
│   └── Dockerfile
├── deploy/
│   ├── pipeline.yaml        # Defines Cloud Deploy pipeline
│   ├── dev.yaml             # Target: Dev cluster
│   ├── prod.yaml            # Target: Prod cluster
│   └── app-release.yaml     # Manifest for both apps
└── cloudbuild.yaml          # Build instructions

## GCP Services Used

- GKE (2 clusters: dev and prod)
- Cloud Build (triggers and automation)
- Cloud Deploy (delivery pipeline and approval management)
- Artifact Registry (stores Docker images)

## Steps to Deploy

1. Create two GKE clusters (dev and prod)
2. Connect your GitHub repo to Cloud Build
3. Enable required GCP APIs:
   - Kubernetes Engine API
   - Artifact Registry API
   - Cloud Build API
   - Cloud Deploy API
4. Push your code to GitHub
5. Monitor Cloud Build logs and Cloud Deploy pipeline
6. Approve promotion to production via Cloud Deploy console

## Required IAM Roles

Ensure the Cloud Build service account has these roles:
- roles/cloudbuild.builds.editor
- roles/clouddeploy.admin
- roles/container.developer

## Troubleshooting

- If the deployment fails with permission errors, verify that the correct service account is used by the Cloud Build trigger.
- If manual approval isn't working, check the `requireApproval: true` setting in your `prod.yaml`.
- Make sure Docker images are successfully pushed to Artifact Registry before release creation.

## Summary

This CI/CD setup gives you a production-grade pipeline using only Google Cloud services. It handles everything from code push to production deployment with manual review gates. Easily extensible for more microservices, environments, or workflows.
