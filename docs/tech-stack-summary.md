# FitFlow Recommended Technology Stack

## Decision

Use a TypeScript-centered frontend and core API, with a separate Python service for AI workloads:

- React Native for iOS and Android
- Next.js for the web application
- Shared TypeScript models, validation, API clients, and design tokens
- NestJS for the core API and WebSocket gateway
- FastAPI for AI and machine-learning inference
- PostgreSQL as the system of record
- Redis for cache, queues, rate limits, and realtime fan-out
- Amazon Cognito for authentication and authorization tokens
- S3-compatible object storage for authorized media and exports
- Docker and GitHub Actions for consistent delivery

## Why This Fits FitFlow

React Native aligns with the original case-study direction and gives a mid-sized JavaScript/TypeScript team a fast route to native iOS and Android experiences. A dedicated Next.js web client avoids forcing every web screen through a mobile abstraction while still sharing non-visual code. NestJS supplies clear modules, guards, validation, WebSockets, testing support, and an architecture that is easier to govern than an unstructured Express application. FastAPI keeps AI work in Python, close to the machine-learning ecosystem, while PostgreSQL provides strong relational integrity for users, consent, plans, workout logs, nutrition, challenge membership, and audit records.

The approach is intentionally hybrid. Native bridges remain available for HealthKit, Health Connect, notifications, camera features, TensorFlow Lite, and ML Kit. Redis handles short-lived and high-frequency data, but PostgreSQL remains authoritative.

