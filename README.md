# Build and Push Image to ECR Action

## Description
This GitHub Action is designed for building and pushing Docker images to Amazon Elastic Container Registry (ECR). It assumes an AWS role based on the environment, installs dependencies, sets up QEMU and Docker Buildx for cross-platform builds, logs into ECR, and then builds and pushes the Docker image.

## Workflow Steps

1. **Assume Role for Environment**: Assumes an appropriate AWS role for the specified environment.

2. **Install Dependencies**: Installs PHP dependencies using Composer. This step uses SSH keys for accessing private repositories if required.

3. **Set up QEMU**: Sets up QEMU to enable cross-platform Docker builds, which is essential for building images for multiple architectures like `amd64` and `arm64`.

4. **Set up Docker Buildx**: Sets up Docker Buildx for building and pushing Docker images.

5. **Login to Amazon ECR**: Logs into Amazon ECR using AWS credentials, preparing for image push.

6. **Build and Push**: Builds the Docker image and pushes it to Amazon ECR with specified tags. It builds for `linux/amd64` and `linux/arm64` platforms and tags the image with both the specific tag and the environment name.

## Usage

To use this action in your GitHub workflow, include it as a step in your job, and ensure to provide the necessary inputs and set the required environment variables. Here's an example:

```yaml
jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Build and Push Image to ECR
        uses: ./.github/actions/build-and-push-ecr
        with:
          environment: 'prod' # Specify the target environment
        env:
          REGISTRY: 'your-ecr-registry-name'
          TAG: 'your-image-tag'
          ENVIRONMENT: 'prod'
```

## Pre-requisites

SSH keys (GA_SSH_KEY and GA_SSH_PUB) must be configured as secrets in your repository if private repository access is required during the build.
ECR registry (REGISTRY), image tag (TAG), and environment (ENVIRONMENT) should be correctly set as environment variables.