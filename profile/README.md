# K8s-VulnSage: CVE Processing and AI-Powered Security Knowledge Retrieval

## Introduction

This system is designed for managing and analyzing Common Vulnerabilities and Exposures (CVEs). It leverages a microservices architecture deployed on Amazon EKS, using various AWS services and open-source tools to provide a scalable and robust solution for CVE tracking, storage, and analysis.

## System Architecture

The system is built on a microservices architecture, deployed on Amazon EKS. It uses Istio for service mesh, Kafka for event streaming, and PostgreSQL for data storage. The system is divided into several key components, each running in its own namespace within the EKS cluster.

## Components

1. **CVE Producer**: Monitors GitHub for new CVE releases and publishes them to Kafka.
2. **Kafka**: Acts as the central event bus for the system.
3. **CVE Consumer**: Consumes CVE data from Kafka and stores it in PostgreSQL.
4. **PostgreSQL**: Stores structured CVE data.
5. **CVE RAG App**: A Retrieval-Augmented Generation (RAG) application for analyzing CVEs, using Pinecone for vector storage and Groq for LLM inference.
6. **Monitoring Stack**: Includes Grafana, Prometheus, Tempo, and Kiali for comprehensive system monitoring and observability.
7. **Istio**: Provides service mesh capabilities, including traffic management and security.

## Development Workflow

1. Developers fork the repository and create pull requests.
2. Jenkins runs PR checks including linting, formatting, and semantic versioning.
3. Upon merge, Jenkins CI job builds containers and pushes them to Docker Hub.
4. Semantic releases are created on GitHub.

## Infrastructure Provisioning

1. Packer is used to create a Jenkins AMI.
2. Terraform provisions the Jenkins EC2 instance.
3. Terraform also provisions the EKS cluster and related AWS resources.

## EKS Cluster

The EKS cluster consists of three nodes, each running multiple namespaces:

- istio-system
- webapp-cve-producer
- kafka
- webapp-cve-consumer
- postgres
- monitoring
- cve-rag-app
- amazon-cloudwatch

Each namespace contains its specific deployments and services, with Istio sidecar (Envoy) injected for service mesh capabilities.

## Data Flow

1. GitHub Release Monitor detects new CVE releases.
2. A Kubernetes job downloads the CVE data and publishes it to Kafka.
3. The CVE consumer reads from Kafka and stores data in PostgreSQL.
4. The CVE RAG app processes data for analysis, using Pinecone for vector storage and Groq for LLM-based analysis.

## Security

- All secrets are managed using AWS Secrets Manager.
- Istio provides secure communication between services.
- Network policies are in place to restrict unnecessary communication between components.

## Monitoring and Observability

- Grafana, Prometheus, and Tempo provide comprehensive monitoring and tracing capabilities.
- Kiali offers service mesh observability.
- Fluent Bit exports logs to AWS CloudWatch.

## Getting Started

To deploy this system:

1. Ensure you have the necessary tools installed (Terraform, Packer, kubectl, AWS CLI).
2. Set up your AWS credentials.
3. Use Packer to build the Jenkins AMI.
4. Use Terraform to provision the Jenkins EC2 instance and EKS cluster.
5. Apply Kubernetes manifests for each component, starting with Istio.
6. Configure Istio Gateway and Virtual Services for routing.
7. Set up external DNS and certificate management.
8. Deploy monitoring tools and configure dashboards.

For detailed deployment instructions, refer to the individual component READMEs in their respective directories.
