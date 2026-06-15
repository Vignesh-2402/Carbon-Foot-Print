# 🌱 EcoTrack — Carbon Footprint Awareness Platform

A full-stack GCP-native platform helping individuals understand, track, and reduce their carbon footprint through AI-powered personalized insights.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    GCP Architecture                      │
├─────────────────────────────────────────────────────────┤
│  Firebase Hosting  ←→  Cloud Run (API)                  │
│  Firestore (DB)    ←→  Cloud Functions (Serverless)     │
│  Vertex AI (LLM)   ←→  BigQuery (Analytics)             │
│  Pub/Sub (Events)  ←→  Cloud Scheduler (CRON)           │
│  Secret Manager    ←→  Cloud Monitoring + Logging       │
└─────────────────────────────────────────────────────────┘
```

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18 + Vite + TailwindCSS |
| Backend | Node.js + Express (Cloud Run) |
| Database | Firestore (NoSQL) |
| AI/ML | Vertex AI (Gemini Pro) |
| Analytics | BigQuery |
| Serverless | Cloud Functions (Gen 2) |
| Messaging | Cloud Pub/Sub |
| Scheduling | Cloud Scheduler |
| Auth | Firebase Authentication |
| Hosting | Firebase Hosting |
| IaC | Terraform |
| Monitoring | Cloud Monitoring + Error Reporting |

## Project Structure

```
carbon-footprint-platform/
├── frontend/               # React SPA → Firebase Hosting
├── backend/                # Express API → Cloud Run
├── functions/              # Cloud Functions (Gen 2)
│   ├── carbon-calculator/  # Emissions calculation engine
│   ├── ai-insights/        # Vertex AI personalized tips
│   └── data-pipeline/      # BigQuery ingestion pipeline
├── infrastructure/         # Terraform IaC
│   ├── terraform/
│   └── scripts/            # Deployment shell scripts
└── tests/                  # Unit, Integration, E2E tests
```

## Quick Start

```bash
# 1. Set your GCP project
export GCP_PROJECT_ID="your-project-id"

# 2. Run setup script (provisions all GCP resources)
chmod +x infrastructure/scripts/setup.sh
./infrastructure/scripts/setup.sh

# 3. Deploy everything
./infrastructure/scripts/deploy-all.sh
```

## Cloud Run Deployment

This repository is configured for Google Cloud Run with a containerized backend and a containerized frontend build.

- `backend/` is packaged as a Cloud Run service using `backend/Dockerfile`
- `frontend/` is packaged using `frontend/cloudbuild-frontend.yaml`
- API requests can be configured using `VITE_API_URL` at build time

### Required Cloud Run Environment Variables

Set these values when deploying the backend service to Cloud Run:

- `FIREBASE_SERVICE_ACCOUNT` (JSON string) or use `GOOGLE_APPLICATION_CREDENTIALS`
- `GCP_PROJECT_ID`
- `GCP_REGION` (optional, defaults to `us-central1`)
- `BQ_LOCATION` (optional, defaults to `US`)
- `BQ_DATASET` (optional, defaults to `ecotrack_analytics`)
- `REPORTS_BUCKET` (optional)
- `PUBSUB_TOPIC` (optional, defaults to `carbon-events`)
- `ALLOW_GUEST_ACCESS` (`true` or `false`)
- `ALLOWED_ORIGINS` (comma-separated origins)
- `GOOGLE_CLIENT_ID`
- `LOG_LEVEL` (optional, defaults to `info` in production)

### Deployment Notes

- `frontend/cloudbuild-frontend.yaml` builds the frontend image with `VITE_API_URL`.
- `backend/Dockerfile` is Cloud Run compatible and exposes port `8080`.
- Use `backend/.env.example` for local container testing.

## Local Cloud Run Support

The backend already includes a Cloud Run compatible Docker image at `backend/Dockerfile`.

#### Run locally with Docker

1. Copy `backend/.env.example` to `backend/.env` and fill in values.
2. From `backend/`, build the container:

```bash
npm run docker:build:local
```

3. Run the container using the env file:

```bash
npm run docker:run:local
```

4. Verify the service:

```bash
curl http://localhost:8080/health
```

5. Start the frontend locally:

```bash
cd ../frontend
npm install
npm run dev
```

6. Open the app in the browser and use the API at `http://localhost:8080`.

#### Notes

- For Firebase access, either set `FIREBASE_SERVICE_ACCOUNT` as JSON in `backend/.env`, or mount a service account file and use `GOOGLE_APPLICATION_CREDENTIALS`
- The container exposes port `8080`, matching Cloud Run.
- The same Dockerfile used locally is the one used for Cloud Run deployment.

## GCP Services Used

- **Firebase Auth** — Google/Email sign-in
- **Firestore** — User profiles, activity logs, goals
- **Cloud Run** — REST API (auto-scaling, containerized)
- **Cloud Functions** — Carbon calc, AI insights, webhooks
- **Vertex AI (Gemini Pro)** — Personalized reduction tips
- **BigQuery** — Aggregate analytics, emissions trends
- **Pub/Sub** — Async event pipeline (activity → insights)
- **Cloud Scheduler** — Weekly report emails, CRON jobs
- **Secret Manager** — API keys, credentials
- **Cloud Monitoring** — Dashboards, alerts, SLOs
- **Cloud Storage** — User exports, report PDFs
- **Artifact Registry** — Docker image storage

## Evaluation Criteria Addressed

- ✅ **Code Quality** — Modular, typed, documented
- ✅ **Security** — IAM, Secret Manager, RBAC, input validation
- ✅ **Efficiency** — Firestore indexes, BigQuery partitioning, CDN
- ✅ **Testing** — Unit (Vitest), Integration (Supertest), E2E (Playwright)
- ✅ **Accessibility** — WCAG 2.1 AA, ARIA labels, keyboard nav
"# Carbon-Foot-Print" 
