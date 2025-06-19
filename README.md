# airbnb-clone-project

## Overview
This project clones a backend system of Airbnb which is specifically designed to support property listings, authenticate users, support bookings and allow for reviews. the main aim is to build a RESTful API which is scalable and closely mimicks key features of Airbnb's backend system. The project also pays attention to clean, smooth architecture and high performance.

## Project Goals
- Implement user authentication (signup/login)
- Perform database operations paying attention to CRUD features
- Handle booking/reservations
- Support reviews/ratings for properties
- Enhance user interactions with search functionalities
- Maintain API documentation

## Tech Stack
The proposed stacks for this project includes the following:
- **Backend**: Python Django framework || Django REST Framework
- **Database**: PostgreSQL and Redis for caching
- **Authentication**: OAUth and API keys
- **GraphQL**: for query
- **API Documentation**: OpenAPI
- **Deployment**: Docker
- **CI/CD Practices**: GitHub Actions
- **Celery**: Asynchronous task running

## Team Roles
The roles for the project are discussed below:
### Backend Developer
Responsibilities:
- Designs and implements the core API logic (user auth, listings, bookings, reviews)
- Writes clean, maintainable code in Python using Django framework
- Ensures proper data validation and error handling
- Integrates with databases and third-party services


### Database Administrator (DBA)
Responsibilities:
- Designs and optimizes PostgreSQL schemas for listings, users, and bookings
- Implements indexes and query optimizations for performance
- Sets up Redis caching for high-traffic endpoints (e.g., search results)
- Ensures data security and backup strategies

### DevOps Engineer
Responsibilities:
- Configures Docker containers for consistent deployment
- Sets up CI/CD pipelines (GitHub Actions)
- Implements monitoring/logging
- Automates scaling for load handling (e.g., during peak booking times)

### QA Engineer
Responsibilities:
- Writes integration/unit tests
- Tests API endpoints for edge cases (e.g., overlapping bookings)
- Validates data integrity across services
- Performs load testing
- Documents and tracks bugs

## Technology Stack
### Backend & API
1. **Django**: A high-level functional Python framework for rapid deployment, handling routing, middleware, and business logic.

2. **Django REST Framework (DRF)**: Extends Django to build RESTful APIs with features like serializers, authentication, and throttling.

3. **GraphQL**: Alternative to REST for flexible queries (e.g., fetching nested booking/user data in a single request).

### Database & Caching
1. **PostgreSQL**: Relational database for structured data (users, listings, bookings) with ACID compliance.

2. **Redis**: In-memory cache for high-speed access to frequently queried data (e.g., search results, session storage).

### Asynchronous Tasks
1. **Celery**: Handles background jobs (e.g., sending confirmation emails, cleaning up expired bookings) with Redis/RabbitMQ as a broker.

### Deployment & Infrastructure
1. **Docker**: Containerization for consistent environments (dev, staging, production).

2. **CI/CD Pipelines**: Automated testing/deployment (e.g., GitHub Actions/GitLab CI) to ensure code quality and streamline releases.

## Database Design

### Key Entities & Relationships
**Users**

**Fields**: id, email, password_hash, first_name, last_name, role (host/guest)

**Relationships**:

- One-to-Many with Properties (A user can list multiple properties)
- One-to-Many with Bookings (A user can make multiple bookings)
- One-to-Many with Reviews (A user can write multiple reviews)

### Properties

**Fields**: id, title, description, price_per_night, location, host_id (FK to Users)

**Relationships**:

- Many-to-One with Users (Each property belongs to one host)
- One-to-Many with Bookings (A property can have multiple bookings)
- One-to-Many with Reviews (A property can receive multiple reviews)

### Bookings

**Fields**: id, start_date, end_date, total_price, status (confirmed/pending/cancelled), guest_id (FK to Users), property_id (FK to Properties)

**Relationships**:

- Many-to-One with Users (A booking is made by one guest)
- Many-to-One with Properties (A booking is for one property)
- One-to-One with Payments (Each booking has one payment)

### Reviews

**Fields**: id, rating, comment, created_at, guest_id (FK to Users), property_id (FK to Properties)

**Relationships**:

- Many-to-One with Users (A review is written by one user)
- Many-to-One with Properties (A review is for one property)

### Payments

**Fields**: id, amount, payment_method, transaction_id, status, booking_id (FK to Bookings)

**Relationships**:

- One-to-One with Bookings (A payment is linked to one booking)

## Feature Breakdown

### 1. **API Documentation**
**Implementation**: OpenAPI standard documentation auto-generated using Django REST framework and Graphene (GraphQL)

**Purpose**: Provides comprehensive, interactive API docs for both REST and GraphQL endpoints, enabling seamless frontend integration and developer onboarding

### 2. **User Authentication**
**Implementation**: OpenAPI keys and OAUth, including email verification and password reset flows

**Purpose**: Secures all API endpoints while supporting modern auth patterns (social login ready via OAuth2)

### 3. **Property Management**
**Implementation**: CRUD operations with Django ORM, featuring geospatial search (PostGIS), image uploads (AWS S3), and rich text descriptions

**Purpose**: Core functionality for hosts to create and manage listings with all necessary metadata

### 4. **Booking System**
**Implementation**: Date-range validation logic, availability calendar, and pricing calculation (base rate + cleaning fees)

**Purpose**: Handles the complete reservation lifecycle from inquiry to confirmation with conflict prevention

### 5. **Payment Processing**
**Implementation**: Stripe integration for secure card payments with webhook handling for asynchronous payment status updates

**Purpose**: PCI-compliant transaction processing supporting holds, refunds, and platform commission

### 6. **Review System**
**Implementation**: Rating validation (1-5 stars), optional text reviews, and host responses with moderation capabilities

**Purpose**: Builds trust through verified guest feedback and public host responses

### 7. **Database Optimisation**
**Implementation**: PostgreSQL query optimization (indexing), Redis caching for frequent queries, and connection pooling

**Purpose**: Ensures scalability and performance under load, particularly for search and booking operations

## API Security

Multiple layers of protection are provided to guard the platform and users:

### Key Security Measures
1. **Authentication**

- JWT (JSON Web Tokens) with short-lived access tokens and refresh tokens
- Secure cookie storage for web clients
- Password hashing using bcrypt

2. **Authorization**

- Role-based access control (RBAC) for users/hosts/admins
- Property ownership verification for all mutating operations
- Django permissions framework for endpoint-level controls

3. **Rate Limiting**

- Redis-backed throttling (DRF's AnonRateThrottle/UserRateThrottle)
- 100 requests/minute for authenticated users
- 20 requests/minute for anonymous users

4. **Data Protection**

- All sensitive fields (emails, payments) encrypted at rest
- PostgreSQL column-level encryption for PII
- Regular security audits with bandit and safety checks

5. **Payment Security**

- PCI-compliant Stripe integration (no raw card data touches our servers)
- Webhook signature verification
- Dual approval for refunds over $500

6. **API Hardening**

- CSRF protection for web endpoints
- CORS restrictions to trusted domains only
- SQL injection prevention via Django ORM
- XSS protection via DRF's template rendering

| Area |  Risks Mitigated  | Business Impact |
|:-----|:--------:|------:|
| User Accounts   | Credential stuffing, account takeovers | Protects user trust and platform integrity |
| Property Data   |  Unauthorized modifications, data leaks  |   Maintains host confidence in the platform |
| Booking System   | Inventory denial, fake reservations |    Prevents revenue loss and system abuse |
| Payments   | Fraud, chargebacks, theft |    Avoids financial losses and legal issues |
| Reviews   | Spam, fake reviews, reputation attacks |    Ensures authentic community feedback |

Regular penetration testing will be conducted as part of the project's CI/CD pipeline

## CI/CD Pipeline

Continuous Integration (CI) and Continuous Deployment (CD) automate the process of testing, building, and deploying code changes. This ensures rapid, reliable updates to the production environment while maintaining stability.

### Importance for This Project:
1. **Quality Control**: Automated testing catches bugs before they reach production

2. **Developer Efficiency**: Enables frequent, small updates instead of risky bulk deployments

3. **Consistency**: Eliminates "works on my machine" issues via containerized environments

4. **Security**: Scans for vulnerabilities in dependencies and code during every push

### The Pipeline Stages:
1. **Test**:
    - Run unit/integration tests (Python + Jest)
    - Security scans (Bandit, npm audit)
    - Linting (ESLint, Flake8)

2. **Build**:
    - Create Docker images for backend services
    - Bundle frontend assets
    - Generate API documentation

3. **Deploy**:
    - Push to staging environment for manual validation
    - Automated production rollout (after approval)
    - Database migrations (with rollback safety)

### Tools To Use:
1. **GitHub Actions**: For workflow automation (triggers on PRs/main branch)
2. **Docker**: Containerization for consistent environments
3. **AWS ECS/ECR**: Cloud deployment, Container orchestration and registry
4. **PostgreSQL**: Managed database migrations

### To setup GitHub Actions:
- Create .github/workflows/deploy.yml in the root folder of the project
- Configure AWS credentials as GitHub secrets, each credential in a separate secret
- Set up environment-specific variables
