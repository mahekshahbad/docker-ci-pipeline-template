Reusable Docker CI Pipeline Template

This repository provides a reusable GitHub Actions CI pipeline that can be used across multiple projects.

It extends a basic CI pipeline by adding code quality checks, security scanning, artifact publishing, and Docker image build & push, all in a single standardized workflow.

What This Template Does

When used by a consumer repository, this pipeline automatically performs:

Code checkout and build setup

Checkstyle (code scanning / linting)

Unit tests with JaCoCo code coverage

SonarQube analysis (optional)

Snyk security scanning (optional)

Application build (JAR)

Publish JAR to JFrog Artifactory (optional)

Build and push Docker image

This ensures code quality, security, and artifact traceability in one CI workflow.

Repository Structure
.github/
└── workflows/
    └── docker-ci-template.yml   # Reusable CI workflow

How to Use This Template in a Project
1. Reference the Template Workflow

In your consumer repository, create a workflow file:

.github/workflows/ci.yml

Example Consumer Workflow
name: Consumer CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  docker-ci:
    uses: <ORG>/<TEMPLATE-REPO>/.github/workflows/docker-ci-template.yml@main

    with:
      image_name: your-dockerhub-username/app-name
      image_tag: 1.0.0
      jar_path: target/app.jar
      build_command: ./mvnw clean package

      sonar_project_key: ${{ vars.SONAR_PROJECT_KEY }}
      sonar_host_url: ${{ vars.SONAR_HOST_URL }}
      sonar_organization: ${{ vars.SONAR_ORGANIZATION }}
      sonar_token: ${{ vars.SONAR_TOKEN }}

      SNYK_TOKEN: ${{ vars.SNYK_TOKEN }}
      JFROG_URL: https://yourcompany.jfrog.io/artifactory

    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      JFROG_USER: ${{ secrets.JFROG_USER }}
      JFROG_PASSWORD: ${{ secrets.JFROG_PASSWORD }}

Required Inputs
Input Name	Description
image_name	Docker image name
image_tag	Docker image tag
jar_path	Path to generated JAR
build_command	Maven build command
Optional Inputs
Input Name	Description
sonar_project_key	SonarQube project key
sonar_host_url	SonarQube server URL
sonar_organization	Sonar organization
sonar_token	Sonar authentication token
SNYK_TOKEN	Snyk API token
JFROG_URL	JFrog Artifactory base URL

Note:
If optional inputs are not provided, the corresponding steps are automatically skipped.

Required Secrets

Configure the following GitHub Secrets in the consumer repository:

Secret Name	Purpose
DOCKER_USERNAME	Docker registry username
DOCKER_PASSWORD	Docker registry password
JFROG_USER	JFrog username
JFROG_PASSWORD	JFrog password
Pipeline Execution Flow

Checkout source code

Setup Java and Maven cache

Run Checkstyle (code scanning)

Run tests and generate JaCoCo coverage

Upload reports as GitHub Actions artifacts

Run SonarQube scan (if configured)

Run Snyk security scan (if configured)

Build application JAR

Publish JAR to JFrog Artifactory

Build and push Docker image

Where to View Results
GitHub Actions

Checkstyle report

JaCoCo coverage report

Complete workflow logs

Snyk Dashboard

Dependency vulnerability results

Docker image security issues

JFrog Artifactory

Maven artifacts (.jar, .pom, metadata)

Versioned snapshot and release builds

Benefits of This Template

Reusable across multiple repositories

Enforces consistent CI standards

Early detection of code quality issues

Early security vulnerability detection

Centralized artifact management

Reduces CI duplication and maintenance effort

Summary

This reusable CI pipeline provides a production-ready DevSecOps workflow that integrates:

Code quality checks

Security scanning

Artifact publishing

Docker image build and push

All achieved using one standardized reusable GitHub Actions template.