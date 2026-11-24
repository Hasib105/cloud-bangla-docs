# Service Map — কোন কাজে কোন service

> একই কাজ চার cloud-এ কোন নামে পাওয়া যায়, এক টেবিলে। প্রতিটি group-এর শেষ কলামে সেই বিষয়ের অধ্যায়ের লিংক আছে।
> নাম **নভেম্বর ২০২৬** অনুযায়ী। Cloud কোম্পানিগুলো নাম বদলায় — পুরোনো নাম `[...]` বন্ধনীতে দেওয়া হয়েছে।

!!! abstract "টেবিল কীভাবে পড়বেন"
    - **কাজ / Concept** — আগে ধারণাটা চিনুন, তারপর ওই সারি ধরে নিজের cloud-এর নাম দেখুন।
    - **—** মানে সেই cloud-এ সরাসরি সমতুল্য native service নেই।
    - **(third-party / Marketplace)** মানে কাজটা হয়, কিন্তু সাধারণত তৃতীয় পক্ষের product (Marketplace থেকে) ব্যবহার করতে হয়।
    - "≈" মানে প্রায় সমান — পুরোপুরি এক নয়; পার্থক্য সংশ্লিষ্ট অধ্যায়ে আছে।
    - Service-এর সংখ্যা ও নাম দ্রুত বদলায়। সন্দেহ হলে প্রতিটি cloud-এর official docs দেখুন: [AWS](https://docs.aws.amazon.com), [Google Cloud](https://cloud.google.com/docs), [Azure](https://learn.microsoft.com/azure), [OCI](https://docs.oracle.com/en-us/iaas).

## ০১ · Cloud-এর ভিত্তি

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| ভৌগোলিক এলাকা | Region | Region | Region | Region | [01](../01-cloud-fundamentals.md) |
| Region-এর ভেতরে আলাদা data center | Availability Zone (AZ) | Zone | Availability Zone | Availability Domain (AD) + Fault Domain | [01](../01-cloud-fundamentals.md) |
| Web console | AWS Management Console | Google Cloud console | Azure portal | OCI Console | [01](../01-cloud-fundamentals.md) |
| Browser-এর ভেতরে terminal | AWS CloudShell | Cloud Shell | Azure Cloud Shell | OCI Cloud Shell | [01](../01-cloud-fundamentals.md) |
| Service outage / health দেখা | AWS Health Dashboard | Google Cloud Service Health | Azure Service Health | OCI Status / Console notifications | [01](../01-cloud-fundamentals.md) |
| তৃতীয় পক্ষের software কেনা | AWS Marketplace | Google Cloud Marketplace | Azure Marketplace | Oracle Cloud Marketplace | [01](../01-cloud-fundamentals.md) |
| Paid support plan | AWS Support plans (Developer/Business/Enterprise) | Google Cloud Customer Care | Azure support plans | Oracle Cloud Support | [01](../01-cloud-fundamentals.md) |

## ০২ · Account, Organization ও Hierarchy

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| সবার উপরের root | Organization (management account) | Organization (domain-এর সাথে যুক্ত) | Tenant (Entra tenant) + Tenant Root Group | Tenancy (root compartment) | [02](../02-accounts-and-hierarchy.md) |
| Billing ও resource-এর মূল সীমানা | AWS account | Project | Subscription | Compartment (billing tenancy-তে) | [02](../02-accounts-and-hierarchy.md) |
| Account/project-এর group | Organizational Unit (OU) | Folder | Management group | Compartment (nested) | [02](../02-accounts-and-hierarchy.md) |
| Resource-এর logical ঝুড়ি | Resource Groups (tag-ভিত্তিক) | — (Project-ই ঝুড়ি) | Resource group | Compartment | [02](../02-accounts-and-hierarchy.md) |
| Billing account | Billing (payer/management account) | Cloud Billing account | Billing account / profile (Microsoft Customer Agreement) | Tenancy-level billing (Cost Management and Billing) | [02](../02-accounts-and-hierarchy.md) |
| Metadata চিহ্ন (খরচ ভাগ, automation) | Tags | Labels (+ Tags = আলাদা, policy-র জন্য) | Tags | Tags (free-form / defined) | [02](../02-accounts-and-hierarchy.md) |
| উপর থেকে নিয়ম চাপানো (guardrail) | Service Control Policies (SCP) | Organization Policy Service | Azure Policy | Security Zones / Compartment quotas | [02](../02-accounts-and-hierarchy.md) |
| Multi-account শুরু-কাঠামো (landing zone) | AWS Control Tower | Cloud Foundation Toolkit / landing zone blueprints | Azure Landing Zones (ALZ) | OCI Landing Zones | [02](../02-accounts-and-hierarchy.md) |

!!! note "কঠিন mapping — Hierarchy"
    - **GCP Project ≈ AWS account ≈ Azure Subscription**: তিনটিই "billing ও permission-এর মূল সীমানা"। কিন্তু OCI-তে আলাদা account/subscription নেই — একটাই **Tenancy**, আর ভেতরে **Compartment** দিয়ে ভাগ করা হয়।
    - **OCI Compartment** AWS OU + account-এর মিশ্রণের মতো: nested করা যায়, আবার policy ও খরচ-ট্র্যাকিংয়ের সীমানাও এটাই। প্রায় প্রতিটি OCI command-এ compartment OCID লাগে।
    - **Azure Resource group** আলাদা জিনিস: একটা Subscription-এর ভেতরে resource-এর lifecycle group। GCP/AWS-এ এর সরাসরি সমতুল্য নেই।

## ০৩ · Identity ও Access (IAM)

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Resource-এর permission নিয়ন্ত্রণ | IAM (JSON policy) | Cloud IAM (role binding) | Azure RBAC (role assignment) | OCI IAM policies (policy statement) | [03](../03-identity-and-access.md) |
| Identity/directory (কে আপনি) | IAM users (+ IAM Identity Center) | Cloud Identity / Google Workspace | Microsoft Entra ID [old: Azure Active Directory] | OCI IAM Identity Domains | [03](../03-identity-and-access.md) |
| কর্মীদের SSO (workforce) | IAM Identity Center [old: AWS SSO] | Cloud Identity + Workforce Identity Federation | Entra ID (SSO, Conditional Access) | Identity Domains (SSO, federation) | [03](../03-identity-and-access.md) |
| Program/VM-এর নিজের identity | IAM role (instance profile) | Service account | Managed identity | Instance principal (dynamic group) | [03](../03-identity-and-access.md) |
| Long-lived key ছাড়া বাইরের system (CI) থেকে login | IAM OIDC identity provider + role | Workload Identity Federation | Federated credential (Entra app / managed identity) | Identity propagation trust (Identity Domains) | [03](../03-identity-and-access.md) |
| গ্রাহক login (CIAM) | Amazon Cognito | Identity Platform | Microsoft Entra External ID [old: Azure AD B2C] | Identity Domains (external users) | [03](../03-identity-and-access.md) |
| Managed Active Directory | AWS Managed Microsoft AD (Directory Service) | Managed Service for Microsoft Active Directory | Microsoft Entra Domain Services | — (VM-এ নিজে AD চালান) | [03](../03-identity-and-access.md) |
| Extra permission ধরা / কমানো | IAM Access Analyzer | IAM Recommender / Policy Analyzer | Access reviews (Entra ID Governance) | Policy review (Cloud Guard / Audit) | [03](../03-identity-and-access.md) |
| Permission-এর সীমা (guardrail) | Permissions boundary / SCP | IAM Deny policy / Organization Policy | Deny assignments / Azure Policy | Policy conditions (`where`) | [03](../03-identity-and-access.md) |

!!! note "কঠিন mapping — Identity"
    - **Azure-এ দুটো আলাদা জিনিস**: *Microsoft Entra ID* = "ব্যবহারকারী কে" (directory, login, MFA, groups) আর *Azure RBAC* = "Azure resource-এ কী করতে পারে" (role assignment)। AWS/GCP/OCI-তে সাধারণত এই দুই কাজ একই IAM-এর ভেতরে।
    - **GCP-তে role bindings resource-এর গায়ে লাগে**, user-এর গায়ে policy লাগে না (AWS-এর মতো)। OCI-তে policy লেখা হয় বাক্য আকারে: `Allow group X to manage object-family in compartment Y`।
    - **Service account (GCP) বনাম managed identity (Azure) বনাম IAM role (AWS) বনাম instance principal (OCI)**: চারটিই "মানুষ নয়, কোড-এর identity" — password/key না রেখে।

## ০৪ · Virtual Machine (Compute)

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Virtual machine | Amazon EC2 | Compute Engine | Azure Virtual Machines | OCI Compute (instances) | [04](../04-compute-vms.md) |
| VM-এর আকার/ধরন | Instance type (`t3.micro`) | Machine type (`e2-medium`) | VM size (`Standard_B2s`) | Shape (`VM.Standard.E5.Flex`) | [04](../04-compute-vms.md) |
| Disk/OS image | AMI | Image (+ image family) | Image (Azure Compute Gallery) | Custom image / Platform image | [04](../04-compute-vms.md) |
| Auto-scaling VM group | Auto Scaling group (ASG) | Managed Instance Group (MIG) | Virtual Machine Scale Sets (VMSS) | Instance pool (+ autoscaling config) | [04](../04-compute-vms.md) |
| সস্তা, বন্ধ-হতে-পারে VM | Spot Instances | Spot VMs [old: Preemptible VMs] | Azure Spot VMs | Preemptible instances | [04](../04-compute-vms.md) |
| নিজস্ব physical server (dedicated) | Dedicated Hosts | Sole-tenant nodes | Azure Dedicated Host | Dedicated Virtual Machine Hosts | [04](../04-compute-vms.md) |
| Bare metal | `*.metal` instances | Bare metal machine types (C3 metal) | BareMetal Infrastructure (বিশেষ কিছু workload) | Bare Metal shapes (`BM.*`) | [04](../04-compute-vms.md) |
| Boot-এ script চালানো | User data | Startup script (metadata) | Custom data / cloud-init / VM extension | cloud-init (user data) | [04](../04-compute-vms.md) |
| SSH ছাড়া VM-এ ঢোকা | Systems Manager Session Manager / EC2 Instance Connect | OS Login / IAP TCP forwarding | Azure Bastion / Entra login for Linux | OCI Bastion / Compute Instance Agent | [04](../04-compute-vms.md) |
| সহজ PaaS web hosting | Elastic Beanstalk | App Engine | Azure App Service | — (Container Instances / Compute) | [04](../04-compute-vms.md) |
| সহজ VPS (flat দামের VM) | Amazon Lightsail | — | — | — | [04](../04-compute-vms.md) |
| Batch job | AWS Batch | Batch | Azure Batch | — | [04](../04-compute-vms.md) |
| VMware workload | Amazon Elastic VMware Service (EVS) | Google Cloud VMware Engine | Azure VMware Solution | Oracle Cloud VMware Solution | [04](../04-compute-vms.md) |

## ০৫ · Networking

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Private network | VPC (regional) | VPC network (**global**) | Virtual Network (VNet, regional) | Virtual Cloud Network (VCN, regional) | [05](../05-networking.md) |
| Subnet | Subnet (এক AZ-তে) | Subnet (regional, সব zone-এ) | Subnet (regional) | Subnet (regional বা AD-specific) | [05](../05-networking.md) |
| Route table | Route table | Routes (VPC-র সাথে যুক্ত) | Route table (UDR) | Route table | [05](../05-networking.md) |
| Internet-এ বের হওয়া/ঢোকা | Internet Gateway (IGW) | Default internet gateway route (implicit) | Public IP + default system route (implicit) | Internet Gateway | [05](../05-networking.md) |
| Private VM-এর outbound internet | NAT Gateway | Cloud NAT | Azure NAT Gateway | NAT Gateway | [05](../05-networking.md) |
| Instance-level firewall (stateful) | Security Group | VPC firewall rules (+ firewall policies, tags/service accounts দিয়ে target) | Network Security Group (NSG) — NIC বা subnet-এ | Network Security Group (NSG) | [05](../05-networking.md) |
| Subnet-level firewall | Network ACL (NACL, stateless) | Hierarchical / network firewall policies | NSG (subnet-এ লাগালে) | Security List (subnet-এ) | [05](../05-networking.md) |
| Network peering | VPC Peering | VPC Network Peering | Virtual Network peering | Local / Remote Peering (LPG / DRG) | [05](../05-networking.md) |
| Hub-and-spoke / transit | Transit Gateway | Network Connectivity Center (NCC) | Virtual WAN / Virtual Hub | Dynamic Routing Gateway (DRG) | [05](../05-networking.md) |
| Private endpoint (service-এ private IP) | AWS PrivateLink (interface endpoint) | Private Service Connect (PSC) | Private Endpoint (Private Link) | Private Endpoint | [05](../05-networking.md) |
| Cloud-এর নিজস্ব service-এ private পথ | Gateway endpoint (S3, DynamoDB) | Private Google Access | Service endpoints | Service Gateway | [05](../05-networking.md) |
| Site-to-site VPN | AWS Site-to-Site VPN | Cloud VPN (HA VPN) | VPN Gateway (site-to-site) | Site-to-Site VPN (IPSec) | [05](../05-networking.md) |
| ব্যবহারকারীর remote VPN | AWS Client VPN | — (third-party / Marketplace) | VPN Gateway (point-to-site) | — (third-party / Marketplace) | [05](../05-networking.md) |
| Dedicated private লিংক | AWS Direct Connect | Cloud Interconnect | ExpressRoute | FastConnect | [05](../05-networking.md) |
| Managed network firewall | AWS Network Firewall | Cloud NGFW | Azure Firewall | OCI Network Firewall | [05](../05-networking.md) |
| Network traffic log | VPC Flow Logs | VPC Flow Logs | VNet flow logs (NSG flow logs-এর পরিবর্তে) | VCN Flow Logs | [05](../05-networking.md) |
| Managed bastion | — (Session Manager ব্যবহার হয়) | Identity-Aware Proxy (IAP) TCP forwarding | Azure Bastion | OCI Bastion | [05](../05-networking.md) |

!!! note "কঠিন mapping — Networking"
    - **GCP VPC global**: একটা VPC সব region জুড়ে, subnet শুধু regional। AWS/Azure/OCI-তে network নিজেই regional — region জুড়তে peering/transit লাগে।
    - **GCP-তে আলাদা "Security Group" নেই**: firewall rules VPC-র স্তরে থাকে, আর network tag বা service account দিয়ে target ঠিক করা হয়।
    - **Azure NSG** subnet **অথবা** NIC-এ লাগে (দুই জায়গাতেই থাকলে দুটোই খাটে)। **OCI**-তে NSG (VNIC-স্তরে) ও Security List (subnet-স্তরে) দুটোই আছে।
    - **Azure NSG flow logs** নতুন করে তৈরি করা বন্ধ হয়ে যাচ্ছে ও পরে retire হবে; নতুন কাজে *VNet flow logs* ব্যবহার করুন।

## ০৬ · Storage

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Object storage | Amazon S3 | Cloud Storage (GCS) | Azure Blob Storage | OCI Object Storage | [06](../06-storage.md) |
| Object-এর container | Bucket | Bucket | Storage account → Container | Bucket (namespace-এর ভেতরে) | [06](../06-storage.md) |
| Block storage (VM disk) | Amazon EBS | Persistent Disk / Hyperdisk | Azure Managed Disks | OCI Block Volume | [06](../06-storage.md) |
| Network file system (NFS) | Amazon EFS | Filestore | Azure Files (NFS/SMB) | OCI File Storage | [06](../06-storage.md) |
| Enterprise file (NetApp / Windows / Lustre) | Amazon FSx | Google Cloud NetApp Volumes | Azure NetApp Files | — (third-party / Marketplace) | [06](../06-storage.md) |
| Cold/archive storage | S3 Glacier storage classes | Archive storage class | Blob Archive access tier | Archive Storage tier | [06](../06-storage.md) |
| Object lifecycle নিয়ম | S3 Lifecycle | Object Lifecycle Management | Blob lifecycle management | Object lifecycle policies | [06](../06-storage.md) |
| Object-এর পুরোনো version | S3 Versioning | Object Versioning | Blob versioning | Object Versioning | [06](../06-storage.md) |
| সময়-সীমিত ডাউনলোড লিংক | Presigned URL | Signed URL | SAS token (Shared Access Signature) | Pre-Authenticated Request (PAR) | [06](../06-storage.md) |
| Disk snapshot | EBS snapshot | Disk snapshot | Managed disk snapshot | Block/Boot volume backup | [06](../06-storage.md) |
| কেন্দ্রীয় backup service | AWS Backup | Backup and DR Service | Azure Backup | Volume backup policies (per-service) | [06](../06-storage.md) |
| Online data transfer | AWS DataSync | Storage Transfer Service | Azure Storage Mover / AzCopy | OCI Storage Gateway / rclone | [06](../06-storage.md) |
| Offline (physical device) transfer | AWS Snow Family | Transfer Appliance | Azure Data Box | OCI Data Transfer Appliance | [06](../06-storage.md) |
| On-prem থেকে cloud storage-এ file gateway | AWS Storage Gateway | — (third-party / Marketplace) | Azure File Sync | OCI Storage Gateway | [06](../06-storage.md) |

!!! note "কঠিন mapping — Storage"
    - **Azure Blob**-এ bucket সরাসরি নেই: আগে *Storage account* বানাতে হয় (এটাই Blob, File, Queue, Table-এর ঘর), তার ভেতরে *Container*।
    - **OCI Object Storage**-এ প্রতিটি tenancy-র একটা *namespace* আছে — URL ও CLI-তে এটা লাগে।
    - Signed URL-এর চার রূপ: AWS **presigned URL**, GCP **signed URL**, Azure **SAS**, OCI **PAR** — চারটিরই কাজ "অ্যাকাউন্ট ছাড়া নির্দিষ্ট সময়ের জন্য এক object-এ access"।
    - AWS Snow Family / Transfer Appliance / Data Box-এর availability region ও সময়ভেদে বদলায়; অর্ডার করার আগে official page দেখুন।

## ০৭ · Database

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Managed PostgreSQL/MySQL | Amazon RDS | Cloud SQL | Azure Database for PostgreSQL / MySQL (Flexible Server) | OCI Database with PostgreSQL / MySQL HeatWave | [07](../07-databases.md) |
| Cloud-native, বেশি performance-এর relational | Amazon Aurora | AlloyDB for PostgreSQL | Azure SQL Database Hyperscale | Autonomous Database | [07](../07-databases.md) |
| Globally distributed SQL | Amazon Aurora DSQL | Cloud Spanner | — (closest: Cosmos DB / SQL Hyperscale) | Globally Distributed Autonomous Database | [07](../07-databases.md) |
| SQL Server | RDS for SQL Server | Cloud SQL for SQL Server | Azure SQL Database / SQL Managed Instance | — (VM-এ নিজে) | [07](../07-databases.md) |
| Oracle Database | RDS for Oracle / Oracle Database@AWS | Oracle Database@Google Cloud | Oracle Database@Azure | Base Database / Exadata / Autonomous Database | [07](../07-databases.md) |
| Key-value / serverless NoSQL | Amazon DynamoDB | Firestore (Datastore mode) | Azure Cosmos DB (NoSQL API) | OCI NoSQL Database | [07](../07-databases.md) |
| Document database (app-facing) | DynamoDB / Amazon DocumentDB | Firestore | Azure Cosmos DB | Autonomous JSON Database | [07](../07-databases.md) |
| Wide-column (বড় scale, time-series) | Amazon Keyspaces | Cloud Bigtable | Cosmos DB for Apache Cassandra | — | [07](../07-databases.md) |
| MongoDB-compatible | Amazon DocumentDB | Firestore (MongoDB compatibility) | Cosmos DB for MongoDB | Autonomous JSON Database (MongoDB API) | [07](../07-databases.md) |
| In-memory cache (Redis/Valkey) | Amazon ElastiCache | Memorystore | Azure Managed Redis [old: Azure Cache for Redis] | OCI Cache | [07](../07-databases.md) |
| Graph database | Amazon Neptune | Spanner Graph | Cosmos DB for Apache Gremlin | Autonomous Database (Graph) | [07](../07-databases.md) |
| Search engine | Amazon OpenSearch Service | — (Vertex AI Search / Elastic on Marketplace) | Azure AI Search | OCI Search Service with OpenSearch | [07](../07-databases.md) |
| Database migration | AWS Database Migration Service (DMS) | Database Migration Service | Azure Database Migration Service | OCI Database Migration | [07](../07-databases.md) |

!!! note "কঠিন mapping — Database"
    - **Aurora ≈ AlloyDB**: দুটোই "PostgreSQL-compatible, storage আলাদা করা cloud-native engine"। Azure-এ ঠিক এক জিনিস নেই; PostgreSQL-এর জন্য Flexible Server-ই সাধারণ উত্তর।
    - **OCI-এর শক্তি Oracle Database**: Autonomous Database / Exadata / Base Database। Oracle Database@AWS/Azure/Google Cloud মানে Oracle hardware অন্য cloud-এর data center-এ বসানো, Oracle চালায়।
    - Cosmos DB একাধিক API দেয় (NoSQL, MongoDB, Cassandra, Gremlin…), তাই এক service একাধিক সারিতে এসেছে।
    - Azure Cache for Redis-এর পুরোনো tier-গুলো retire হচ্ছে; নতুন কাজে Azure Managed Redis দেখুন।

## ০৮ · Container ও Kubernetes

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Container image registry | Amazon ECR | Artifact Registry [old: Container Registry] | Azure Container Registry (ACR) | OCI Registry (OCIR) | [08](../08-containers-kubernetes.md) |
| Managed Kubernetes | Amazon EKS | Google Kubernetes Engine (GKE) | Azure Kubernetes Service (AKS) | Oracle Kubernetes Engine (OKE) | [08](../08-containers-kubernetes.md) |
| Node-ব্যবস্থাপনা ছাড়া Kubernetes | EKS Auto Mode / EKS on Fargate | GKE Autopilot | AKS Automatic | OKE Virtual Nodes | [08](../08-containers-kubernetes.md) |
| Node group | Managed node group | Node pool | Node pool | Node pool | [08](../08-containers-kubernetes.md) |
| Non-Kubernetes container orchestrator | Amazon ECS | — (Cloud Run ≈ সরলতম বিকল্প) | Azure Container Apps | OCI Container Instances | [08](../08-containers-kubernetes.md) |
| Pod-কে cloud permission | IRSA / EKS Pod Identity | Workload Identity Federation for GKE | Microsoft Entra Workload ID | OKE Workload Identity | [08](../08-containers-kubernetes.md) |
| Service mesh | AWS App Mesh (ECS Service Connect-এ সরানো হচ্ছে) | Cloud Service Mesh | Istio-based service mesh add-on for AKS | OCI Service Mesh | [08](../08-containers-kubernetes.md) |
| Kubernetes-এ ingress | AWS Load Balancer Controller (ALB/NLB) | GKE Ingress / Gateway API | Application Gateway for Containers / ingress-nginx | OCI Native Ingress / Load Balancer | [08](../08-containers-kubernetes.md) |

## ০৯ · Serverless

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Function (event-driven code) | AWS Lambda | Cloud Run functions [old: Cloud Functions] | Azure Functions | OCI Functions | [09](../09-serverless.md) |
| Serverless container (HTTP service) | AWS App Runner / ECS on Fargate | Cloud Run | Azure Container Apps | OCI Container Instances | [09](../09-serverless.md) |
| Run-to-completion container job | ECS task on Fargate / AWS Batch | Cloud Run jobs | Container Apps jobs | OCI Container Instances | [09](../09-serverless.md) |
| API gateway | Amazon API Gateway | API Gateway / Apigee | Azure API Management | OCI API Gateway | [09](../09-serverless.md) |
| Workflow orchestration | AWS Step Functions | Workflows | Logic Apps / Durable Functions | — (Functions + Events দিয়ে নিজে) | [09](../09-serverless.md) |
| Schedule (cron) | Amazon EventBridge Scheduler | Cloud Scheduler | Logic Apps recurrence / Functions timer trigger | — (Resource Scheduler শুধু start/stop-এর জন্য) | [09](../09-serverless.md) |
| Edge-এ code | CloudFront Functions / Lambda@Edge | Service Extensions | Front Door Rules Engine | — | [09](../09-serverless.md) |

## ১০ · Load Balancing, Autoscaling, DNS ও CDN

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| L7 (HTTP/HTTPS) load balancer | Application Load Balancer (ALB) | Application Load Balancer (global/regional) | Application Gateway | Load Balancer (flexible, L7) | [10](../10-load-balancing-scaling-dns-cdn.md) |
| L4 (TCP/UDP) load balancer | Network Load Balancer (NLB) | Network Load Balancer (proxy/passthrough) | Azure Load Balancer | Network Load Balancer | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Global entry (anycast / edge) | Global Accelerator / CloudFront | Global external Application LB | Azure Front Door | — | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Appliance-এর সামনে LB (inspection) | Gateway Load Balancer | — (next-hop internal LB) | Gateway Load Balancer | — | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Health check | Target group health check | Health checks | Health probes | Health Checks (backend set) | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Autoscaling | EC2 Auto Scaling / Application Auto Scaling | Autoscaler (MIG) | Azure Monitor autoscale (VMSS) | Autoscaling (instance pool) | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Public DNS | Amazon Route 53 | Cloud DNS | Azure DNS | OCI DNS | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Private DNS | Route 53 private hosted zone | Cloud DNS private zone | Azure Private DNS | Private DNS zone (private view) | [10](../10-load-balancing-scaling-dns-cdn.md) |
| DNS-ভিত্তিক traffic routing | Route 53 routing policies | Cloud DNS routing policies | Azure Traffic Manager | Traffic Management Steering Policies | [10](../10-load-balancing-scaling-dns-cdn.md) |
| CDN | Amazon CloudFront | Cloud CDN / Media CDN | Azure Front Door (CDN ফিচারসহ) | — (third-party / Marketplace, যেমন Akamai/Cloudflare) | [10](../10-load-balancing-scaling-dns-cdn.md) |
| TLS certificate (managed) | AWS Certificate Manager (ACM) | Certificate Manager (Google-managed certs) | App Service Managed Certificate / Key Vault certificates | OCI Certificates | [10](../10-load-balancing-scaling-dns-cdn.md) |
| Domain কেনা | Route 53 Registered Domains | Cloud Domains | App Service Domains | — (third-party registrar) | [10](../10-load-balancing-scaling-dns-cdn.md) |

!!! note "কঠিন mapping — Load balancer"
    - **GCP-তে "load balancer" একটাই পরিবার**, ধরন বদলায় scope-এ: global বনাম regional, external বনাম internal, proxy বনাম passthrough।
    - **Azure-এ দুইটি গুলিয়ে ফেলা নাম**: *Load Balancer* = L4, *Application Gateway* = L7 (regional), *Front Door* = L7 global (CDN + WAF-সহ)।
    - **Azure CDN-এর পুরোনো SKU**-গুলো phase out হচ্ছে; নতুন কাজে Front Door।

## ১১ · Messaging ও Event

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Message queue | Amazon SQS | Pub/Sub (pull subscription) / Cloud Tasks | Service Bus queues / Storage Queues | OCI Queue | [11](../11-messaging-and-events.md) |
| Publish/Subscribe (fan-out) | Amazon SNS | Pub/Sub | Service Bus topics / Event Grid | OCI Notifications | [11](../11-messaging-and-events.md) |
| Event routing (resource event → target) | Amazon EventBridge | Eventarc | Azure Event Grid | OCI Events | [11](../11-messaging-and-events.md) |
| Event streaming | Kinesis Data Streams | Pub/Sub | Azure Event Hubs | OCI Streaming | [11](../11-messaging-and-events.md) |
| Managed Apache Kafka | Amazon MSK | Managed Service for Apache Kafka | Event Hubs (Kafka endpoint) | OCI Streaming (Kafka-compatible API) | [11](../11-messaging-and-events.md) |
| Dead-letter queue (DLQ) | SQS DLQ | Pub/Sub dead-letter topic | Service Bus dead-letter sub-queue | OCI Queue dead letter queue | [11](../11-messaging-and-events.md) |
| Email পাঠানো | Amazon SES | — (third-party / Marketplace, যেমন SendGrid) | Azure Communication Services (Email) | OCI Email Delivery | [11](../11-messaging-and-events.md) |
| SMS / mobile push | SNS (SMS / mobile push) | Firebase Cloud Messaging | Azure Communication Services / Notification Hubs | OCI Notifications (SMS, নির্দিষ্ট region-এ) | [11](../11-messaging-and-events.md) |

## ১২ · Monitoring, Logging ও Tracing

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Metric ও alarm | Amazon CloudWatch (Metrics, Alarms) | Cloud Monitoring (alerting policies) | Azure Monitor Metrics / Alerts | OCI Monitoring (Alarms) | [12](../12-observability.md) |
| Log জমানো | CloudWatch Logs | Cloud Logging | Azure Monitor Logs (Log Analytics workspace) | OCI Logging | [12](../12-observability.md) |
| Log query | CloudWatch Logs Insights | Logs Explorer (Logging query language) | Log Analytics (KQL) | OCI Logging Analytics / Log Search | [12](../12-observability.md) |
| Distributed tracing / APM | AWS X-Ray / CloudWatch Application Signals | Cloud Trace | Application Insights | OCI Application Performance Monitoring (APM) | [12](../12-observability.md) |
| Dashboard | CloudWatch Dashboards | Cloud Monitoring dashboards | Azure Dashboards / Workbooks | Console dashboards (Monitoring, Logging Analytics) | [12](../12-observability.md) |
| Managed Prometheus | Amazon Managed Service for Prometheus | Google Cloud Managed Service for Prometheus | Azure Monitor managed service for Prometheus | — (third-party / Marketplace) | [12](../12-observability.md) |
| Managed Grafana | Amazon Managed Grafana | — (third-party / Marketplace) | Azure Managed Grafana | — (third-party / Marketplace) | [12](../12-observability.md) |
| Audit log (কে কী করল) | AWS CloudTrail | Cloud Audit Logs | Azure Activity Log | OCI Audit | [12](../12-observability.md) |
| Resource inventory ও পরিবর্তনের ইতিহাস | AWS Config | Cloud Asset Inventory | Azure Resource Graph | OCI Search (Resource Search) / Tenancy Explorer | [12](../12-observability.md) |
| Synthetic / uptime check | CloudWatch Synthetics | Uptime checks (Cloud Monitoring) | Application Insights availability tests | OCI Health Checks / APM Synthetic Monitoring | [12](../12-observability.md) |
| VM patch ও ops automation | AWS Systems Manager | VM Manager (OS Config) | Azure Update Manager / Azure Automation | OS Management Hub | [12](../12-observability.md) |
| Vendor-neutral instrumentation | OpenTelemetry (ADOT) | OpenTelemetry (Ops Agent / OTLP) | OpenTelemetry (Azure Monitor distro) | OpenTelemetry (APM OTLP) | [12](../12-observability.md) |

!!! note "কঠিন mapping — Observability"
    - **AWS CloudWatch একাই** metric + log + alarm + dashboard। GCP-তে তা **Cloud Monitoring + Cloud Logging** দুই service। Azure-এ **Azure Monitor** ছাতার নিচে Metrics, Log Analytics, Application Insights। OCI-তে **Monitoring, Logging, Logging Analytics, APM** আলাদা।
    - **Activity Log (Azure)** subscription-স্তরের control-plane event; resource-এর ভেতরের log চাইলে *diagnostic settings* দিয়ে Log Analytics-এ পাঠাতে হয়।

## ১৩ · Security

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Encryption key management | AWS KMS | Cloud KMS | Azure Key Vault (Keys) | OCI Vault (KMS) | [13](../13-security.md) |
| Secret সংরক্ষণ (password, API key) | AWS Secrets Manager | Secret Manager | Azure Key Vault (Secrets) | OCI Vault (Secrets) | [13](../13-security.md) |
| Certificate ও private CA | ACM / AWS Private CA | Certificate Manager / Certificate Authority Service | Key Vault Certificates | OCI Certificates (CA ও certificate) | [13](../13-security.md) |
| Hardware Security Module (HSM) | AWS CloudHSM | Cloud HSM | Azure Managed HSM / Dedicated HSM | OCI Vault (HSM-backed / Dedicated KMS) | [13](../13-security.md) |
| Web Application Firewall | AWS WAF | Google Cloud Armor | Azure WAF (Front Door / Application Gateway) | OCI WAF | [13](../13-security.md) |
| DDoS সুরক্ষা | AWS Shield (Standard/Advanced) | Cloud Armor (DDoS protection) | Azure DDoS Protection | Built-in DDoS protection (OCI WAF সহ) | [13](../13-security.md) |
| Security posture (misconfiguration ধরা) | AWS Security Hub | Security Command Center | Microsoft Defender for Cloud | OCI Cloud Guard | [13](../13-security.md) |
| Threat detection | Amazon GuardDuty | SCC Event Threat Detection | Defender for Cloud (workload protection) | Cloud Guard (Threat Detector) | [13](../13-security.md) |
| Vulnerability scanning | Amazon Inspector | Artifact Analysis / SCC | Defender for Cloud (vulnerability assessment) | Vulnerability Scanning Service | [13](../13-security.md) |
| SIEM | Amazon Security Lake + partner SIEM | Google Security Operations (SecOps) | Microsoft Sentinel | — (Logging Analytics / third-party / Marketplace) | [13](../13-security.md) |
| Sensitive data খোঁজা (PII) | Amazon Macie | Sensitive Data Protection | Microsoft Purview | Data Safe (database-এর জন্য) | [13](../13-security.md) |
| Compliance report ডাউনলোড | AWS Artifact | Compliance Reports Manager | Service Trust Portal | Oracle Cloud Compliance documents | [13](../13-security.md) |

!!! note "কঠিন mapping — Security"
    - **Azure Key Vault** একটাই service তিন কাজে: key, secret, certificate। AWS/GCP-তে এগুলো আলাদা service (KMS / Secrets Manager / ACM)। OCI-তে key ও secret দুটোই **Vault**-এর ভেতরে।
    - **Security Hub + GuardDuty (AWS)** একসাথে মোটামুটি **Security Command Center (GCP)**, **Defender for Cloud (Azure)**, **Cloud Guard (OCI)**-এর কাজ করে।

## ১৪ · Infrastructure as Code

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Native IaC | AWS CloudFormation | Infrastructure Manager [Deployment Manager deprecated] | ARM templates / Bicep | OCI Resource Manager (Terraform-ভিত্তিক) | [14](../14-infrastructure-as-code.md) |
| Code দিয়ে IaC লেখা | AWS CDK | — (Terraform / Pulumi) | Bicep / Pulumi | — (Terraform / Pulumi) | [14](../14-infrastructure-as-code.md) |
| Terraform provider | `aws` | `google` (`google-beta`) | `azurerm` (`azapi`) | `oci` | [14](../14-infrastructure-as-code.md) |
| Terraform remote state ঘর | S3 backend (locking সহ) | `gcs` backend | `azurerm` backend (Blob) | Object Storage (S3-compatible endpoint) | [14](../14-infrastructure-as-code.md) |
| Template catalog / reusable stack | AWS Service Catalog | Service Catalog / Infrastructure Manager | Template Specs / Deployment Stacks | Resource Manager stacks | [14](../14-infrastructure-as-code.md) |

## ১৫ · CI/CD ও DevOps

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Git repository | AWS CodeCommit (বেশিরভাগ দল GitHub/GitLab নেয়) | — (GitHub/GitLab; Cloud Source Repositories নতুন user-এর জন্য বন্ধ) | Azure Repos (Azure DevOps) | OCI DevOps code repositories | [15](../15-cicd-devops.md) |
| Build | AWS CodeBuild | Cloud Build | Azure Pipelines | OCI DevOps build pipelines | [15](../15-cicd-devops.md) |
| Pipeline / orchestration | AWS CodePipeline | Cloud Build triggers / Cloud Deploy | Azure Pipelines | OCI DevOps deployment pipelines | [15](../15-cicd-devops.md) |
| Progressive delivery (rollout) | AWS CodeDeploy | Cloud Deploy | Azure Pipelines environments / deployment strategies | OCI DevOps deployment strategies | [15](../15-cicd-devops.md) |
| Package/artifact repository | AWS CodeArtifact | Artifact Registry | Azure Artifacts | OCI Artifact Registry | [15](../15-cicd-devops.md) |
| GitHub Actions → cloud (OIDC, key ছাড়া) | IAM OIDC provider + role | Workload Identity Federation | Entra federated credential | Identity propagation trust (Identity Domains) | [15](../15-cicd-devops.md) |

## ১৬ · Data ও Analytics

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Data warehouse | Amazon Redshift | BigQuery | Azure Synapse Analytics / Microsoft Fabric (Warehouse) | Autonomous Data Warehouse | [16](../16-data-and-analytics.md) |
| Data lake storage | S3 + AWS Lake Formation | Cloud Storage + BigLake | ADLS Gen2 (Blob) + OneLake (Fabric) | Object Storage | [16](../16-data-and-analytics.md) |
| Lake-এ সরাসরি SQL query | Amazon Athena | BigQuery external tables / BigLake | Synapse serverless SQL pool | Autonomous Database external tables | [16](../16-data-and-analytics.md) |
| Spark / Hadoop | Amazon EMR | Dataproc | HDInsight / Synapse Spark / Fabric | OCI Data Flow / Big Data Service | [16](../16-data-and-analytics.md) |
| ETL / data integration | AWS Glue | Dataflow / Cloud Data Fusion | Azure Data Factory (Fabric Data Factory) | OCI Data Integration | [16](../16-data-and-analytics.md) |
| Data catalog | AWS Glue Data Catalog | Dataplex Universal Catalog [old: Data Catalog] | Microsoft Purview Data Catalog | OCI Data Catalog | [16](../16-data-and-analytics.md) |
| Stream processing | Managed Service for Apache Flink | Dataflow | Azure Stream Analytics | OCI GoldenGate Stream Analytics | [16](../16-data-and-analytics.md) |
| Workflow orchestration (Airflow) | Amazon MWAA | Cloud Composer | Data Factory (Workflow Orchestration Manager) | — (OCI Data Integration pipelines) | [16](../16-data-and-analytics.md) |
| BI / dashboard | Amazon QuickSight (Quick Suite) | Looker / Looker Studio | Power BI | Oracle Analytics Cloud | [16](../16-data-and-analytics.md) |

## ১৭ · AI ও Machine Learning

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| ML platform (train/deploy) | Amazon SageMaker AI | Vertex AI | Azure Machine Learning | OCI Data Science | [17](../17-ai-ml.md) |
| Managed GenAI (foundation model API) | Amazon Bedrock | Vertex AI (Gemini, Model Garden) | Azure OpenAI in Microsoft Foundry [old: Azure AI Foundry] | OCI Generative AI | [17](../17-ai-ml.md) |
| AI agent platform | Bedrock AgentCore | Vertex AI Agent Builder | Foundry Agent Service | OCI Generative AI Agents | [17](../17-ai-ml.md) |
| Image/video analysis | Amazon Rekognition | Cloud Vision API / Video Intelligence | Azure AI Vision | OCI Vision | [17](../17-ai-ml.md) |
| Speech ↔ text | Amazon Transcribe / Polly | Speech-to-Text / Text-to-Speech | Azure AI Speech | OCI Speech (speech-to-text) | [17](../17-ai-ml.md) |
| Translation | Amazon Translate | Cloud Translation | Azure AI Translator | OCI Language (translation) | [17](../17-ai-ml.md) |
| Text analysis (NLP) | Amazon Comprehend | Natural Language API | Azure AI Language | OCI Language | [17](../17-ai-ml.md) |
| Document থেকে data বের করা | Amazon Textract | Document AI | Azure AI Document Intelligence | OCI Document Understanding | [17](../17-ai-ml.md) |
| Vector search | OpenSearch / Aurora pgvector / S3 Vectors | Vertex AI Vector Search / AlloyDB | Azure AI Search / Cosmos DB | Oracle AI Vector Search (Autonomous Database) | [17](../17-ai-ml.md) |
| GPU VM | EC2 P/G instances | GPU machine types (A3, G2…) | ND / NC-series VMs | GPU shapes (`BM.GPU.*`, `VM.GPU.*`) | [17](../17-ai-ml.md) |
| নিজস্ব AI chip | Trainium / Inferentia | TPU | — | — | [17](../17-ai-ml.md) |

## ১৮ · Cost Management (FinOps)

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Billing console | Billing and Cost Management | Cloud Billing | Microsoft Cost Management + Billing | Cost Management and Billing | [18](../18-cost-management.md) |
| খরচ বিশ্লেষণ | AWS Cost Explorer | Cost reports / Cost table | Cost analysis | Cost Analysis | [18](../18-cost-management.md) |
| Budget ও alert | AWS Budgets | Budgets & alerts (Cloud Billing) | Cost Management budgets | Budgets | [18](../18-cost-management.md) |
| Detailed billing data export | Data Exports (Cost and Usage Report) | Billing export to BigQuery | Cost Management exports | Cost and Usage Reports | [18](../18-cost-management.md) |
| খরচ ভাগ করা (allocation) | Cost allocation tags | Labels | Tags (+ cost allocation rules) | Cost-tracking tags | [18](../18-cost-management.md) |
| Commitment ছাড় | Savings Plans / Reserved Instances | Committed Use Discounts (CUDs) | Reservations / Savings plan for compute | Universal Credits (commitment) | [18](../18-cost-management.md) |
| Rightsizing সুপারিশ | AWS Compute Optimizer / Trusted Advisor | Recommender (VM rightsizing) | Azure Advisor | OCI Cloud Advisor | [18](../18-cost-management.md) |
| Cost anomaly | AWS Cost Anomaly Detection | Cost anomaly detection | Cost Management anomaly alerts | — (Budgets + Cost Analysis) | [18](../18-cost-management.md) |
| Price estimate calculator | AWS Pricing Calculator | Google Cloud Pricing Calculator | Azure Pricing calculator | OCI Cost Estimator | [18](../18-cost-management.md) |
| Free tier | AWS Free Tier | Free Trial + Free Tier (Always Free products) | Azure free account | Free Trial + Always Free | [18](../18-cost-management.md) |

!!! note "কঠিন mapping — Cost"
    - **Savings Plans (AWS) ≈ Flex CUDs (GCP) ≈ Azure Savings plan for compute**: নির্দিষ্ট instance নয়, ঘণ্টা-প্রতি খরচের প্রতিশ্রুতি। **Reserved Instances / Reservations / resource-based CUDs** নির্দিষ্ট ধরনের resource-এ লক।
    - **OCI-তে commitment** আসে **Universal Credits** চুক্তির মাধ্যমে; কোন service-এ কত ছাড় তা চুক্তিতে ঠিক হয়।
    - কোনো cloud-এর দাম এখানে দেওয়া হয়নি — দাম region ও সময় অনুযায়ী বদলায়। সবসময় official pricing page ও calculator দেখুন।

## ১৯ · Architecture, HA/DR ও Multi-cloud

| কাজ / Concept | AWS | GCP | Azure | OCI | অধ্যায় |
|---|---|---|---|---|---|
| Design best-practice framework | AWS Well-Architected Framework | Google Cloud Well-Architected Framework [old: Architecture Framework] | Azure Well-Architected Framework | OCI Well-Architected Framework | [19](../19-architecture-ha-dr.md) |
| Reference architecture library | AWS Architecture Center | Google Cloud Architecture Center | Azure Architecture Center | Oracle Architecture Center | [19](../19-architecture-ha-dr.md) |
| On-prem-এ cloud hardware | AWS Outposts | Google Distributed Cloud | Azure Local [old: Azure Stack HCI] | OCI Dedicated Region / Cloud@Customer | [19](../19-architecture-ha-dr.md) |
| Hybrid/multi-cloud একক management | AWS Systems Manager (hybrid) / EKS Anywhere | GKE Enterprise [old: Anthos] | Azure Arc | — (Cloud@Customer / Oracle Database@ models) | [19](../19-architecture-ha-dr.md) |
| Disaster recovery service | AWS Elastic Disaster Recovery | Backup and DR Service | Azure Site Recovery | Full Stack Disaster Recovery | [19](../19-architecture-ha-dr.md) |
| Server migration (lift-and-shift) | AWS Application Migration Service (MGN) | Migrate to Virtual Machines | Azure Migrate | OCI Cloud Migrations | [19](../19-architecture-ha-dr.md) |
| Multi-region database | Aurora Global Database / DynamoDB global tables | Spanner multi-region | Cosmos DB multi-region writes | Autonomous Data Guard / Globally Distributed DB | [19](../19-architecture-ha-dr.md) |

!!! note "কঠিন mapping — Hybrid"
    - **Azure Local (পুরোনো নাম Azure Stack HCI)** on-prem hardware-এ চলা Azure stack; **Azure Arc** আলাদা — আপনার যেকোনো server/Kubernetes-কে Azure থেকে নিয়ন্ত্রণ করার control plane।
    - **OCI Dedicated Region** পুরো OCI region আপনার data center-এ; **Cloud@Customer** ছোট, নির্দিষ্ট service-এর (যেমন Exadata) জন্য।

## বিশেষ সতর্কতা

!!! warning "নামের ফাঁদ"
    - এই টেবিল **সাদৃশ্য** দেখায়, **সমতুল্যতা** নয়। দুটো service একই কাজ করলেও limit, scope, pricing model ও API আলাদা হতে পারে — অধ্যায়গুলোর "মিল ও অমিল" অংশ দেখুন।
    - পুরোনো নাম এখনো docs/blog/Stack Overflow-এ ছড়ানো: *Azure AD → Microsoft Entra ID*, *Cloud Functions → Cloud Run functions*, *Container Registry → Artifact Registry*, *Anthos → GKE Enterprise*, *Azure Stack HCI → Azure Local*, *AWS SSO → IAM Identity Center*, *Preemptible VM → Spot VM*।
    - Preview/GA অবস্থা, region availability ও নতুন-ব্যবহারকারীর জন্য বন্ধ হওয়া service দ্রুত বদলায়; production সিদ্ধান্তের আগে official docs দেখে নিন।
