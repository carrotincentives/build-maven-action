# Build and Push Docker Image GitHub Action

A reusable GitHub Action to build Java applications with Maven, create a Docker image, and push it to your Docker registry.

## Overview

This GitHub Action automates the following tasks:

1. **Repository Checkout:** Clones your repository at a specified Git reference (branch, tag, or commit hash).
2. **Java & Maven Setup:** Configures the Java environment (using the specified distribution and version) and Maven.
3. **Maven Build:** Builds your Java application using Maven.
4. **Docker Registry Login:** Authenticates to your Docker registry.
5. **Docker Build & Push:** Builds a Docker image based on your provided Dockerfile, tags it with the specified version, and pushes it to your Docker registry.


## Inputs

| Input Name         | Description                                                                                             | Required | Default                  |
| ------------------ | ------------------------------------------------------------------------------------------------------- | -------- | ------------------------ |
| `checkout_ref`     | Git reference to checkout (branch, tag, or commit hash).                                                | Yes      |                          |
| `java_version`     | Java version to use.                                                                                    | Yes      |                          |
| `java_distribution`| Java distribution to use (e.g., `zulu`).                                                                | No       | `zulu`                   |
| `maven_version`    | Maven version to use.                                                                                   | Yes      |                          |
| `maven_options`    | Maven options for building the project.                                                                 | No       | `-U clean package -DskipTests` |
| `docker_context`   | Path to the Docker build context.                                                                       | No       | `.`                      |
| `dockerfile_path`  | Path to the Dockerfile.                                                                                 | No       | `./Dockerfile`           |
| `image_name`       | Name of the Docker image.                                                                               | Yes      |                          |
| `build_version`    | Version to tag the Docker image with.                                                                   | Yes      |                          |
| `registry_url`     | URL of the Docker registry.                                                                             | Yes      |                          |
| `registry_username`| Username for the Docker registry.                                                                       | Yes      |                          |
| `registry_password`| Password for the Docker registry.                                                                       | Yes      |                          |

## Example Workflow

```yaml
name: Build, Test, and Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build and Push Docker Image
        uses: carrotincentives/build-maven-action@v1
        with:
          # Repository Checkout
          checkout_ref: 'main'

          # Java and Maven Configuration
          java_version: '11'
          java_distribution: 'zulu'
          maven_version: '3.8.4'
          maven_options: '-U clean package -DskipTests'

          # Docker Build & Tagging
          dockerfile_path: './Dockerfile'
          image_name: 'my-java-app'
          build_version: 'v1.0.0'

          # Docker Registry Credentials
          registry_url: 'docker.io'
          registry_username: 'your-username'
          registry_password: 'your-password'
```
