# Spring AI Features API (Microservices Ecosystem)

This repository contains the architecture, core configuration, and service implementations for a production-ready, AI-driven microservices platform. The ecosystem leverages **Spring AI** for LLM orchestration and Retrieval-Augmented Generation (RAG), deployed on a cloud-native **AWS EKS** infrastructure.

## 🏗️ Architecture Overview

The system is designed as a distributed microservices architecture running inside an **Amazon EKS (Kubernetes)** cluster, managed with a GitOps approach.


### Core Architecture Components

*   **Ingress & Routing:** AWS Route 53 routes incoming traffic through an AWS Application Load Balancer (ALB) directly to the **Spring Cloud Gateway** inside EKS.
*   **Microservices Layer:**
    *   **User Service:** Handles authentication, authorization, and user profiles.
    *   **AI Service:** Orchestrates LLM interactions, prompts, and RAG pipelines using **Spring AI**.
    *   **Content Service:** Manages domain-specific structured metadata and data feeds.
    *   **Notify Service:** Listens to async events for decoupled system alerting and notification dispatching.
*   **Cross-Cutting Concerns:** Unified configuration sharing (ConfigMaps/Secrets), service discovery, fault tolerance via **Resilience4j Circuit Breakers**, and system-wide **Micrometer metrics**.


## 🛠️ Tech Stack & Managed Infrastructure

### Data & AI Foundations (Managed AWS)
*   **Database:** Amazon RDS PostgreSQL (isolated, per-service multi-tenant DBs).
*   **Caching:** Amazon ElastiCache (Redis) for low-latency session and data caching.
*   **Vector Database:** `pgvector` or Amazon OpenSearch serving as the embedded knowledge base for vector searches.
*   **LLM Inference Engine:** **Amazon Bedrock** for highly reliable serverless foundation models.

### Object Storage & Event Streaming
*   **Amazon S3:** Serves as the raw object store for unstructured documents and embedding sources.
*   **Amazon MSK (Kafka):** Orchestrates high-throughput, asynchronous AI batch processing jobs and internal microservice events.

### Platform & DevSecOps Layer
*   **Infrastructure as Code:** Terraform
*   **Continuous Deployment:** ArgoCD GitOps
*   **Observability Matrix:** Prometheus & Grafana (Metrics), Loki (Log Aggregation), Jaeger (Distributed Tracing)


## 🚀 Getting Started

### Prerequisites
*   Java 25
*   Docker & Kubernetes (`kubectl`)
*   AWS CLI configured with proper IAM access to Bedrock, RDS, and EKS.

### Local Development Setup

1. **Clone the Repository:**
   ```bash
   git clone https://github.com
   cd spring-ai-features-api
   ```

2. **Environment Configuration:**
   Create an `.env` file or export environment variables for credentials. **Never commit these keys to version control.**
   ```env
   AWS_ACCESS_KEY_ID=your_aws_key
   AWS_SECRET_ACCESS_KEY=your_aws_secret
   SPRING_AI_BEDROCK_REGION=us-east-1
   ```

3. **Build the Microservices:**
   ```bash
   ./gradlew build  # or ./mvnw clean package
   ```

4. **Spin up Local Core Infrastructure:**
   Use the local development profile to start databases and mock services:
   ```bash
   docker-compose -f docker/docker-compose.local.yml up -d
   ```

## 📊 Observability & Monitoring

Once deployed within the Kubernetes namespaces, access the cluster tracking interfaces via your local forwarding or private ingress URLs:
*   **Metrics Dashboard:** `http://localhost:3000` (Grafana)
*   **Distributed Traces:** `http://localhost:16686` (Jaeger UI)
