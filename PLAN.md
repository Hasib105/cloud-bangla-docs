# Cloud বাংলা ডকস — Plan (Opus)

## Goal

One Bangla documentation site that teaches cloud **by common topic** (compute, networking, IAM, …) and, for
every topic, shows **which service does the same job on AWS, GCP, Azure and OCI** — with concept first,
then side-by-side platform tabs, CLI, Terraform, differences and gotchas. Zero → advanced.

Base material: `.ref/aws-original/` (hrmsway/aws-bangla-docs, AWS only). Use it as a reference for depth
and tone, but **rewrite** — never copy its running example.

## Hard rules

- **No AUREV, Luna, travel, concierge, booking platform** or anything from the reference's running example.
  The only running example is **`shopbd`** — a fictional small Bangladeshi online shop
  (web frontend + REST API + PostgreSQL + product-image storage + order emails + nightly reports).
  Resource names: `shopbd-prod-vpc`, `shopbd-images`, `shopbd-api`, `ops@example.com`, domain `shopbd.example.com`.
- Bangla prose, English technical terms (like the reference). Code, commands, comments inside code,
  resource names: **English only**.
- Current service names (Nov 2026): Microsoft Entra ID (not Azure AD), GCP Artifact Registry
  (not Container Registry), GCP Infrastructure Manager (Deployment Manager is deprecated), Azure Monitor /
  Log Analytics, OCI Compartments, etc. Note deprecations where a reader may meet the old name.
- No exact prices. Say what drives cost and point to the official pricing page / calculator.
- No invented CLI flags. If unsure, show the most common documented form and keep it short.

## Site structure (MkDocs Material)

| Section | Pages |
|---|---|
| হোম | `index.md` (Opus) |
| মূল ধারণা (common topics) | 19 chapters below |
| প্ল্যাটফর্ম গাইড | `platforms/aws.md`, `gcp.md`, `azure.md`, `oci.md` |
| রেফারেন্স | `reference/service-map.md`, `reference/cli-cheatsheet.md`, `reference/glossary.md` |

### Common-topic chapters (`docs/NN-slug.md`)

| # | File | Topic | Must cover |
|---|---|---|---|
| 01 | `01-cloud-fundamentals.md` | Cloud-এর ভিত্তি | IaaS/PaaS/SaaS/FaaS, region/zone/edge per platform, shared responsibility, pricing models (on-demand, reserved/commitment, spot/preemptible), free tiers, the 4 providers' strengths, how to pick |
| 02 | `02-accounts-and-hierarchy.md` | Account, Organization ও Resource hierarchy | AWS Organizations/OUs/accounts; GCP Organization/folders/projects; Azure Tenant/Management groups/Subscriptions/Resource groups; OCI Tenancy/Compartments; billing accounts, tags/labels, landing zones |
| 03 | `03-identity-and-access.md` | Identity ও Access (IAM) | users/groups/roles/service accounts/managed identities/instance principals, policies (JSON vs role bindings vs RBAC vs OCI policy statements), least privilege, MFA, SSO/federation, workload identity federation, conditions |
| 04 | `04-compute-vms.md` | Virtual Machine (Compute) | EC2 / Compute Engine / Azure VMs / OCI Compute; machine families & shapes, images, SSH keys, metadata & startup scripts, disks, spot, instance groups/VMSS/instance pools, dedicated hosts, bare metal |
| 05 | `05-networking.md` | Networking | VPC/VNet/VCN (GCP VPC is global!), subnets (regional vs zonal), route tables, IGW/NAT, SG/NACL vs firewall rules vs NSG vs security lists, peering, transit (TGW/NCC/vWAN/DRG), private endpoints, VPN, Direct Connect/Interconnect/ExpressRoute/FastConnect |
| 06 | `06-storage.md` | Storage | object (S3/GCS/Blob/Object Storage), storage classes & lifecycle, versioning, presigned/SAS/PAR URLs; block (EBS/PD-Hyperdisk/Managed Disks/Block Volume), snapshots; file (EFS/Filestore/Azure Files/File Storage); archive; data transfer |
| 07 | `07-databases.md` | Database | managed relational (RDS/Aurora, Cloud SQL/AlloyDB/Spanner, Azure SQL/Flexible Server, OCI MySQL HeatWave/Autonomous DB/Base DB), NoSQL (DynamoDB/Firestore/Bigtable/Cosmos DB/OCI NoSQL), cache (ElastiCache/Memorystore/Azure Cache for Redis/OCI Cache), backups, replicas, HA, picking one |
| 08 | `08-containers-kubernetes.md` | Container ও Kubernetes | registries (ECR/Artifact Registry/ACR/OCIR), managed K8s (EKS/GKE/AKS/OKE), autopilot/fargate-style modes, ECS, node pools, ingress, K8s IAM integration |
| 09 | `09-serverless.md` | Serverless | functions (Lambda/Cloud Run functions/Azure Functions/OCI Functions), serverless containers (App Runner + ECS Fargate/Cloud Run/Container Apps/OCI Container Instances), API gateways, triggers, cold start, limits |
| 10 | `10-load-balancing-scaling-dns-cdn.md` | Load balancing, Autoscaling, DNS ও CDN | L4 vs L7 LB per platform, health checks, autoscaling policies, Route 53/Cloud DNS/Azure DNS/OCI DNS, routing policies, CloudFront/Cloud CDN/Azure Front Door/OCI CDN options, TLS certificates |
| 11 | `11-messaging-and-events.md` | Messaging ও Event | queue (SQS/Pub/Sub/Service Bus/Storage Queue/OCI Queue), pub/sub (SNS/Pub/Sub/Event Grid/OCI Notifications), event bus (EventBridge/Eventarc/Event Grid/OCI Events), streaming (Kinesis/MSK, Pub/Sub, Event Hubs, OCI Streaming), workflows (Step Functions/Workflows/Logic Apps & Durable Functions/OCI —), patterns: fan-out, DLQ, idempotency |
| 12 | `12-observability.md` | Monitoring, Logging ও Tracing | CloudWatch/Cloud Monitoring+Logging/Azure Monitor+Log Analytics+App Insights/OCI Monitoring+Logging+APM, metrics, alarms, log queries (Logs Insights, Log Analytics KQL, Logging query language, OCI search), tracing (X-Ray/Cloud Trace/App Insights/APM), audit logs (CloudTrail/Cloud Audit Logs/Activity Log/OCI Audit), OpenTelemetry |
| 13 | `13-security.md` | Security | key management (KMS/Cloud KMS/Key Vault/OCI Vault), secrets, certificates, WAF, DDoS (Shield/Cloud Armor/DDoS Protection), posture & threat detection (Security Hub+GuardDuty/Security Command Center/Defender for Cloud/Cloud Guard), encryption at rest/in transit, compliance |
| 14 | `14-infrastructure-as-code.md` | Infrastructure as Code (Terraform) | **Terraform as the main thread** (providers aws/google/azurerm/oci, state & remote backends per cloud, modules, workspaces, plan/apply, drift), OpenTofu note, native tools (CloudFormation & CDK, Infrastructure Manager, ARM/Bicep, OCI Resource Manager), the same `shopbd` network+VM+bucket in all 4 |
| 15 | `15-cicd-devops.md` | CI/CD ও DevOps | native pipelines (CodePipeline/CodeBuild, Cloud Build/Cloud Deploy, Azure DevOps Pipelines, OCI DevOps), GitHub Actions with OIDC to each cloud (no long-lived keys), deployment strategies, artifact registries |
| 16 | `16-data-and-analytics.md` | Data ও Analytics | warehouse (Redshift/BigQuery/Synapse & Fabric/Autonomous Data Warehouse), data lake, ETL (Glue/Dataflow & Dataproc/Data Factory/OCI Data Integration), query-on-lake (Athena/BigQuery external/Synapse serverless), BI |
| 17 | `17-ai-ml.md` | AI ও Machine Learning | ML platforms (SageMaker/Vertex AI/Azure ML/OCI Data Science), managed GenAI (Bedrock/Vertex AI Gemini/Azure OpenAI & AI Foundry/OCI Generative AI), prebuilt AI APIs (vision, speech, translate), GPUs, responsible use |
| 18 | `18-cost-management.md` | Cost management (FinOps) | billing consoles, budgets & alerts, cost allocation via tags/labels, commitments (Savings Plans/CUDs/Azure Reservations & Savings Plan/OCI Universal Credits), rightsizing tools, egress costs, free tiers, cost checklist |
| 19 | `19-architecture-ha-dr.md` | Architecture, HA/DR ও Multi-cloud | well-architected frameworks (AWS WAF, Google Cloud Architecture Framework, Azure WAF, OCI best-practices framework), HA, RPO/RTO, DR strategies, multi-region, multi-cloud & hybrid (Outposts/Distributed Cloud/Arc/Cloud@Customer), migration (6R), full `shopbd` reference architecture on each cloud |

### Platform guides (`docs/platforms/*.md`)

Each: what makes this cloud different (mindset), sign-up & free tier, console tour, CLI install + auth
(`aws configure`/SSO, `gcloud init`, `az login`, `oci setup config`), first project hierarchy, naming quirks,
the platform's full service list grouped by category with a link to the common chapter that covers it,
certifications path, learning resources.

### Reference

- `service-map.md` — the master table: every category row × AWS/GCP/Azure/OCI columns (~80 rows).
- `cli-cheatsheet.md` — same task, 4 CLIs side by side (tabs), grouped by topic.
- `glossary.md` — A–Z terms, Bangla explanation, which cloud uses which word.

## Writers (Sonnet)

| Agent | Pages |
|---|---|
| W1 | 01, 02, 03 |
| W2 | 04, 05 |
| W3 | 06, 07, 16 |
| W4 | 08, 09, 10 |
| W5 | 11, 12, 13 |
| W6 | 14, 15, 18 |
| W7 | 17, 19 |
| W8 | platforms/aws, gcp, azure, oci |
| W9 | reference/service-map, cli-cheatsheet, glossary |

Every writer follows `STYLE.md`. Opus reviews: strict build, banned-word grep, spot read, nav.
