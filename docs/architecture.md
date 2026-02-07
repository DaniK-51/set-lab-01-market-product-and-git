# Architecture

## Overview

Yandex Go is a ride-hailing platform that connects passengers with drivers through a scalable microservices architecture. The system handles the complete ride lifecycle—from user registration and ride booking to real-time dispatching, dynamic pricing, payment processing, and post-ride analytics.

Key architectural principles:
- **Microservices**: Domain-driven decomposition into loosely coupled, independently deployable services
- **Event-driven communication**: Asynchronous messaging via Kafka/Logbroker for state changes and cross-service coordination
- **API Gateway pattern**: Single entry point for all client requests with authentication, rate limiting, and request routing
- **Cloud-native design**: Full deployment on Yandex Cloud with Kubernetes orchestration for elasticity and resilience
- **Separation of concerns**: Distinct operational (YDB) and analytical (ClickHouse) data stores for transactional and reporting workloads

The platform serves millions of daily rides across multiple cities while maintaining sub-second response times for critical user interactions.

## Components

![Yandex Go Component Diagram](./diagrams/out/yandex-go/architecture-component/Component%20Diagram.svg)

[Component Diagram PlantUML source code](.\diagrams\src\yandex-go\architecture-component.puml)

The system comprises the following core components:

- **API Gateway**: Entry point for all client requests. Handles authentication, request validation, rate limiting, and routing to appropriate backend services. Provides unified GraphQL/REST API surface.

- **User Service**: Manages user profiles, authentication tokens, driver licenses verification, and passenger/driver status. Integrates with Yandex ID for single sign-on.

- **Pricing Service**: Calculates dynamic ride fares based on distance, time, traffic conditions, demand surges, and promotional discounts. Uses real-time data from Maps & Routing Service.

- **Dispatch Service**: Core matching engine that pairs available drivers with ride requests using geospatial algorithms. Maintains real-time driver location state and optimizes assignment latency.

- **Maps & Routing Service**: Provides map tiles, geocoding, route calculation, and real-time traffic data. Integrates with Yandex Maps APIs and maintains offline map caches for drivers.

- **Payment Service**: Processes payments via multiple methods (cards, Yandex Pay, cash). Handles fare calculation finalization, refunds, driver payouts, and transaction reconciliation.

- **Notification Service**: Manages push notifications (via Yandex Push), SMS, and in-app messages for ride status updates, promotions, and system alerts.

- **Shared infrastructure**:
  - **Message Broker (Kafka/Logbroker)**: Event bus for asynchronous communication (e.g., `RideRequested`, `DriverAssigned`, `PaymentCompleted`)
  - **Redis Cache**: Stores session data, driver locations, and frequently accessed configuration
  - **YDB (Yandex Database)**: Distributed SQL database for transactional data (users, rides, payments)
  - **ClickHouse**: Columnar database for analytics, reporting, and business intelligence queries

Services communicate via gRPC for internal synchronous calls and publish/consume events through the message broker for asynchronous workflows.

## Sequence

![Yandex Go Sequence Diagram](diagrams\out\yandex-go\architecture-sequence\Sequence%20Diagram.svg)

[Sequence Diagram PlantUML source code](diagrams\src\yandex-go\architecture-sequence.puml)

The diagram illustrates the primary ride-booking flow:

1. **Passenger initiates ride request** through the mobile app, specifying pickup/drop-off locations.
2. **API Gateway** authenticates the user and forwards the request to Pricing Service.
3. **Pricing Service** calculates fare estimate using Maps & Routing Service for distance/duration and current demand factors.
4. Passenger confirms the ride; API Gateway publishes `RideRequested` event to the message broker.
5. **Dispatch Service** consumes the event, queries Redis for nearby available drivers, and executes matching algorithm.
6. Upon successful match, Dispatch Service publishes `DriverAssigned` event and notifies both passenger and driver via Notification Service.
7. During the ride, driver app periodically sends location updates to Dispatch Service, which forwards them to passenger app via WebSocket connection.
8. Upon ride completion, driver confirms arrival; Payment Service processes final fare calculation and initiates payment transaction.
9. Payment Service publishes `PaymentCompleted` event, triggering receipt generation and driver payout workflow.

This sequence demonstrates the event-driven nature of the architecture, where services remain decoupled while maintaining eventual consistency across the system state.

## Deployment

![Yandex Go Deployment Diagram](diagrams\out\yandex-go\architecture-deployment\Deployment%20Diagram.svg)

[Deployment Diagram PlantUML source code](diagrams\src\yandex-go\architecture-deployment.puml)

The system is deployed within Yandex Cloud infrastructure with the following topology:

- **Client tier**: The Yandex Go mobile application runs on user smartphones (iOS/Android), while the web version operates in browsers on desktop computers. Both clients communicate with the backend via secure HTTPS connections.

- **Edge layer**: All incoming traffic passes through a Load Balancer that distributes requests across service instances to ensure high availability and fault tolerance.

- **Application tier**: All microservices (API Gateway, User Service, Payment Service, Dispatch Service, Pricing Service, Maps & Routing Service, Notification Service) are deployed as Pods within a managed Kubernetes cluster (Yandex Managed Service for Kubernetes), enabling dynamic scaling and self-healing capabilities.

- **Data and integration tier**:
  - Message Broker (Kafka/Logbroker) operates as a dedicated cluster to handle the event bus between services.
  - Redis cache runs in a separate cluster for storing temporary states and sessions.
  - Operational database (YDB) and analytical database (ClickHouse) are hosted in dedicated managed clusters.
  - External services (Yandex Pay, Yandex Maps) are integrated via secure HTTPS calls initiated by the respective microservices.

Internal communication between microservices and databases primarily uses gRPC and YQL protocols to ensure high performance and strict type safety. Network policies within Kubernetes enforce zero-trust security principles, allowing only explicitly permitted service-to-service communication.