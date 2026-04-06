# API Overview

Complete OpenAPI documentation for {{{ docsVersionInfo.k0rdentName }}} Atlas, Arc, and shared services APIs.

## Overview

> **Draft:** This documentation is currently a work in progress and subject to change.

This section provides an overview of the {{{ docsVersionInfo.k0rdentName }}} API shape, covering:

- **Atlas** – provider console and infrastructure management.
- **Arc** – customer-facing AI console.
- **Shared services** – authentication, IAM, and other cross-cutting APIs.

The API is designed around a small, composable set of resource types with clear multi-tenant boundaries and strong contracts for authentication, authorization, and lifecycle operations.

## API docs variants

We're experimenting with different ways to present the API. Choose the format that works best for you:

- **This docs site (recommended)** – API + product docs combined for the {{{ docsVersionInfo.k0rdentName }}} platform.
- **[API Specification page](../api-reference/index.md)** – single page with Scalar, ReDoc, and Swagger UI; switch viewers using the tab bar at the top.

In this repository, the **primary source of truth** is:

- The bundled OpenAPI document at `openapi/openapi.bundled.yaml`.
- The narrative and endpoint reference in this `API specification` section.

Interactive API reference (same OpenAPI spec, different UIs):

- [Reference (Scalar)](../api-reference/index.md) – default view, focused Scalar UI.
- [Reference (ReDoc)](../api-reference/index.md#redoc) – classic three-panel reader.
- [Reference (Swagger UI)](../api-reference/index.md#swagger) – Swagger UI explorer.

## Welcome to the {{{ docsVersionInfo.k0rdentName }}} API

{{{ docsVersionInfo.k0rdentName }}} is a Kubernetes-native platform for managing AI infrastructure at scale. Built for Neoclouds and enterprises, it provides a composable, template-driven approach to deploying and operating Kubernetes clusters and AI services across distributed environments.

From an API perspective, {{{ docsVersionInfo.k0rdentName }}} exposes:

- **Cluster and compute lifecycle APIs** for Kubernetes clusters, VMs, and networks.
- **Infrastructure APIs** for provider-only operations.
- **Tenant and project APIs** for modeling organizations and projects.
- **Auth and IAM APIs** for tokens, checks, and policy.

## What {{{ docsVersionInfo.k0rdentName }}} solves

Modern AI infrastructure faces three core challenges:

- **Tech stack complexity** – divergent tooling, APIs, and security controls across providers and regions make it hard to standardize operations as hardware and platforms proliferate.
- **Operational efficiency** – without centralized visibility, cluster health, state, cost, and risk drift across environments, creating instability and compliance gaps.
- **Customer experience** – slow time-to-value onboarding new GPU capacity and services, difficulty maximizing utilization while meeting strict tenant requirements, and hard multi-tenancy raise the bar for reliability and isolation.

{{{ docsVersionInfo.k0rdentName }}} addresses these challenges through Kubernetes-native composability and multi-cluster automation. Infrastructure and services are defined as templates and reconciled continuously into running environments, enabling repeatable rollout across many clusters and sites with controlled drift and upgrades.

## Key capabilities

- **Composable infrastructure** – define infrastructure and services as templates, reconcile them across clusters and sites.
- **GPU efficiency** – GPU partitioning, topology-aware scheduling, and allocation strategies to improve utilization.
- **Hardware-enforced multi-tenancy** – tenant isolation across GPU allocation, virtualization, and network boundaries.
- **Integrated observability and FinOps** – centralized monitoring and cost reporting for operational control and tenant-level consumption reporting.
- **Workload convergence** – support for VM and container workloads under one operational model.

## Platform components

{{{ docsVersionInfo.k0rdentName }}} consists of three main components:

- **{{{ docsVersionInfo.k0rdentName }}} Cluster Manager (KCM)** – deployment and lifecycle management of Kubernetes clusters, including configuration, updates, and other CRUD operations.
- **{{{ docsVersionInfo.k0rdentName }}} State Manager (KSM)** – installation and lifecycle management of deployed services (leveraging [Project Sveltos](https://github.com/projectsveltos/sveltos)).
- **{{{ docsVersionInfo.k0rdentName }}} Observability and FinOps (KOF)** – cluster and beach-head services monitoring, events and log management, with integrated cost reporting and tenant-level consumption tracking.

## API endpoint reference

All API endpoints are served from:

```text
https://api.k0rdent.ai
```

Visibility is controlled per-endpoint via the `x-visibility` extension:

- `internal` – provider / Atlas operations only.
- `public` – customer / Arc-facing operations.

### Compute

Region-scoped compute resources under `/v1/regions/{region}/projects/{project}/compute/`.

| Endpoint | Purpose |
|---------|---------|
| `/v1/regions/{region}/projects/{project}/compute/clusters` | Kubernetes cluster deployments. |
| `/v1/regions/{region}/projects/{project}/compute/clusters/{clusterId}/kubeconfigs` | Kubeconfig issuance and management. |
| `/v1/regions/{region}/projects/{project}/compute/instances` | VM and bare metal instance lifecycle. |
| `/v1/regions/{region}/projects/{project}/compute/networks` | Network configuration and topology. |
| `/v1/regions/{region}/projects/{project}/compute/subnets` | Subnet management. |
| `/v1/regions/{region}/projects/{project}/compute/addresses` | IP address allocation and release. |

### Infrastructure

Provider-only (`internal` visibility) infrastructure resources.

| Endpoint | Purpose |
|---------|---------|
| `/v1/regions/{region}/projects/{project}/infrastructure/servers` | Bare metal server lifecycle — enrollment, provisioning, state transitions. |

### Organizations & projects

Global resources for tenant and project management.

| Endpoint | Purpose |
|---------|---------|
| `/v1/regions/global/organizations` | Organization (tenant) management. |
| `/v1/regions/global/organizations/{id}/invitations` | Organization invitation management. |
| `/v1/regions/global/projects` | Project resource grouping. |

### Authentication

Token lifecycle and permission evaluation under `/v1/regions/global/auth/`.

| Endpoint | Purpose |
|---------|---------|
| `/v1/regions/global/auth/token` | Mint access tokens (`authorization_code`, `api_key`, `client_credentials`). |
| `/v1/regions/global/auth/introspect` | Token introspection (RFC 7662) — validate and decode token claims. |
| `/v1/regions/global/auth/check` | Evaluate permissions for a principal against actions and resources. |
| `/v1/regions/global/auth/revoke` | Token revocation (RFC 7009). |

### IAM

Identity and access management resources under `/v1/regions/global/iam/`.

| Endpoint | Purpose |
|---------|---------|
| `/v1/regions/global/iam/users` | User management and profile. |
| `/v1/regions/global/iam/groups` | User groups for team-based access control. |
| `/v1/regions/global/iam/roles` | RBAC role definitions. |
| `/v1/regions/global/iam/policies` | IAM policy bindings. |
| `/v1/regions/global/iam/apikeys` | API keys for programmatic access. |
| `/v1/regions/global/iam/service-accounts` | Machine-to-machine service accounts and credentials. |
| `/v1/regions/global/iam/providers` | Identity provider (OAuth/OIDC) configuration. |

## Future / TBD

The following endpoints are under discussion and have not been implemented yet:

| Endpoint | Purpose |
|---------|---------|
| `/v1/regions/global/audit` | Audit logs. |
| `/v1/regions/global/billing` | Billing and consumption. |
| `/v1/regions/global/analytics` | Analytics. |
| `/v1/regions/global/webhooks` | Webhook subscriptions. |
| `/v1/regions/{region}/projects/{project}/compute/instances` | Virtual machine lifecycle (in progress). |
| `/v1/regions/{region}/inference` | Inference endpoint lifecycle. |
| `/v1/regions/{region}/training` | Training job lifecycle. |

## API lifecycle and contracts

The {{{ docsVersionInfo.k0rdentName }}} API aims to provide:

- **Explicit versioning** – versioned base paths (`/v1/...`) with a clear story for introducing new versions.
- **Consistent envelopes** – shared conventions for resource identifiers, pagination, errors, and metadata.
- **Predictable evolution** – additive change by default, with deprecations and removals tracked in an API changelog.

For a full history of API endpoint changes and iterations, see the API changelog (to be added as a dedicated page in this section).

## Next steps

To go deeper:

- **Authentication** – learn how to authenticate your API requests (tokens, scopes, and principals).
- **Versioning** – understand the API versioning strategy and best practices for clients.
- **Common structures** – review conventions, data types, and common patterns.
- **Errors** – see error handling, status codes, and error response structures.
- **Standards** – learn the naming conventions and response envelopes that apply across endpoints.

These topics can be added as additional pages under the `API specification` section over time.
