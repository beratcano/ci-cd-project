# DevOps CI/CD Pipeline Project: Python Application Deployment with Kind and GitHub Actions

## Project Goal

To build a complete CI/CD pipeline for a simple Python application, using Docker, GitHub Container Registry (GHCR), Kind (Kubernetes IN Docker), and GitHub Actions.

## Project Stages

1.  **Initial Setup:**
    * Created a GitHub repository.
    * Developed a basic Python application (`app.py`).
    * Implemented unit tests (`test_app.py`).
    * Committed and pushed the initial code.
2.  **Basic CI Pipeline (GitHub Actions):**
    * Created a GitHub Actions workflow (`ci-cd.yml`).
    * Configured the workflow to:
        * Checkout the code.
        * Set up Python.
        * Install dependencies (pytest).
        * Run unit tests.
        * Execute the Python application.
    * Committed and pushed the workflow.
    * Observed the pipeline execution in GitHub Actions.
3.  **Dockerization and GHCR Integration:**
    * Created a `Dockerfile` to containerize the Python application.
    * Created a `requirements.txt` file.
    * Modified the `ci-cd.yml` workflow to:
        * Build the Docker image.
        * Log in to GHCR using a personal access token (`CR_PAT`).
        * Tag and push the Docker image to GHCR.
    * Created a GitHub secret named `CR_PAT` to store the GHCR personal access token with `write:packages` scope.
    * Committed and pushed the updated workflow.
    * Verified the Docker image in GHCR.
4.  **Kubernetes Deployment (Kind):**
    * Installed Go (Golang).
    * Installed Kind.
    * Created a Kind Kubernetes cluster using `kind create cluster --config kind-config.yaml` to specify a specific kubernetes version.
    * Created Kubernetes deployment and service YAML files (`deployment.yaml` and `service.yaml`).
    * Configured `kubectl` in GitHub Actions using the `azure/setup-kubectl` action and a `KUBE_CONFIG` secret.
    * Created a Github secret called `KUBE_CONFIG` and placed the entire contents of the `~/.kube/config` file into it.
    * Modified the `ci-cd.yml` workflow to:
        * Set up `kubectl`.
        * Apply the deployment and service YAMLs.
        * Verify the deployment.
    * Committed and pushed the updated workflow.
    * Troubleshot issues with `kubectl` context and `kubeconfig`.
    * Verified the deployment in the Kind cluster using `kubectl get pods` and `kubectl get services`.
    * Accessed the application using the Docker Network Gateway IP and the NodePort.
5.  **Troubleshooting and Refinement:**
    * Encountered and resolved issues related to:
        * Missing `kubeconfig` file.
        * `kubectl` context errors.
        * Accessing the application through `NodePort`.
    * Documented the process and troubleshooting steps.

## Key Technologies

* **GitHub Actions:** CI/CD platform.
* **Docker:** Containerization.
* **GitHub Container Registry (GHCR):** Container image registry.
* **Kind (Kubernetes IN Docker):** Local Kubernetes cluster.
* **kubectl:** Kubernetes command-line tool.
* **Go (Golang):** Kind dependency.
* **Python:** Application development.
* **pytest:** Unit testing.

## Challenges and Solutions

* **kubeconfig Issues:**
    * **Problem:** `~/.kube/config` file not found.
    * **Solution:** Installed Minikube/Kind and started the cluster.
    * **Problem:** Incorrect or missing context.
    * **Solution:** Verified context names, recreated the cluster, and ensured the correct `kubeconfig` was used.
* **GHCR Authentication:**
    * **Problem:** Securely storing and using GHCR credentials.
    * **Solution:** Created a GitHub secret (`CR_PAT`) and used it in the workflow.
* **Kind Cluster Access:**
    * **Problem:** Accessing the application from outside the local machine.
    * **Solution:** Used the docker network gateway ip address and the NodePort of the service.
* **Kubernetes Version Mismatch:**
    * **Problem:** Running a specific kubernetes version.
    * **Solution:** Created a kind configuration file, and specified the desired kubernetes version.

## Future Improvements

* Implement Ingress controllers for domain-based access.
* Add persistent volumes for data storage.
* Integrate Helm for application management.
* Implement monitoring and logging.
* Enhance security with RBAC and network policies.
* Automated testing of infrastructure as code.
* Implement blue/green deployments.
* Implement automated rollback procedures.
* Add integration tests to the CI/CD pipeline.
* Add security scanning to the docker images.

## Project Outcome

A fully functional CI/CD pipeline that automates the deployment of a Python application to a local Kubernetes cluster using Kind, Docker, and GitHub Actions.

## Raw Yaml Files.

### Dockerfile

```dockerfile
FROM python:3.9-slim-buster

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]