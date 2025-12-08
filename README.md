# tesa-app-deploy
 
 ## Overview
 This repository provides reusable GitHub Actions workflows for deploying applications using Docker images to Azure Container Registry (ACR) or similar platforms. It is designed to automate and standardize deployment processes for backend and frontend services across multiple projects within the same organization.
 
 ## How It Works
 - **Reusable Workflows:** This repo contains workflows that can be triggered from any application repository to build and push Docker images for backend and frontend services.
 - **Centralized Deployment:** By using this repo, you ensure consistent deployment practices and security across all your application projects.
 
 ## Requirements for Application Repositories
 To use the deployment workflows in this repository, your application repo must:
 
 1. **Directory Structure:**
	 - Have a `backend/` and `frontend/` directory, each containing a valid `Dockerfile`.
 
 2. **Workflow Triggers:**
	 - Include `.github/workflows/` directory with trigger YAML files (e.g., `Trigger_backend_deploy.yml`, `Trigger_frontend_deploy.yml`) that call the workflows from this deploy repo.
 
 3. **Secrets, Variables & Environments:**
	 - Properly configure all required secrets, variables, and environments in your application repo settings (e.g., ACR credentials, environment variables, etc.).
 
 4. **Repository Location:**
	 - Be under the same organization/account as this deploy repo for access and security.
	 - Can be **private**, **internal**, or **public** (private is preferred for code safety; add collaborators as needed).
 
 ## About This Deploy Repo
 - **Type:** Internal repository
 - **Access:** Should have access enabled to other repos in the same organization/account.
 - **Purpose:** Centralizes deployment automation, improves security, and reduces duplication of CI/CD logic.

## Workflows in This Repository

### Backend_docker_deploy.yml
- **Purpose:** Builds, approves, pushes, and updates the backend Docker image for deployment to Azure Container Registry (ACR) and Azure Container Apps.
- **How it works:**
	1. **Build Image:** Accepts inputs for environment, image name, and app info. Uses secrets for Azure credentials and Docker environment variables. Checks out code, creates `.env`, logs in to Azure, sets version, builds the Docker image, saves and uploads it as an artifact.
	2. **Approval:** Waits for manual approval (can be set for production or any environment via GitHub UI).
	3. **Push Image:** Downloads the artifact, loads the Docker image, logs in to Azure and ACR, pushes the image to ACR.
	4. **Update Container App:** Sets registry credentials and updates the Azure Container App to use the new image version.

### Frontend_docker_deploy.yml
- **Purpose:** Builds, approves, pushes, and updates the frontend Docker image for deployment to Azure Container Registry (ACR) and Azure Container Apps.
- **How it works:**
	1. **Build Image:** Accepts inputs for environment, image name, and app info. Uses secrets for Azure credentials and Docker environment variables. Checks out code, logs in to Azure, sets version, builds the Docker image with build args, saves and uploads it as an artifact.
	2. **Approval:** Waits for manual approval (can be set for production or any environment via GitHub UI).
	3. **Push Image:** Downloads the artifact, loads the Docker image, logs in to Azure and ACR, pushes the image to ACR.
	4. **Update Container App:** Sets registry credentials and updates the Azure Container App to use the new image version.

## How to Make These Workflows Accessible to All Repos
1. **Keep this repo internal** within your organization and enable access to other repos in the same organization/account.
2. **Reference these workflows** in your application repo's trigger YAML files using the `workflow_call` event. Example:
	 ```yaml
	 jobs:
		 deploy-backend:
			 uses: <org>/<this-repo>/.github/workflows/Backend_docker_deploy.yml@main
			 with:
				 environment: "production"
				 image_name: "my-backend"
				 app_info: '{"acr_name": "myacr"}'
			 secrets:
				 azure_creds: ${{ secrets.AZURE_CREDS }}
				 docker_env: ${{ secrets.DOCKER_ENV }}
	 ```
3. **Ensure repo access** is configured so this deploy repo can be used by other repos (organization-level permissions).
	- Go to **Settings** → **Actions** → **General** → **Access** in the `tesa-app-deploy` repository.
	- Set **Accessible from repositories in the organization**.
 
 ## Example Application Repo
 
 <img width="1857" height="562" alt="image" src="https://github.com/user-attachments/assets/a6b89a0a-963f-4eb8-aa7d-61a50455cbef" />

 See the sample structure in the image above (`ai-hub-reusable-structure`):
 - Has `backend/` and `frontend/` folders
 - Contains `.github/workflows/` with trigger YAML files
 - Properly configured secrets and environments
 
 ## Getting Started
 1. **Fork or create your application repo** under the same organization/account.
 2. **Add backend and frontend services** with valid Dockerfiles.
 3. **Create trigger workflows** in `.github/workflows/` that reference the workflows in this deploy repo.
 4. **Configure secrets and environments** in your repo settings.
 5. **Ensure repo access** is set up for this deploy repo to interact with your application repo.
 
 ## Security Recommendations
 - Prefer **private** repos for applications to protect your code.
 - Grant collaborator access as needed.
 - Use organization-level secrets and environments for sensitive data.
 
 ## Support
 For questions or help, contact the maintainers or open an issue in this repository.
 
 ---
 **This repo is for deployment automation only. Application code and Dockerfiles should reside in your application repositories.**

---
For more information, refer to the wiki page: **Deployment Workflows for Applications**
