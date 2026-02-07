# Roles and Skills Mapping for Yandex Go

## Components and roles

- Mobile App (iOS/Android)
  - Mobile Engineer (iOS/Android)
  - QA Engineer (Mobile)
  - UX/UI Designer
  - Release Manager
  - Security Engineer (Mobile)

- API Gateway
  - Backend Engineer (API/Platform)
  - DevOps Engineer
  - SRE (Site Reliability Engineer)
  - Security Engineer (API Security)
  - QA Engineer (Integration Testing)

- Dispatch Service
  - Backend Engineer (Geospatial/Real-time Systems)
  - Data Engineer (Location Data Pipelines)
  - ML Engineer (Matching Algorithms)
  - QA Engineer (Load/Performance Testing)
  - SRE

- Payment Service
  - Backend Engineer (Financial Systems)
  - Security Engineer (PCI DSS Compliance)
  - QA Engineer (Payment Flow Testing)
  - Compliance Specialist
  - DevOps Engineer

- Maps & Routing Service
  - Backend Engineer (Geospatial Systems)
  - Data Engineer (Map Data Processing)
  - ML Engineer (Traffic Prediction)
  - QA Engineer (Accuracy Testing)
  - DevOps Engineer

- Kubernetes Infrastructure & Deployment
  - DevOps Engineer
  - Platform Engineer
  - SRE
  - Cloud Infrastructure Engineer (Yandex Cloud Specialist)
  - Security Engineer (Cloud Security)

- Data Layer (YDB, ClickHouse, Redis)
  - Database Engineer
  - Data Engineer
  - DevOps Engineer (Database Operations)
  - SRE (Data Reliability)
  - Analytics Engineer (ClickHouse)

## Roles and responsibilities

### Backend Engineer (Microservices)
Designs, implements, and maintains microservices using gRPC and event-driven architecture. Writes business logic for domain services (User, Payment, Dispatch), implements API contracts, optimizes database queries, participates in code reviews, and ensures services meet SLA requirements. Collaborates with frontend/mobile teams on API design and integration.

### DevOps Engineer
Manages CI/CD pipelines for automated testing and deployment to Kubernetes clusters. Configures infrastructure as code (Terraform), implements monitoring/alerting (Prometheus/Grafana), manages secrets and certificates, optimizes container images, and ensures secure network policies between services. Handles incident response for infrastructure-related issues.

### Mobile Engineer (iOS/Android)
Develops and maintains native mobile applications using platform-specific SDKs (Swift/Kotlin). Implements real-time features (location tracking, push notifications), optimizes app performance and battery usage, integrates with backend APIs via REST/gRPC, handles offline scenarios, and ensures compliance with app store guidelines and security best practices.

### SRE (Site Reliability Engineer)
Defines and monitors SLOs/SLIs for critical user journeys (ride booking, dispatch). Implements observability solutions (distributed tracing, metrics, logs), conducts blameless postmortems for incidents, automates toil reduction, performs chaos engineering experiments, and ensures system resilience during traffic spikes or infrastructure failures.

### Data Engineer
Designs and maintains data pipelines between operational (YDB) and analytical (ClickHouse) stores. Implements ETL/ELT processes for ride analytics, optimizes ClickHouse schemas for reporting queries, manages Kafka/Logbroker topics and retention policies, ensures data quality and lineage, and supports business intelligence teams with data modeling.

## Common skills across roles

### Programming & Technologies
- Proficiency in at least one backend language (Go, Java, Python, Kotlin)
- SQL and database fundamentals (YDB, ClickHouse)
- REST/gRPC API design principles
- Git and version control workflows
- Linux/Unix command line proficiency

### Cloud & Infrastructure
- Kubernetes fundamentals (pods, services, deployments)
- Containerization (Docker)
- Cloud provider concepts (Yandex Cloud/AWS/GCP equivalents)
- Networking basics (HTTP/S, TCP/IP, DNS)
- Infrastructure as Code (Terraform/CloudFormation)

### Practices & Methodologies
- Microservices architecture patterns
- Event-driven architecture concepts
- Testing strategies (unit, integration, E2E)
- Monitoring and observability principles
- Security fundamentals (authentication, authorization, encryption)
- Agile/Scrum development practices

### Soft Skills
- Cross-functional collaboration
- Technical documentation writing
- Incident response and debugging under pressure
- Understanding of product requirements and user needs
- Continuous learning mindset for evolving technologies
