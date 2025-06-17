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
