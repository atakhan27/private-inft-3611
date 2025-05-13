## Task 1 – httpServer_crash.go Analysis

The `httpServer_crash.go` file simulates a deliberately unreliable HTTP server. Its core purpose is to randomly fail in order to model a faulty microservice under load and test error handling, SLOs, and observability.

### Key Points:
- The server starts and listens on a specified port (usually 8080).
- It handles HTTP requests to endpoints such as `/` or `/health`.
- A **random crash mechanism** is introduced in the code using Go’s `math/rand` package. The logic might be something like:
  ```go
  if rand.Float32() < 0.3 {
      panic("server crash!")
  }

## Task 2 – Docker Build and Push

To containerize and deploy the unreliable HTTP server, I followed these steps:

1. **Authenticated Docker with Artifact Registry:**
   ```bash
   gcloud auth configure-docker me-central1-docker.pkg.dev

2. **Built the Docker image from the assignment directory:**
   ```bash
   docker build -t me-central1-docker.pkg.dev/inft-3611/unreliable-banking-image/atakhan-v1 .

3. **Pushed the image to GCP Artifact Registry:**
   ```bash
   docker push me-central1-docker.pkg.dev/inft-3611/unreliable-banking-image/atakhan-v1

## Task 3 – Terraform Deployment

To deploy the unreliable HTTP server to Google Cloud Run using Terraform, I followed these steps:

1. **Created main.tf with the following configuration:**
   ```bash
   provider "google" {
   project = "inft-3611"
   region  = "me-central1"
    }
    
    resource "google_cloud_run_v2_service" "unreliable_service" {
      name     = "unreliable-banking"
      location = "me-central1"
    
      template {
        containers {
          image = "me-central1-docker.pkg.dev/inft-3611/unreliable-banking-image/atakhan-v1"
        }
      }
    
      traffic {
        percent = 100
        type    = "TRAFFIC_TARGET_ALLOCATION_TYPE_LATEST"
      }
    }
    
    resource "google_cloud_run_v2_service_iam_member" "invoker" {
      name     = "unreliable-banking"
      location = "me-central1"
      project  = "inft-3611"
      role     = "roles/run.invoker"
      member   = "allUsers"
    }

2. **Initialized and Applied Terraform:**
   ```bash
   terraform init
   terraform apply

## Task 6 – YouTube Video
https://youtu.be/8kc456b2UJU
