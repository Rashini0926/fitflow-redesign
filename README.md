# FitFlow Redesign

FitFlow is a redesigned cross-platform fitness application focused on personalized workout planning, low-effort nutrition tracking, private social motivation, and understandable progress feedback.

This repository is the technology-selection and architecture baseline created for IT3060 Human Computer Interaction Lab Exercise 05.

## Recommended Stack

| Layer | Technology | Purpose |
| --- | --- | --- |
| Mobile | React Native with TypeScript | Shared iOS and Android application |
| Web | Next.js with React and TypeScript | Accessible, responsive web experience |
| Shared frontend | TypeScript packages | Models, validation, API client, design tokens |
| Core API | NestJS on Node.js | Maintainable modular API and WebSocket gateway |
| AI service | Python FastAPI | Workout generation, explanations, and model inference |
| Primary data | PostgreSQL | Transactional health, workout, nutrition, consent, and social data |
| Cache and realtime | Redis | Cache, rate limits, sessions, queues, and realtime fan-out |
| Authentication | Amazon Cognito | OAuth 2.0 and OpenID Connect identity for mobile and web |
| Object storage | Amazon S3 | User-authorized media and exports |
| Delivery | Docker and GitHub Actions | Repeatable builds, tests, and deployment |

## Repository Structure

```text
fitflow-redesign/
├── .github/
│   ├── pull_request_template.md
│   └── workflows/ci.yml
├── ai-service/
│   ├── app/main.py
│   ├── README.md
│   └── requirements.txt
├── backend/
│   ├── src/main.ts
│   ├── README.md
│   └── package.json
├── docs/
│   ├── adr/ADR-001-selected-technology-stack.md
│   ├── architecture.md
│   ├── decision-matrix.csv
│   ├── technology-comparison.md
│   └── tech-stack-summary.md
├── frontend/
│   ├── mobile/README.md
│   └── web/README.md
├── .editorconfig
├── .gitignore
├── CONTRIBUTING.md
└── package.json
```

## FitFlow Feature Flows

- Personalized workout plans: profile and consent data are validated by the core API before the AI service creates a plan, rationale, and safe alternatives.
- Nutrition tracking: users record food and portions through the core API; optional camera or barcode assistance remains consent-based.
- Private social challenges: audience rules are checked on every read and write; authorized updates are distributed through the WebSocket layer.
- Progress tracking: workout and nutrition events are aggregated into understandable weekly and monthly summaries.

## Security Baseline

- OAuth 2.0 and OpenID Connect with Authorization Code and PKCE
- Short-lived access tokens, refresh-token rotation, and role/attribute checks
- TLS in transit and managed encryption at rest
- Explicit consent, data minimization, export, correction, and deletion workflows
- Audit logging for administrative and sensitive-data access
- Secrets stored outside the repository
- Rate limiting, input validation, dependency scanning, and protected branches

Technology selection alone does not make FitFlow HIPAA or GDPR compliant. Compliance depends on contracts, deployment configuration, operating procedures, data flows, retention, access control, and continuous verification.

## Local Development

This Lab 05 repository is an architecture scaffold. The source folders contain minimal health-check placeholders so that CI can validate the initial structure.

```bash
npm install
npm run check
```

For the AI service:

```bash
cd ai-service
python -m venv .venv
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8001
```

## Documentation

- [Technology comparison](docs/technology-comparison.md)
- [Weighted decision matrix](docs/decision-matrix.csv)
- [High-level architecture](docs/architecture.md)
- [Architecture Decision Record](docs/adr/ADR-001-selected-technology-stack.md)

## GitHub Setup

After creating an empty public or private repository named `fitflow-redesign`, connect and push this local repository:

```bash
git remote add origin https://github.com/Rashini0926/fitflow-redesign.git
git branch -M main
git push -u origin main
```

Then enable branch protection for `main`: require a pull request, require the CI status check, dismiss stale approvals, and block force pushes.

