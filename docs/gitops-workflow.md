
# GitOps Deployment Workflow

## Overview

This project demonstrates a CI/CD workflow that combines
automated application builds with GitOps-based deployment
configuration.

## Workflow

1. A developer pushes application changes to GitHub.
2. Jenkins checks out the source code.
3. Maven builds the Java application.
4. Docker packages the application into a container image.
5. Jenkins publishes the image to Amazon ECR.
6. The Kubernetes deployment configuration references the
   appropriate image in Amazon ECR.
7. Argo CD monitors the Git repository for configuration changes.
8. Argo CD synchronizes the desired configuration with Amazon EKS.

## Responsibilities

- Jenkins: automates build and image-publishing tasks.
- Amazon ECR: stores container images.
- Kubernetes: manages application containers.
- Argo CD: synchronizes Kubernetes configuration from Git.
- Amazon EKS: provides the managed Kubernetes environment.

## Important Implementation Note

Publishing a new image to Amazon ECR does not automatically
update the Kubernetes deployment configuration.

The image reference in Git must also be updated, or another
configured mechanism must update it, before Argo CD can deploy
the new image.

This repository contains example configurations for portfolio
demonstration. A complete end-to-end deployment has not yet
been verified.
