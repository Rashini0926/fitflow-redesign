# ADR 001 Selected Technology Stack

- Status: Accepted for the FitFlow redesign prototype
- Date: 2026-09-13
- Decision owners: FitFlow product and engineering team

## Context

FitFlow requires a high-quality iOS and Android experience, a responsive web application, adaptive and explainable workout planning, low-effort nutrition logging, private social challenges, progress tracking, realtime updates, and careful handling of health-related and consent data. A mid-sized team must deliver quickly without creating four unrelated platform implementations.

The architecture must also preserve direct access to HealthKit, Health Connect, notifications, camera and on-device ML capabilities. Compliance is treated as an operational and architectural responsibility, not as a product label supplied by a framework.

## Decision

Use React Native with TypeScript for mobile, Next.js for web, and shared TypeScript packages for domain models, validation, API clients, and design tokens. Use NestJS as a modular core API and WebSocket gateway, PostgreSQL as the authoritative relational database, Redis for cache and realtime coordination, Amazon Cognito for identity, and a separate FastAPI service for AI inference. Use object storage for consented media and exports. Deploy services in containers and validate changes with GitHub Actions.

## Reasons

1. React Native aligns with the FitFlow case-study direction and enables rapid iOS/Android delivery while retaining native-module access.
2. A dedicated Next.js web client gives better web semantics and maintainability than forcing all web screens through a mobile UI abstraction.
3. TypeScript sharing reduces contract duplication across mobile, web, and the core API.
4. NestJS provides modules, guards, validation, OpenAPI, WebSockets, testing, and maintainable conventions for a mid-sized team.
5. FastAPI isolates Python AI dependencies and exposes a typed, documented internal interface.
6. PostgreSQL supports strong integrity and flexible queries for users, consent, workouts, nutrition, challenges, progress, and audit references.
7. Cognito integrates identity with the selected AWS deployment and can issue standard OAuth 2.0/OIDC tokens.

## Alternatives Considered

- Flutter: strongest single-UI-codebase alternative, but the selected design values React/TypeScript team reuse and a dedicated semantic web client.
- Kotlin Multiplatform: stronger native control, but lower UI reuse and higher specialist staffing cost for the initial redesign.
- SwiftUI: excellent Apple option but cannot satisfy Android and web requirements alone.
- Firebase-only backend: very fast for an MVP, but complex relational reporting, consent dependencies, and long-term vendor coupling make PostgreSQL plus a governed API preferable.
- Go core API: excellent runtime efficiency but lower delivery speed and code sharing for the chosen team profile.
- Auth0: excellent developer experience; Cognito is selected as the tie-breaker because identity, logs, storage, database, and deployment can remain under one AWS governance boundary.

## Consequences

### Positive

- Fast mobile delivery with native capability where required
- Clear separation between transactional product logic and AI inference
- Strong relational integrity and reporting flexibility
- Shared types and API contracts across most of the stack
- Independent horizontal scaling of core API, realtime, and AI workloads

### Negative

- Two frontend renderers must be maintained: React Native and Next.js
- The organization operates both TypeScript and Python toolchains
- React Native upgrades and native modules require planned compatibility testing
- Cognito configuration and custom user experience need specialist attention
- Distributed services add observability and deployment work

## Risk Controls

- Start with a modular monolith in NestJS and one bounded AI service; split further only with measured need.
- Generate TypeScript and Python clients/models from OpenAPI where practical.
- Use automated contract, accessibility, security, and end-to-end tests.
- Minimize data sent to the AI service and keep sensitive image analysis on-device where feasible.
- Review this decision if the web application becomes content/SEO dominant, native sensor performance becomes inadequate, or the AI workload requires a different serving platform.

