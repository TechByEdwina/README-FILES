# Serverless Job Application Tracker on AWS

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazonaws&logoColor=white)
![Serverless](https://img.shields.io/badge/Architecture-Serverless-blue?logo=amazonaws)
![DynamoDB](https://img.shields.io/badge/Database-DynamoDB-yellow?logo=amazonaws)
![Lambda](https://img.shields.io/badge/Compute-Lambda-orange?logo=amazonaws)
![Status](https://img.shields.io/badge/Status-Deployed%20%26%20Validated-brightgreen)

> **Read the full project write-up on Medium:** [link]
> **Connect on LinkedIn:** [link]

---

## TL;DR

A fully serverless CRUD application built on AWS that allows job seekers to create, retrieve, and update job application records through a REST API and a hosted frontend. Built end-to-end using Lambda, DynamoDB, API Gateway, and Amplify — no servers, no infrastructure management, and zero cost within the free tier.

---

## What This Project Demonstrates

- Designing and deploying a full serverless CRUD application on AWS
- Building a REST API with API Gateway and connecting it to Lambda business logic
- Designing a DynamoDB schema for a real-world application use case
- Applying IAM least-privilege between Lambda, DynamoDB, and API Gateway
- Hosting a working frontend on AWS Amplify
- End-to-end system thinking: data layer → compute → API → frontend

---

## Prerequisites

- AWS account with admin IAM user
- Basic understanding of REST APIs and JSON
- AWS CLI installed (optional)

---

## Architecture Overview
Browser / Frontend (AWS Amplify)
│
▼
Amazon API Gateway (REST API)
│           │
▼           ▼
POST /     GET /applications
applications
│           │
└─────┬─────┘
▼
AWS Lambda Function
(Business Logic Handler)
│
▼
Amazon DynamoDB
(JobApplications Table)

**Data flow:**
1. User interacts with the frontend hosted on Amplify
2. Frontend sends HTTP requests to API Gateway endpoints
3. API Gateway triggers the Lambda function
4. Lambda reads from or writes to the DynamoDB table
5. Response travels back up through API Gateway to the frontend

---

## AWS Services Used

| Service | Purpose |
|---|---|
| Amazon API Gateway | Exposes HTTP REST endpoints for CRUD operations |
| AWS Lambda | Handles all business logic and DynamoDB interactions |
| Amazon DynamoDB | Stores all job application records |
| AWS IAM | Least-privilege execution role for Lambda |
| AWS Amplify | Hosts and serves the frontend to the browser |

---

## DynamoDB Table Configuration

| Setting | Value |
|---|---|
| Table Name | `JobApplications` |
| Partition Key | `applicationId` (String) |
| Billing Mode | On-Demand (Pay-per-request) |
| Capacity | Auto-scaling, no provisioning required |

**Data model — each job application record:**

```json
{
  "applicationId": "1",
  "companyName": "TechByEdwina",
  "jobTitle": "AWS Cloud Engineer",
  "applicationSource": "Company Website",
  "status": "Applied",
  "dateApplied": "2025-12-17",
  "location": "Remote",
  "actionToTake": "Evaluate company history and prepare for interview"
}
```

**Application status values:** `Applied` | `Interviewing` | `Offer` | `Rejected`

> [📸 Screenshot: DynamoDB table with sample records in Explore Items view]

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/applications` | Create a new job application record |
| GET | `/applications` | Retrieve all job application records |

> [📸 Screenshot: API Gateway console showing deployed REST API with routes]

---

## Implementation Breakdown

### Implementation 1: Data Layer — DynamoDB

Creates the `JobApplications` table with `applicationId` as the partition key. On-demand billing mode means no capacity planning is needed — the table scales automatically with usage.

**Validated:** Sample records created and confirmed in DynamoDB Explore Items view.

> [📸 Screenshot: DynamoDB table creation confirmation screen]

---

### Implementation 2: Backend Logic — AWS Lambda

Deploys a Lambda function as the central business logic handler. The function processes API Gateway events, performs the appropriate DynamoDB operation (PutItem for POST, Scan for GET), and returns a structured HTTP response.

The Lambda execution role is scoped to only what it needs: `dynamodb:PutItem` and `dynamodb:Scan` on the `JobApplications` table.

**Validated:** Lambda test events confirm successful DynamoDB reads and writes.

> [📸 Screenshot: Lambda function configuration and test execution result]
> [📸 Screenshot: IAM execution role policy attached to Lambda]

---

### Implementation 3: API Layer — API Gateway

Configures a REST API in API Gateway with two routes mapped to Lambda integrations. CORS is enabled to allow the Amplify-hosted frontend to make requests. The API is deployed to a `prod` stage.

**Validated:** API endpoint tested via curl and browser — returns correct DynamoDB records.

> [📸 Screenshot: API Gateway routes and Lambda integration configuration]
> [📸 Screenshot: API test response showing job application records]

---

### Implementation 4: Frontend — AWS Amplify

A lightweight HTML/CSS/JavaScript frontend is deployed to Amplify via the console. The frontend fetches data from the API Gateway endpoint and displays application records in the browser. Amplify handles hosting, CDN distribution, and HTTPS automatically.

**Validated:** Frontend live on Amplify URL, successfully fetching and displaying job application records from the API.

> [📸 Screenshot: Amplify deployment screen showing successful build]
> [📸 Screenshot: Live frontend in browser displaying job application records]

---

## Key Design Decisions

**Why on-demand DynamoDB billing?**
For a portfolio project and low-traffic applications, on-demand billing means you pay nothing until there is real usage. No capacity planning, no over-provisioning, no cost when idle.

**Why a single Lambda function for all routes?**
Simplicity is appropriate for this scale. One function handles the routing logic internally based on the HTTP method. In a production system, this would split into individual functions per route for better isolation and observability.

**Why Amplify instead of S3 static hosting?**
Amplify provides HTTPS, CDN distribution, and a managed deployment pipeline out of the box. For a frontend that needs to make authenticated API calls from a browser, HTTPS is required — Amplify handles this without manual certificate configuration.

---

## Skills Demonstrated

- Serverless application design and deployment
- REST API design with API Gateway
- DynamoDB schema design and CRUD operations
- Lambda function development and IAM scoping
- Frontend deployment and CDN hosting with Amplify
- End-to-end cloud application architecture

---

## Related

- Medium article: [link]
- LinkedIn: [link]