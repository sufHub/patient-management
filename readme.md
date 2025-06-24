# Patient Management - Microservices Architecture

## Overview

This project implements a comprehensive healthcare management system using microservices architecture. The system is designed to handle patient management, billing operations, analytics, and event-driven communication between services.

## Architecture Components

### Core Services

#### Patient Service
- **PatientApplication**: Main application entry point
- **PatientController**: REST API endpoints for patient operations
- **PatientService**: Business logic layer
- **PatientRepository**: Data access layer
- **PatientMapper**: Object mapping utilities
- **DTOs**: Data transfer objects for API communication
    - PatientRequestDTO
    - PatientResponseDTO
- **Exceptions**: Custom exception handling
    - PatientNotFoundException
    - GlobalExceptionHandler

#### Billing Service
- **BillingServiceApplication**: Main application for billing operations
- **BillingServiceGrpcClient**: gRPC client for inter-service communication
- **BillingGrpcService**: gRPC server implementation
- **BillingServiceOuterClass**: Protocol buffer generated classes
- **Request/Response Builders**: Utility classes for building billing requests and responses
- **DTOs**:
    - BillingRequest
    - BillingResponse

#### Analytics Service
- **AnalyticsServiceApplication**: Main application for data analytics
- **Comprehensive test suite**: AnalyticsServiceApplicationTests

#### API Gateway
- **ApiGatewayApplication**: Central entry point for all client requests
- **ApiGatewayApplicationTests**: Gateway testing suite

### Infrastructure Components

#### Event-Driven Architecture
- **KafkaProducer**: Publishes events to Kafka topics
- **KafkaConsumer**: Consumes events from Kafka topics
- **PatientEvent**: Event model for patient-related operations
- **PatientEventOrBuilder**: Event builder pattern implementation
- **PatientEventOuterClass**: Protocol buffer for event serialization

#### Data Management
- **Patient**: Core patient entity
- **CreatePatientValidationGroup**: Validation groups for patient creation

#### Email Service
- **EmailAlreadyExistsException**: Handles duplicate email validation

## Technology Stack

- **Framework**: Spring Boot
- **Communication**:
    - REST APIs
    - gRPC for inter-service communication
- **Message Broker**: Apache Kafka
- **API Gateway**: Spring Cloud Gateway
- **Data Serialization**: Protocol Buffers
- **Testing**: JUnit with comprehensive test suites

## Service Communication Patterns

### Synchronous Communication
- REST APIs for client-facing operations
- gRPC for high-performance inter-service communication

### Asynchronous Communication
- Kafka-based event streaming for decoupled service communication
- Event-driven architecture for real-time updates

## Key Features

### Patient Management
- Complete CRUD operations for patient records
- Validation and exception handling
- Event publishing for patient lifecycle changes

### Billing Integration
- Seamless integration between patient and billing services
- gRPC-based communication for billing operations
- Request/response pattern with proper error handling

### Analytics
- Data processing and analytics capabilities
- Event consumption for real-time analytics

### API Gateway
- Single entry point for all client requests
- Request routing and load balancing
- Cross-cutting concerns handling

## Getting Started

### Prerequisites
- Java 21
- Docker and Docker Compose
- Apache Kafka (modern version without Zookeeper dependency)
- Maven or Gradle
- IntelliJ IDEA (recommended for using pre-configured run configurations)

### Running the Services

1. **Start Infrastructure Services**
   ```bash
   docker-compose up -d kafka
   ```

2. **Run Services using IntelliJ IDEA**

   The project includes pre-configured run configurations in the `.run` folder. In IntelliJ IDEA:
    - Open the project
    - Navigate to the `.run` folder to see all available run configurations
    - Use the run configurations to start services in the correct order:
        1. Start Kafka (if using Docker)
        2. Patient Service
        3. Billing Service
        4. Analytics Service
        5. API Gateway

3. **Alternative: Build and Run Services via Command Line**
   ```bash
   # Patient Service
   mvn spring-boot:run -f patient-service/pom.xml
   
   # Billing Service
   mvn spring-boot:run -f billing-service/pom.xml
   
   # Analytics Service
   mvn spring-boot:run -f analytics-service/pom.xml
   
   # API Gateway
   mvn spring-boot:run -f api-gateway/pom.xml
   ```

### API Endpoints

#### Patient Service (via API Gateway)
- `GET /api/patients` - Get all patients
- `GET /api/patients/{id}` - Get patient by ID
- `POST /api/patients` - Create new patient
- `PUT /api/patients/{id}` - Update patient
- `DELETE /api/patients/{id}` - Delete patient

## Testing

Each service includes comprehensive test suites:

```bash
# Run all tests
mvn test

# Run specific service tests
mvn test -f patient-service/pom.xml
mvn test -f billing-service/pom.xml
mvn test -f analytics-service/pom.xml
mvn test -f api-gateway/pom.xml
```

## Architecture Benefits

- **Scalability**: Each service can be scaled independently
- **Resilience**: Failure in one service doesn't affect others
- **Technology Diversity**: Each service can use different technologies
- **Team Independence**: Different teams can work on different services
- **Deployment Flexibility**: Independent deployment cycles

## Event Flow

1. Client request comes through API Gateway
2. Gateway routes to appropriate service (Patient Service)
3. Patient Service processes request and updates database
4. Patient Service publishes event to Kafka
5. Analytics Service consumes event for real-time processing
6. Billing Service can be triggered via gRPC for billing operations

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.