# docker-ci-pipeline-template

Reusable GitHub Actions pipeline for building applications, creating Docker images, and pushing them to Docker Hub.

## Design Notes

### Reusability Across Multiple Repositories

This pipeline template centralizes CI logic into a single reusable workflow.  
Instead of copying build and Docker steps into every repository:

- The same workflow can be reused across multiple repositories
- Only application-specific values are passed as inputs
- CI behavior remains consistent across projects

Any updates to the CI process can be made once in this template and will automatically apply to all consumer repositories.

### Onboarding a New Application Repository

To onboard a new application repository to use this reusable pipeline:

1. Ensure the application repository contains:
   - A valid build command
   - A Dockerfile

2. Create a Docker Hub repository for the application image.

3. Add the following GitHub Secrets to the application repository:
   - `DOCKER_USERNAME`
   - `DOCKER_PASSWORD`

4. Create a GitHub Actions workflow file at:
.github/workflows/ci.yml


5. Configure the workflow to call this reusable pipeline and pass:
- Docker image name
- Build command
- Dockerfile path

6. Push the changes to the default branch.

After setup, every push to the default branch will automatically:
- Build the application
- Build the Docker image
- Push the image to Docker Hub
