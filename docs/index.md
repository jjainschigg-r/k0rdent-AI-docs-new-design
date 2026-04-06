# Welcome to k0rdent AI

<div class="newstyle-home">

{{{ docsVersionInfo.k0rdentName }}} is a Kubernetes-native platform for managing AI infrastructure at scale. Built for Neoclouds and enterprises, it provides a composable, template-driven approach to deploying and operating Kubernetes clusters and AI services across distributed environments.

<br/>
<h3>Get started</h3>

<div class="newstyle-cards newstyle-cards--primary">
  <a href="api-specification/overview/" class="newstyle-card">
    <span class="newstyle-card__title">API overview</span>
    <span class="newstyle-card__desc">
      Our plans for k0rdent AI API evolution, API contracts, and versioning.
    </span>
    <span class="newstyle-card__cta">View API overview →</span>
  </a>

  <a href="#what-k0rdent-ai-solves" class="newstyle-card">
    <span class="newstyle-card__title">What {{{ docsVersionInfo.k0rdentName }}} solves</span>
    <span class="newstyle-card__desc">
      Tech stack complexity, operational efficiency, and customer experience challenges in AI infrastructure.
    </span>
    <span class="newstyle-card__cta">Learn more →</span>
  </a>

  <a href="#key-capabilities" class="newstyle-card">
    <span class="newstyle-card__title">Key capabilities</span>
    <span class="newstyle-card__desc">
      Composable infrastructure, GPU efficiency, multi-tenancy, observability, and workload convergence.
    </span>
    <span class="newstyle-card__cta">Explore →</span>
  </a>
</div>

</div>

<h2 id="what-k0rdent-ai-solves">What k0rdent AI solves</h2>

Modern AI infrastructure faces three core challenges:

* **Tech stack complexity**: Divergent tooling, APIs, and security controls across providers and regions make it hard to standardize operations as hardware and platforms proliferate.

* **Operational efficiency**: Without centralized visibility, cluster health, state, cost, and risk drift across environments, creating instability and compliance gaps.

* **Customer experience**: Slow time-to-value onboarding new GPU capacity and services, difficulty maximizing utilization while meeting strict tenant requirements, and hard multi-tenancy raise the bar for reliability and isolation.

{{{ docsVersionInfo.k0rdentName }}} addresses these challenges through Kubernetes-native composability and multi-cluster automation. Infrastructure and services are defined as templates and reconciled continuously into running environments, enabling repeatable rollout across many clusters and sites with controlled drift and upgrades.

<h2 id="key-capabilities">Key capabilities</h2>

* **Composable infrastructure**: Define infrastructure and services as templates, reconcile them across clusters and sites.
* **GPU efficiency**: GPU partitioning, topology-aware scheduling, and allocation strategies to improve utilization.
* **Hardware-enforced multi-tenancy**: Tenant isolation across GPU allocation, virtualization, and network boundaries.
* **Integrated observability and FinOps**: Centralized monitoring and cost reporting for operational control and tenant-level consumption reporting.
* **Workload convergence**: Support for VM and container workloads under one operational model.

## Platform components

{{{ docsVersionInfo.k0rdentName }}} consists of three main components:

* **{{{ docsVersionInfo.k0rdentName }}} Cluster Manager (KCM)**: Deployment and lifecycle management of Kubernetes clusters, including configuration, updates, and other CRUD operations.

* **{{{ docsVersionInfo.k0rdentName }}} State Manager (KSM)**: Installation and lifecycle management of deployed services. KSM leverages [Project Sveltos](https://github.com/projectsveltos/sveltos) for an increasing amount of functionality.

* **{{{ docsVersionInfo.k0rdentName }}} Observability and FinOps (KOF)**: Cluster and beach-head services monitoring, events and log management, with integrated cost reporting and tenant-level consumption tracking.
