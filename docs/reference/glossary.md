# Glossary — A থেকে Z

> Cloud-এর শব্দগুলো সহজ বাংলায়, সাথে কোন cloud-এ একই জিনিসের কী নাম। নাম **নভেম্বর ২০২৬** অনুযায়ী।
> তুলনামূলক টেবিল দেখতে চাইলে [Service Map](service-map.md), কমান্ডের জন্য [CLI Cheatsheet](cli-cheatsheet.md)।

!!! tip "কীভাবে খুঁজবেন"
    পাতার উপরের search বক্সে ইংরেজি শব্দ লিখুন (যেমন `subnet`)। প্রতিটি শব্দের নিচে প্রথমে বাংলায় ব্যাখ্যা, তারপর **কোন cloud-এ কী নামে**। যেখানে চার cloud-এ একই নাম, সেখানে আলাদা করে লেখা হয়নি।

## A

Access key
:   Program বা CLI থেকে cloud-এ ঢোকার জন্য দীর্ঘমেয়াদি ID + secret জোড়া। চুরি গেলে বড় ক্ষতি, তাই যেখানে পারেন role/managed identity/federation ব্যবহার করুন।

    **কোন cloud-এ কী নামে:** AWS **access key** (IAM user) · GCP **service account key** (JSON) · Azure **client secret / certificate** (app registration) · OCI **API signing key**

ACL (Access Control List)
:   কে কী করতে পারবে তার তালিকা — object বা network স্তরে। আধুনিক cloud-এ policy/role সাধারণত ACL-এর চেয়ে ভালো।

    **কোন cloud-এ কী নামে:** AWS **Network ACL** (subnet) ও S3 ACL (legacy) · GCP **Object ACL** (legacy; IAM ভালো) · Azure **NSG rules** / Blob ACL (ADLS) · OCI **Security List** (subnet)

Alarm
:   Metric একটা সীমা পার হলে notification বা action চালু করার নিয়ম, যেমন "CPU ৮০%-এর বেশি ৫ মিনিট"।

    **কোন cloud-এ কী নামে:** AWS **CloudWatch Alarm** · GCP **alerting policy** (Cloud Monitoring) · Azure **metric alert rule** (Azure Monitor) · OCI **Alarm** (Monitoring)

Anycast
:   একই IP address বিশ্বের অনেক জায়গা থেকে প্রচার হয়; ব্যবহারকারী সবচেয়ে কাছেরটায় পৌঁছায়। Global load balancer ও CDN-এ ব্যবহৃত।

    **কোন cloud-এ কী নামে:** AWS **Global Accelerator** / CloudFront · GCP **global external Application LB** · Azure **Front Door** · OCI সাধারণত নেই (third-party / Marketplace)

API Gateway
:   আপনার backend API-র সামনের "প্রবেশদ্বার" — authentication, rate limit, routing, logging এক জায়গায়।

    **কোন cloud-এ কী নামে:** AWS **API Gateway** · GCP **API Gateway** / Apigee · Azure **API Management (APIM)** · OCI **API Gateway**

API key
:   ছোট একটা secret string, যা API call-এর সাথে পাঠিয়ে caller-কে চেনানো হয়। শক্তিশালী প্রমাণ নয় — সীমিত ব্যবহারে ঠিক আছে।

ARN / Resource ID
:   একটা resource-কে সারা cloud-এ অনন্যভাবে চেনার নাম। টেক্সটে লেখা পুরো ঠিকানা।

    **কোন cloud-এ কী নামে:** AWS **ARN** (`arn:aws:s3:::shopbd-images`) · GCP **resource name** (`projects/my-project-id/...`) · Azure **resource ID** (`/subscriptions/.../resourceGroups/...`) · OCI **OCID** (`ocid1.compartment.oc1..example`)

Auto Scaling
:   চাপ বাড়লে নিজে নিজে VM/instance বাড়ানো, কমলে কমানো — খরচ ও availability দুটোই সামলায়।

    **কোন cloud-এ কী নামে:** AWS **Auto Scaling group** · GCP **Autoscaler** (Managed Instance Group) · Azure **VM Scale Sets + autoscale** · OCI **Instance pool + autoscaling configuration**

Availability Zone (AZ)
:   একই region-এর ভেতরে আলাদা বিদ্যুৎ-নেটওয়ার্কসহ আলাদা data center। এক AZ নষ্ট হলে অন্যটা চালু থাকে।

    **কোন cloud-এ কী নামে:** AWS **Availability Zone** · GCP **Zone** · Azure **Availability Zone** · OCI **Availability Domain (AD)** (+ **Fault Domain**; সব region-এ একাধিক AD নেই)

## B

Backup
:   Data-র কপি, যাতে ভুলে মুছে গেলে বা নষ্ট হলে ফিরে পাওয়া যায়। Replica আর backup এক জিনিস নয় — replica-তে ভুল মোছাও ছড়িয়ে যায়।

    **কোন cloud-এ কী নামে:** AWS **AWS Backup** · GCP **Backup and DR Service** · Azure **Azure Backup** · OCI **Block/Boot Volume backups, Database backups** (per-service)

Bare metal
:   Virtualization ছাড়া সরাসরি physical server ভাড়া — সর্বোচ্চ performance বা বিশেষ license-এর জন্য।

    **কোন cloud-এ কী নামে:** AWS **`.metal` instances** · GCP **bare metal machine types** · Azure **BareMetal Infrastructure** (সীমিত) · OCI **Bare Metal shapes (`BM.*`)**

Bastion host
:   Private network-এর ভেতরের server-এ ঢোকার জন্য একটা নিয়ন্ত্রিত "দরজা"। আজকাল managed বা agent-ভিত্তিক বিকল্প বেশি নিরাপদ।

    **কোন cloud-এ কী নামে:** AWS **Session Manager** (Systems Manager) · GCP **IAP TCP forwarding** · Azure **Azure Bastion** · OCI **OCI Bastion**

Block storage
:   VM-এর সাথে লাগানো "হার্ড ডিস্ক" — raw disk, যার উপর filesystem বানান। (Object storage-এর সাথে গুলিয়ে ফেলবেন না।)

    **কোন cloud-এ কী নামে:** AWS **EBS** · GCP **Persistent Disk / Hyperdisk** · Azure **Managed Disks** · OCI **Block Volume**

Blue/green deployment
:   পুরোনো (blue) ও নতুন (green) দুই সংস্করণ পাশাপাশি চালিয়ে traffic একবারে সরিয়ে দেওয়া; সমস্যা হলে সাথে সাথে ফিরে আসা।

    **কোন cloud-এ কী নামে:** AWS **CodeDeploy blue/green** · GCP **Cloud Deploy / Cloud Run traffic split** · Azure **deployment slots (App Service)** · OCI **OCI DevOps blue-green strategy**

Bucket
:   Object storage-এর "ফোল্ডার-সদৃশ পাত্র", যার ভেতরে file (object) জমা হয়।

    **কোন cloud-এ কী নামে:** AWS **S3 bucket** · GCP **Cloud Storage bucket** · Azure **Blob container** (Storage account-এর ভেতরে) · OCI **Object Storage bucket**

Budget
:   মাসে কত খরচ করবেন তার সীমা — পার হওয়ার আগেই alert পাঠায়, সাধারণত খরচ আটকায় না।

    **কোন cloud-এ কী নামে:** AWS **AWS Budgets** · GCP **Cloud Billing budget** · Azure **Cost Management budget** · OCI **Budgets**

Burstable instance
:   স্বাভাবিক সময়ে কম CPU, দরকারে সাময়িক বেশি ব্যবহার করতে পারে এমন সস্তা VM — ছোট website/dev server-এর জন্য ভালো।

    **কোন cloud-এ কী নামে:** AWS **T-series** (`t3`, `t4g`) · GCP **E2 shared-core** (`e2-micro`…) · Azure **B-series** · OCI **Flex shapes with baseline OCPU utilization**

## C

Cache
:   বারবার লাগে এমন data দ্রুত memory-তে রাখা, যাতে database-এ বারবার যেতে না হয়।

    **কোন cloud-এ কী নামে:** AWS **ElastiCache** · GCP **Memorystore** · Azure **Azure Managed Redis** [old: Azure Cache for Redis] · OCI **OCI Cache**

Canary deployment
:   নতুন সংস্করণ প্রথমে অল্প ব্যবহারকারীকে (যেমন ৫%) দেখানো; ঠিক থাকলে ধীরে ধীরে বাড়ানো।

CDN (Content Delivery Network)
:   Image/video/JS-এর কপি ব্যবহারকারীর কাছাকাছি server-এ রেখে দ্রুত পৌঁছে দেওয়া।

    **কোন cloud-এ কী নামে:** AWS **CloudFront** · GCP **Cloud CDN / Media CDN** · Azure **Azure Front Door** · OCI সাধারণত third-party (Marketplace)

CIDR
:   IP range লেখার ধরন, যেমন `10.0.0.0/16`। `/` এর পরের সংখ্যা যত বড়, range তত ছোট। Network পরিকল্পনার ভিত্তি।

CI/CD
:   Code push হলে নিজে নিজে build, test ও deploy করার প্রক্রিয়া (Continuous Integration / Delivery)।

    **কোন cloud-এ কী নামে:** AWS **CodePipeline / CodeBuild** · GCP **Cloud Build / Cloud Deploy** · Azure **Azure Pipelines** · OCI **OCI DevOps**

Cloud Shell
:   Browser-এর ভেতরেই চলা terminal, CLI আগে থেকে install ও login করা — কিছু install না করেই শুরু করা যায়।

    **কোন cloud-এ কী নামে:** AWS **CloudShell** · GCP **Cloud Shell** · Azure **Azure Cloud Shell** · OCI **Cloud Shell**

Cloud-native
:   Cloud-এর সুবিধা (scale, managed service, automation) পুরো কাজে লাগিয়ে বানানো application — শুধু VM-এ পুরোনো app তুলে আনা নয়।

Cold start
:   অনেকক্ষণ ব্যবহার না হলে serverless function প্রথম call-এ চালু হতে যে বাড়তি সময় লাগে।

Commitment discount
:   ১ বা ৩ বছরের জন্য নির্দিষ্ট পরিমাণ ব্যবহারের প্রতিশ্রুতি দিলে ছাড়।

    **কোন cloud-এ কী নামে:** AWS **Savings Plans / Reserved Instances** · GCP **Committed Use Discounts (CUDs)** · Azure **Reservations / Savings plan** · OCI **Universal Credits commitment**

Compartment
:   OCI-র নিজস্ব ধারণা: Tenancy-র ভেতরে resource ভাগ করার logical ঘর (nested করা যায়) — policy, quota ও খরচ-ট্র্যাকিং এর উপর নির্ভর করে।

    **কোন cloud-এ কী নামে:** AWS **account / OU** এর কাছাকাছি · GCP **project / folder** · Azure **resource group / subscription** · OCI **Compartment**

Console
:   Cloud-এর web-ভিত্তিক graphical dashboard, যেখানে mouse দিয়ে resource বানানো যায়।

    **কোন cloud-এ কী নামে:** AWS **Management Console** · GCP **Google Cloud console** · Azure **Azure portal** · OCI **OCI Console**

Container
:   App + তার দরকারি সব library একসাথে প্যাক করা হালকা ইউনিট, যা যেকোনো জায়গায় একই রকম চলে (Docker)।

Control plane / Data plane
:   Control plane = resource বানানো, বদলানো, মোছার API (management)। Data plane = resource-এর ভেতরে আসল কাজ (যেমন file পড়া, query চালানো)। Permission ও audit log প্রায়ই এই দুই স্তরে আলাদা।

Cost allocation
:   কোন দল/প্রজেক্ট কত খরচ করল তা tag/label দিয়ে ভাগ করে দেখা।

    **কোন cloud-এ কী নামে:** AWS **cost allocation tags** · GCP **labels** (+ billing export) · Azure **tags / cost allocation rules** · OCI **cost-tracking tags**

Customer-managed key (CMK)
:   Encryption key নিজের নিয়ন্ত্রণে রাখা (rotate, disable করার ক্ষমতা সহ), cloud-এর ডিফল্ট key-র বদলে।

    **কোন cloud-এ কী নামে:** AWS **customer managed key (KMS)** · GCP **CMEK** (Cloud KMS) · Azure **customer-managed key** (Key Vault) · OCI **customer-managed key** (Vault)

## D

Data lake / Data warehouse
:   Data lake = সব ধরনের কাঁচা data সস্তায় জমানো (সাধারণত object storage)। Data warehouse = বিশ্লেষণের জন্য সাজানো structured data-র বিশাল SQL database।

    **কোন cloud-এ কী নামে:** Lake: S3 / Cloud Storage / ADLS Gen2 (Blob) / Object Storage · Warehouse: Redshift / BigQuery / Synapse & Fabric / Autonomous Data Warehouse

Dead-letter queue (DLQ)
:   যে message বারবার চেষ্টা করেও process হয়নি, সেগুলো আলাদা queue-তে রাখা — পরে দেখে সমাধান করার জন্য।

    **কোন cloud-এ কী নামে:** AWS **SQS DLQ** · GCP **Pub/Sub dead-letter topic** · Azure **Service Bus dead-letter queue** · OCI **Queue dead letter queue**

Dedicated host
:   শুধু আপনার জন্য নির্দিষ্ট physical server; license বা compliance-এর কারণে লাগে।

    **কোন cloud-এ কী নামে:** AWS **Dedicated Hosts** · GCP **Sole-tenant nodes** · Azure **Dedicated Host** · OCI **Dedicated Virtual Machine Hosts**

DDoS
:   বিপুল নকল traffic পাঠিয়ে সাইট অচল করার আক্রমণ।

    **কোন cloud-এ কী নামে:** AWS **Shield** · GCP **Cloud Armor** · Azure **DDoS Protection** · OCI **built-in DDoS protection + WAF**

Disaster recovery (DR)
:   পুরো region/data center নষ্ট হলে অন্য জায়গায় সিস্টেম ফিরিয়ে আনার পরিকল্পনা ও ব্যবস্থা (দেখুন RPO / RTO)।

    **কোন cloud-এ কী নামে:** AWS **Elastic Disaster Recovery** · GCP **Backup and DR Service** · Azure **Site Recovery** · OCI **Full Stack Disaster Recovery**

DNS
:   Domain name (`shopbd.example.com`) থেকে IP address খুঁজে দেওয়ার "ফোন বই"।

    **কোন cloud-এ কী নামে:** AWS **Route 53** · GCP **Cloud DNS** · Azure **Azure DNS** · OCI **OCI DNS**

Drift
:   Terraform/IaC-তে যা লেখা আছে আর আসলে cloud-এ যা আছে — এই দুইয়ের অমিল (কেউ console থেকে হাতে বদলালে হয়)।

Dynamic group
:   OCI-র ধারণা: নির্দিষ্ট নিয়মে মেলা instance-গুলোকে একটা "group" ধরে তাদের policy দেওয়া (instance principal)।

    **কোন cloud-এ কী নামে:** AWS **IAM role + instance profile** · GCP **service account attached to VM** · Azure **managed identity** · OCI **Dynamic group + policy**

## E

Edge location
:   বিশ্বজুড়ে ছড়ানো ছোট server-কেন্দ্র যেখানে CDN content রাখে — ব্যবহারকারীর খুব কাছে।

    **কোন cloud-এ কী নামে:** AWS **CloudFront edge location / Local Zone** · GCP **Edge PoP / Cloud CDN** · Azure **Front Door POP / Edge Zone** · OCI **FastConnect/edge PoP** (CDN সীমিত)

Egress
:   আপনার cloud থেকে বাইরে (internet বা অন্য region/cloud-এ) যাওয়া data। প্রায়ই এটাই সবচেয়ে অপ্রত্যাশিত খরচ। ভেতরে ঢোকা (ingress) সাধারণত বিনামূল্যে।

Elasticity
:   চাহিদা অনুযায়ী resource নিজে বাড়া-কমার ক্ষমতা। Scalability আরও ব্যাপক — elasticity হলো দ্রুত ও স্বয়ংক্রিয় বাড়া-কমা।

Encryption at rest / in transit
:   At rest = disk/storage-এ রাখা data এনক্রিপ্ট করা। In transit = network-এ চলার সময় (TLS)। দুটোই লাগে।

Endpoint
:   কোনো service-এ পৌঁছানোর ঠিকানা (URL/IP)। *Private endpoint* = সেই service-এর একটা IP আপনার private network-এর ভেতরে।

    **কোন cloud-এ কী নামে:** AWS **PrivateLink (interface endpoint)** · GCP **Private Service Connect** · Azure **Private Endpoint** · OCI **Private Endpoint**

Event-driven
:   "কিছু ঘটলে → code চলবে" ধরনের design; যেমন image upload হলে thumbnail বানানোর function।

    **কোন cloud-এ কী নামে:** AWS **EventBridge** · GCP **Eventarc** · Azure **Event Grid** · OCI **Events**

## F

Failover
:   মূল সিস্টেম নষ্ট হলে নিজে নিজে বা হাতে ব্যাকআপ সিস্টেমে চলে যাওয়া।

Fault domain
:   একই data center-এর ভেতরে আলাদা rack/hardware গ্রুপ, যাতে একসাথে সব VM নষ্ট না হয়।

    **কোন cloud-এ কী নামে:** AWS **Placement group (spread)** · GCP **Spread placement policy** · Azure **Fault domain** (Availability set) · OCI **Fault Domain**

Federation
:   আরেকটা identity system (company login, Google, GitHub) বিশ্বাস করে তাদের user-কে cloud-এ ঢুকতে দেওয়া — আলাদা password না বানিয়ে।

Firewall
:   কোন network traffic ঢুকতে/বেরোতে পারবে তার নিয়ম।

    **কোন cloud-এ কী নামে:** AWS **Security Group / NACL / Network Firewall** · GCP **VPC firewall rules / Cloud NGFW** · Azure **NSG / Azure Firewall** · OCI **NSG / Security List / Network Firewall**

FinOps
:   Cloud খরচ বোঝা, ভাগ করা ও কমানোর পেশাদার চর্চা — engineering ও finance একসাথে।

Flow log
:   Network-এ কোন IP থেকে কোন IP-তে কত traffic গেল তার রেকর্ড (সমস্যা ও নিরাপত্তা খোঁজার জন্য)।

    **কোন cloud-এ কী নামে:** AWS **VPC Flow Logs** · GCP **VPC Flow Logs** · Azure **VNet flow logs** · OCI **VCN Flow Logs**

Function (FaaS)
:   শুধু ছোট code লিখুন, server cloud চালায়; চলার সময়ের জন্যই খরচ।

    **কোন cloud-এ কী নামে:** AWS **Lambda** · GCP **Cloud Run functions** [old: Cloud Functions] · Azure **Azure Functions** · OCI **OCI Functions**

## G

Gateway
:   এক network থেকে অন্য network-এ যাওয়ার দরজা (internet, NAT, VPN, transit)। কোন cloud-এ কোন নাম — দেখুন Internet gateway, NAT।

GPU
:   গ্রাফিক্স/সমান্তরাল গণনার জন্য বিশেষ processor — AI model train ও inference-এ অপরিহার্য।

    **কোন cloud-এ কী নামে:** AWS **P/G instances** · GCP **A3/G2 GPU machine types** · Azure **ND/NC-series** · OCI **GPU shapes (`BM.GPU.*`)**

## H

Health check
:   Load balancer নিয়মিত backend-কে জিজ্ঞেস করে "ঠিক আছো?" — না থাকলে traffic পাঠানো বন্ধ করে।

    **কোন cloud-এ কী নামে:** AWS **target group health check** · GCP **health checks** · Azure **health probes** (Load Balancer) · OCI **Health Checks** (backend set)

High availability (HA)
:   কিছু অংশ নষ্ট হলেও সিস্টেম চালু থাকার design — সাধারণত একাধিক AZ ব্যবহার করে।

Hybrid cloud
:   নিজের data center/on-prem ও public cloud একসাথে ব্যবহার।

    **কোন cloud-এ কী নামে:** AWS **Outposts** · GCP **Google Distributed Cloud** · Azure **Azure Local + Arc** · OCI **Dedicated Region / Cloud@Customer**

Hypervisor
:   এক physical server-কে অনেক virtual machine-এ ভাগ করার software।

## I

IaaS / PaaS / SaaS
:   IaaS = শুধু VM/network/disk ভাড়া। PaaS = platform (runtime, database) ভাড়া — code দিন। SaaS = তৈরি software ব্যবহার (Gmail)। বিস্তারিত: [Cloud-এর ভিত্তি](../01-cloud-fundamentals.md)।

IaC (Infrastructure as Code)
:   Infrastructure console-এ ক্লিক না করে code/ফাইলে লেখা — version control, review ও পুনরায় তৈরি সহজ।

    **কোন cloud-এ কী নামে:** AWS **CloudFormation / CDK** · GCP **Infrastructure Manager** · Azure **ARM / Bicep** · OCI **Resource Manager** · চার cloud-এই **Terraform**

IAM
:   "কে কোন resource-এ কী করতে পারবে" ঠিক করার ব্যবস্থা। বিস্তারিত: [Identity ও Access](../03-identity-and-access.md)।

    **কোন cloud-এ কী নামে:** AWS **IAM** · GCP **Cloud IAM** · Azure **Microsoft Entra ID (identity) + Azure RBAC (permission)** · OCI **OCI IAM** (policies + Identity Domains)

Idempotency
:   একই অনুরোধ একাধিকবার চালালে ফল একই থাকা (যেমন order দুইবার এলেও একবারই নেওয়া)। Retry ও queue-ভিত্তিক সিস্টেমে জরুরি।

Identity provider (IdP)
:   যে system ব্যবহারকারীর পরিচয় যাচাই করে (login দেয়)।

    **কোন cloud-এ কী নামে:** AWS **IAM Identity Center / Cognito** · GCP **Cloud Identity / Identity Platform** · Azure **Microsoft Entra ID / Entra External ID** · OCI **Identity Domains**

Image
:   VM চালু করার template — OS ও software ভরা disk-এর snapshot।

    **কোন cloud-এ কী নামে:** AWS **AMI** · GCP **Image** (image family) · Azure **Image / Compute Gallery** · OCI **Custom image / Platform image**

Ingress
:   (১) Network-এ ঢোকা traffic। (২) Kubernetes-এ বাইরের HTTP traffic cluster-এর service-এ পৌঁছানোর নিয়ম — cloud-এর load balancer-এর সাথে জুড়ে যায়।

Instance
:   একটা চলমান VM (বা database server)। Cloud-ভেদে "VM" ও "instance" প্রায় সমার্থক।

Instance profile / Instance principal
:   VM-কে নিজস্ব identity দেওয়া, যাতে app-এর ভেতরে কোনো password/key রাখতে না হয়।

    **কোন cloud-এ কী নামে:** AWS **IAM role + instance profile** · GCP **attached service account** · Azure **managed identity** · OCI **instance principal**

Internet gateway
:   Private network ও public internet-এর মধ্যে দরজা।

    **কোন cloud-এ কী নামে:** AWS **Internet Gateway (IGW)** · GCP **default internet gateway route** (implicit) · Azure **implicit default route (public IP)** · OCI **Internet Gateway**

IOPS
:   Disk প্রতি সেকেন্ডে কতগুলো read/write operation পারে — disk-এর "গতি"-র একটা মাপ (আরেকটা throughput, MB/s)।

## J

JWT (JSON Web Token)
:   Login-এর পর পাওয়া ছোট signed token, যাতে user-এর তথ্য ও মেয়াদ থাকে। OIDC/OAuth-এর ভিত্তি।

## K

Key pair (SSH)
:   Public key (VM-এ রাখা) + private key (আপনার কাছে); password ছাড়া নিরাপদে SSH করার জন্য।

    **কোন cloud-এ কী নামে:** AWS **EC2 key pair** · GCP **metadata/OS Login SSH key** · Azure **SSH public key resource** · OCI **SSH key** (instance launch-এ)

KMS (Key Management Service)
:   Encryption key নিরাপদে বানানো, রাখা ও ঘোরানো (rotate)।

    **কোন cloud-এ কী নামে:** AWS **AWS KMS** · GCP **Cloud KMS** · Azure **Azure Key Vault** · OCI **OCI Vault (KMS)**

Kubernetes
:   অনেক container চালানো, scale করা ও সামলানোর open-source orchestrator।

    **কোন cloud-এ কী নামে:** AWS **EKS** · GCP **GKE** · Azure **AKS** · OCI **OKE**

## L

Landing zone
:   নতুন company-র cloud শুরুর "ভিত্তি কাঠামো" — multi-account, network, IAM, logging, guardrail আগে থেকে সাজানো।

    **কোন cloud-এ কী নামে:** AWS **Control Tower** · GCP **landing zone blueprints / Cloud Foundation Toolkit** · Azure **Azure Landing Zones** · OCI **OCI Landing Zones**

Latency
:   অনুরোধ পাঠিয়ে উত্তর আসতে যে সময় লাগে। ব্যবহারকারী যত দূরে, latency তত বেশি — তাই Bangladesh-এর জন্য Mumbai/Singapore region জনপ্রিয়।

Least privilege
:   কাজের জন্য যতটুকু permission লাগে ঠিক ততটুকুই দেওয়া — বেশি নয়।

Lifecycle policy
:   Object/data কত দিন পর সস্তা tier-এ যাবে বা মুছে যাবে তার স্বয়ংক্রিয় নিয়ম।

    **কোন cloud-এ কী নামে:** AWS **S3 Lifecycle** · GCP **Object Lifecycle Management** · Azure **Blob lifecycle management** · OCI **Object lifecycle policies**

Load balancer
:   আসা traffic একাধিক server-এ ভাগ করে দেওয়া (L4 = TCP/UDP, L7 = HTTP)।

    **কোন cloud-এ কী নামে:** AWS **ALB / NLB** · GCP **Application / Network Load Balancer** · Azure **Application Gateway (L7) / Load Balancer (L4) / Front Door** · OCI **Load Balancer / Network Load Balancer**

Log analytics
:   বিপুল log থেকে query দিয়ে খুঁজে প্যাটার্ন বের করা।

    **কোন cloud-এ কী নামে:** AWS **CloudWatch Logs Insights** · GCP **Logs Explorer / Log Analytics** · Azure **Log Analytics (KQL)** · OCI **Logging Analytics**

## M

Managed identity
:   Azure-এ resource (VM, App Service, Function)-কে দেওয়া নিজস্ব identity — password ছাড়াই অন্য service-এ ঢোকে।

    **কোন cloud-এ কী নামে:** AWS **IAM role** · GCP **service account** · Azure **managed identity** · OCI **instance principal / resource principal**

Managed service
:   যে service-এর patching, backup, availability cloud কোম্পানি সামলায়; আপনি শুধু ব্যবহার করেন (যেমন RDS, Cloud SQL)।

Management group
:   Azure-এ একাধিক Subscription-কে একসাথে নিয়ম ও permission দেওয়ার উপরের স্তর।

    **কোন cloud-এ কী নামে:** AWS **Organizational Unit (OU)** · GCP **Folder** · Azure **Management group** · OCI **Compartment (parent)**

Marketplace
:   তৃতীয় পক্ষের software/image কেনা বা চালানোর দোকান, বিল cloud-এর সাথেই।

    **কোন cloud-এ কী নামে:** AWS **AWS Marketplace** · GCP **Google Cloud Marketplace** · Azure **Azure Marketplace** · OCI **Oracle Cloud Marketplace**

Metric
:   সময়ের সাথে বদলানো সংখ্যা, যেমন CPU %, request সংখ্যা, latency।

MFA (Multi-Factor Authentication)
:   Password-এর সাথে দ্বিতীয় প্রমাণ (app code, security key)। Root/admin account-এ বাধ্যতামূলক ধরে নিন।

Multi-AZ
:   এক region-এর একাধিক AZ-তে সিস্টেম ছড়িয়ে রাখা — এক AZ নষ্ট হলেও চালু থাকে।

Multi-cloud
:   একাধিক cloud কোম্পানি একসাথে ব্যবহার। ঝুঁকি কমে, কিন্তু জটিলতা ও দক্ষতার খরচ বাড়ে।

## N

Namespace
:   (১) Kubernetes-এ resource আলাদা রাখার ভার্চুয়াল ভাগ। (২) OCI Object Storage-এ প্রতিটি tenancy-র একটি অনন্য namespace — bucket-এর URL ও CLI-তে লাগে। (৩) Azure Service Bus/Event Hubs namespace — একাধিক queue/topic-এর পাত্র।

NAT (Network Address Translation)
:   Private IP-র VM-কে internet-এ বের হতে দেয়, কিন্তু বাইরে থেকে ঢুকতে দেয় না।

    **কোন cloud-এ কী নামে:** AWS **NAT Gateway** · GCP **Cloud NAT** · Azure **NAT Gateway** · OCI **NAT Gateway**

Network ACL (NACL)
:   Subnet-স্তরের stateless firewall (AWS-এর ভাষায়)।

    **কোন cloud-এ কী নামে:** AWS **Network ACL** · GCP **firewall policies** · Azure **NSG (subnet-এ)** · OCI **Security List**

NoSQL
:   Table-row ছাড়া অন্য ধরনের database (key-value, document, wide-column, graph) — বড় scale ও নমনীয় structure-এর জন্য।

    **কোন cloud-এ কী নামে:** AWS **DynamoDB** · GCP **Firestore / Bigtable** · Azure **Cosmos DB** · OCI **OCI NoSQL Database**

NSG (Network Security Group)
:   Resource/NIC বা subnet-এ লাগানো stateful firewall rule-এর সেট।

    **কোন cloud-এ কী নামে:** AWS **Security Group** · GCP **VPC firewall rules** (tag/service account target) · Azure **NSG** · OCI **NSG** (+ Security List)

## O

Object storage
:   File-কে "object" হিসেবে (data + metadata + key) HTTP API দিয়ে রাখা — অসীম আকারের সস্তা storage; ছবি, backup, log-এর জন্য আদর্শ।

    **কোন cloud-এ কী নামে:** AWS **S3** · GCP **Cloud Storage** · Azure **Blob Storage** · OCI **Object Storage**

OCID
:   OCI-র প্রতিটি resource-এর অনন্য ID (`ocid1.instance.oc1..example`)। OCI CLI/Terraform-এ প্রায় সবখানে লাগে।

OIDC (OpenID Connect)
:   OAuth 2.0-র উপর বানানো login protocol। CI (যেমন GitHub Actions) থেকে long-lived key ছাড়া cloud-এ ঢুকতে ব্যবহার হয়।

    **কোন cloud-এ কী নামে:** AWS **IAM OIDC provider** · GCP **Workload Identity Federation** · Azure **Federated credential** · OCI **Identity propagation trust**

Orchestration
:   অনেক container/service/step সঠিক ক্রমে চালানো ও সামলানো — Kubernetes (container), Step Functions/Workflows (কাজের ধাপ)।

Organization
:   সবগুলো account/project-কে এক ছাতার নিচে এনে কেন্দ্রীয় billing, নিয়ম ও security।

    **কোন cloud-এ কী নামে:** AWS **Organizations** · GCP **Organization** (resource) · Azure **Tenant + Management groups** · OCI **Tenancy**

OU (Organizational Unit)
:   AWS-এ একাধিক account-কে দলবদ্ধ করার ধাপ, যার উপর SCP লাগে।

    **কোন cloud-এ কী নামে:** AWS **OU** · GCP **Folder** · Azure **Management group** · OCI **Compartment**

## P

Peering
:   দুটো private network সরাসরি জুড়ে দেওয়া, যাতে internet ছাড়াই কথা বলে।

    **কোন cloud-এ কী নামে:** AWS **VPC Peering** · GCP **VPC Network Peering** · Azure **VNet peering** · OCI **Local / Remote Peering (LPG / DRG)**

Pod
:   Kubernetes-এর ক্ষুদ্রতম ইউনিট — এক বা একাধিক container একসাথে।

Policy
:   Permission বা নিয়মের লিখিত রূপ। চার cloud-এ লেখার ধরন আলাদা: AWS **JSON policy** · GCP **role binding** · Azure **role assignment** · OCI **policy statement**।

Presigned URL
:   সময়-সীমিত ডাউনলোড/আপলোড লিংক, যা দিয়ে অ্যাকাউন্ট ছাড়াই একটা object-এ access মেলে।

    **কোন cloud-এ কী নামে:** AWS **Presigned URL** · GCP **Signed URL** · Azure **SAS token** · OCI **Pre-Authenticated Request (PAR)**

Principal
:   যে "কেউ/কিছু" action চালায় — user, group, role, service account ইত্যাদি। Policy-তে "কে" অংশটা।

    **কোন cloud-এ কী নামে:** AWS **principal** (IAM user/role) · GCP **member / principal** · Azure **security principal** (user, group, service principal, managed identity) · OCI **principal** (user, group, instance principal)

Private endpoint
:   (দেখুন Endpoint) — public internet ছাড়াই PaaS service-এ private IP দিয়ে যাওয়ার পথ।

Project
:   GCP-র মূল সীমানা: resource, billing, API ও permission এক project-এ। AWS **account**, Azure **Subscription** (+ Resource group)-এর কাছাকাছি।

Pub/Sub (Publish/Subscribe)
:   একজন message topic-এ পাঠায়, একাধিক গ্রাহক নিজেদের মতো পায় (fan-out)।

    **কোন cloud-এ কী নামে:** AWS **SNS** · GCP **Pub/Sub** · Azure **Service Bus topics / Event Grid** · OCI **Notifications**

## Q

Queue
:   Message লাইন ধরে রাখে, consumer নিজের গতিতে একটা একটা তুলে নেয় — হঠাৎ চাপ সামলায়।

    **কোন cloud-এ কী নামে:** AWS **SQS** · GCP **Pub/Sub (pull) / Cloud Tasks** · Azure **Service Bus queues / Storage Queues** · OCI **OCI Queue**

Quota (service limit)
:   Cloud কোম্পানির দেওয়া ঊর্ধ্বসীমা (যেমন region-এ সর্বোচ্চ ১০টা vCPU)। নতুন অ্যাকাউন্টে কম থাকে; বাড়ানোর অনুরোধ করতে হয়।

    **কোন cloud-এ কী নামে:** AWS **Service Quotas** · GCP **Quotas (IAM & Admin)** · Azure **Quotas** · OCI **Service Limits**

## R

RBAC (Role-Based Access Control)
:   Permission সরাসরি user-কে না দিয়ে "role"-এ জমা করে role দেওয়া। Azure-এ resource permission-এর নামই *Azure RBAC*; কাজ একই, Entra ID থেকে আলাদা।

Read replica
:   Database-এর পড়ার জন্য একটা অতিরিক্ত কপি — read-এর চাপ ভাগ করে। Backup বা automatic failover-এর বিকল্প নয়।

Region
:   ভৌগোলিক এলাকা যেখানে একাধিক data center আছে (যেমন Mumbai)। Latency, আইন ও দাম region-ভেদে আলাদা।

    **কোন cloud-এ কী নামে:** AWS **ap-south-1** · GCP **asia-south1** · Azure **centralindia** · OCI **ap-mumbai-1**

Reservation
:   নির্দিষ্ট ধরনের resource আগাম কিনে নিয়ে ছাড় পাওয়া (commitment discount-এর একরকম)।

    **কোন cloud-এ কী নামে:** AWS **Reserved Instances** · GCP **resource-based CUDs** · Azure **Reservations** · OCI **Universal Credits** (reserved-ধরনের ছাড় চুক্তিতে)

Resource group
:   Azure-এ একই কাজে লাগা resource-এর গ্রুপ — একসাথে বানানো, permission দেওয়া ও মোছা যায়।

    **কোন cloud-এ কী নামে:** AWS **Resource Groups (tag-ভিত্তিক) / CloudFormation stack** · GCP **Project** · Azure **Resource group** · OCI **Compartment**

Role
:   Permission-এর সেট। AWS-এ role আবার "পরিচয়" — কেউ *assume* করে অস্থায়ী credential পায়।

    **কোন cloud-এ কী নামে:** AWS **IAM role** · GCP **IAM role** (permission-এর সেট) · Azure **role definition** · OCI **— (policy statement দিয়ে group-কে সরাসরি permission)**

Route table
:   Network packet কোন পথে যাবে (internet, NAT, VPN, peering) তার নিয়মের তালিকা।

    **কোন cloud-এ কী নামে:** AWS **Route table** · GCP **Routes** · Azure **Route table (UDR)** · OCI **Route table**

RPO / RTO
:   RPO (Recovery Point Objective) = সর্বোচ্চ কতটুকু data হারানো মেনে নেওয়া যায় (সময়ে)। RTO (Recovery Time Objective) = সিস্টেম কত সময়ের মধ্যে ফিরে আসা দরকার।

## S

SaaS
:   (দেখুন IaaS / PaaS / SaaS) তৈরি software, browser-এ ব্যবহার — Gmail, Microsoft 365।

Savings Plan
:   নির্দিষ্ট instance নয়, ঘণ্টা-প্রতি খরচের প্রতিশ্রুতি (১ বা ৩ বছর) দিয়ে ছাড়।

    **কোন cloud-এ কী নামে:** AWS **Savings Plans** · GCP **Flex CUDs** · Azure **Savings plan for compute** · OCI **Universal Credits**

Scale up / scale out
:   Scale up (vertical) = একই server-কে বড় করা। Scale out (horizontal) = আরও server যোগ করা। Cloud-এ scale out-ই সাধারণত পছন্দ।

Secret
:   Password, API key, token — যা কোডে বা repo-তে রাখা চলে না; secret store-এ রাখতে হয়।

    **কোন cloud-এ কী নামে:** AWS **Secrets Manager** · GCP **Secret Manager** · Azure **Key Vault (Secrets)** · OCI **Vault (Secrets)**

Security group
:   (দেখুন NSG) AWS-এর stateful instance-level firewall।

Serverless
:   Server চালানো/সামলানোর ঝামেলা নেই, ব্যবহার অনুযায়ী খরচ — function, serverless container, managed database ইত্যাদি।

    **কোন cloud-এ কী নামে:** AWS **Lambda, Fargate, DynamoDB** · GCP **Cloud Run, Cloud Run functions, Firestore** · Azure **Functions, Container Apps, Cosmos DB** · OCI **Functions, Container Instances**

Service account
:   মানুষ নয়, application-এর identity। GCP-তে এটাই ঘরের নাম; অন্যত্র সমতুল্য: AWS **IAM role** · Azure **managed identity / service principal** · OCI **instance principal / dynamic group**

Service principal
:   Azure-এ application-এর identity — Entra ID-তে বানানো (app registration থেকে)। CI/CD বা script থেকে Azure-এ ঢুকতে ব্যবহৃত; আধুনিক পছন্দ managed identity বা federated credential।

Service mesh
:   Microservice-এর মধ্যকার traffic-এ encryption, retry, observability যোগ করার স্তর (Istio, Linkerd)।

    **কোন cloud-এ কী নামে:** AWS **App Mesh** (পরিবর্তন আসছে; ECS Service Connect) · GCP **Cloud Service Mesh** · Azure **Istio add-on for AKS** · OCI **OCI Service Mesh**

Shape
:   OCI-তে VM/instance-এর আকারের নাম (CPU+memory-র ধরন), যেমন `VM.Standard.E5.Flex`। Flex shape-এ OCPU ও memory নিজে ঠিক করা যায়।

    **কোন cloud-এ কী নামে:** AWS **Instance type** · GCP **Machine type** · Azure **VM size** · OCI **Shape**

Shared responsibility model
:   Cloud কোম্পানি দায়ী cloud-এর *নিরাপত্তার* (hardware, data center); আপনি দায়ী cloud-এর *ভেতরের* সবকিছুর (data, IAM, config)। বিস্তারিত: [Cloud-এর ভিত্তি](../01-cloud-fundamentals.md)।

SLA / SLO
:   SLA = কোম্পানির আইনি প্রতিশ্রুতি (যেমন ৯৯.৯৯% uptime, না মানলে credit)। SLO = আপনার নিজের লক্ষ্য।

Snapshot
:   Disk বা database-এর নির্দিষ্ট মুহূর্তের কপি।

    **কোন cloud-এ কী নামে:** AWS **EBS / RDS snapshot** · GCP **Disk snapshot / Cloud SQL backup** · Azure **Disk snapshot / backup** · OCI **Block volume backup**

Spot instance
:   Cloud-এর অব্যবহৃত capacity বড় ছাড়ে — কিন্তু cloud যেকোনো সময় ফেরত নিতে পারে। Batch/stateless কাজের জন্য ভালো।

    **কোন cloud-এ কী নামে:** AWS **Spot Instances** · GCP **Spot VMs** [old: Preemptible VMs] · Azure **Spot VMs** · OCI **Preemptible instances**

SSO (Single Sign-On)
:   একবার login করে অনেক app/cloud-এ ঢোকা।

    **কোন cloud-এ কী নামে:** AWS **IAM Identity Center** [old: AWS SSO] · GCP **Cloud Identity SSO / Workforce Identity Federation** · Azure **Entra ID SSO** · OCI **Identity Domains SSO**

State file (Terraform)
:   Terraform-এর "স্মৃতি" — কোন resource সে বানিয়েছে তার তালিকা। দলে কাজ করলে remote backend-এ রাখতে হয়, কখনো Git-এ নয়।

    **কোন cloud-এ কী নামে:** remote backend: AWS **S3** · GCP **GCS** · Azure **Blob Storage** · OCI **Object Storage** (S3-compatible)

Storage class / tier
:   Object কতটা ঘন ঘন লাগে তার ভিত্তিতে সস্তা-ধীর বা দামি-দ্রুত স্তর (hot / cool / archive)।

    **কোন cloud-এ কী নামে:** AWS **S3 Standard … Glacier** · GCP **Standard, Nearline, Coldline, Archive** · Azure **Hot, Cool, Cold, Archive** · OCI **Standard, Infrequent Access, Archive**

Subnet
:   একটা বড় private network-এর ছোট ভাগ (যেমন web, app, database আলাদা)।

    **কোন cloud-এ কী নামে:** AWS **Subnet (এক AZ)** · GCP **Subnet (regional)** · Azure **Subnet (regional)** · OCI **Subnet (regional বা AD-specific)**

Subscription
:   Azure-এ billing ও resource-এর মূল সীমানা। AWS **account**, GCP **project**-এর কাছাকাছি।

## T

Tag / Label
:   Resource-এ `key=value` চিহ্ন — খরচ ভাগ, automation ও খোঁজার জন্য।

    **কোন cloud-এ কী নামে:** AWS **Tags** · GCP **Labels** (এবং আলাদা **Tags** — policy-র জন্য) · Azure **Tags** · OCI **Freeform / Defined tags**

Tenancy
:   OCI-তে আপনার পুরো account/root — সব compartment এর উপরে।

    **কোন cloud-এ কী নামে:** AWS **Organization / account** · GCP **Organization** · Azure **Tenant** · OCI **Tenancy**

Tenant (Entra)
:   Azure-এ আপনার সংস্থার identity directory-র নমুনা (Microsoft Entra tenant) — user, group, app সব এর ভেতরে; একাধিক Subscription এই tenant-এর অধীনে থাকে।

Terraform
:   HashiCorp-এর IaC tool, `.tf` ফাইলে HCL ভাষায় লিখে `plan` / `apply` দিয়ে চার cloud-এই একই workflow-এ infrastructure বানানো। OpenTofu একটি open-source fork। বিস্তারিত: [Infrastructure as Code](../14-infrastructure-as-code.md)।

Throttling
:   API বা service অতিরিক্ত অনুরোধ ফিরিয়ে দেয় (প্রায়ই `429`/`ThrottlingException`)। Retry + exponential backoff দিয়ে সামলান।

TLS certificate
:   HTTPS-এর পরিচয়পত্র — ব্রাউজার ও সার্ভারের মাঝে encrypted যোগাযোগ।

    **কোন cloud-এ কী নামে:** AWS **ACM** · GCP **Certificate Manager** · Azure **Key Vault certificates / App Service managed certificates** · OCI **OCI Certificates**

Topic
:   Pub/Sub-এ message পাঠানোর "চ্যানেল"; গ্রাহকরা subscription দিয়ে শোনে।

Tracing (distributed)
:   একটা অনুরোধ অনেক service ঘুরে কোথায় কত সময় নিল তার পথ-রেকর্ড।

    **কোন cloud-এ কী নামে:** AWS **X-Ray** · GCP **Cloud Trace** · Azure **Application Insights** · OCI **APM**

## U

Uptime
:   সিস্টেম কত শতাংশ সময় চালু থাকে (৯৯.৯% মানে মাসে প্রায় ৪৩ মিনিট বন্ধ)।

User data / Startup script
:   VM প্রথমবার চালু হওয়ার সময় নিজে নিজে চলা script — software install/config।

    **কোন cloud-এ কী নামে:** AWS **User data** · GCP **Startup script** (metadata) · Azure **Custom data / cloud-init** · OCI **cloud-init (user data)**

## V

Vector database
:   Text/ছবির সংখ্যাগত "embedding" জমিয়ে অর্থ-ভিত্তিক খোঁজার database — AI search ও RAG-এ ব্যবহৃত।

    **কোন cloud-এ কী নামে:** AWS **OpenSearch / Aurora pgvector / S3 Vectors** · GCP **Vertex AI Vector Search / AlloyDB** · Azure **Azure AI Search** · OCI **Oracle AI Vector Search**

Versioning
:   Object/file বদলালে পুরোনো সংস্করণ জমিয়ে রাখা — ভুলে মুছলে ফিরে পাওয়া যায়।

Virtual machine (VM)
:   Software দিয়ে বানানো কম্পিউটার — নিজের OS, CPU, memory, disk নিয়ে।

    **কোন cloud-এ কী নামে:** AWS **EC2** · GCP **Compute Engine** · Azure **Virtual Machines** · OCI **Compute**

VPC / VNet / VCN
:   Cloud-এর ভেতরে আপনার একান্ত private network। GCP-তে VPC **global**, অন্য তিনটি **regional**।

    **কোন cloud-এ কী নামে:** AWS **VPC** · GCP **VPC network** · Azure **Virtual Network (VNet)** · OCI **Virtual Cloud Network (VCN)**

VPN
:   Public internet-এর ওপর encrypted "সুড়ঙ্গ" — অফিস বা ব্যবহারকারীকে cloud network-এর সাথে জোড়া।

    **কোন cloud-এ কী নামে:** AWS **Site-to-Site VPN / Client VPN** · GCP **Cloud VPN (HA VPN)** · Azure **VPN Gateway** · OCI **Site-to-Site VPN**

## W

WAF (Web Application Firewall)
:   Web app-কে SQL injection, XSS, bot-এর মতো আক্রমণ থেকে HTTP স্তরে বাঁচায়।

    **কোন cloud-এ কী নামে:** AWS **AWS WAF** · GCP **Cloud Armor** · Azure **Azure WAF** · OCI **OCI WAF**

Well-Architected Framework
:   Cloud কোম্পানির নিজস্ব best-practice গাইড (reliability, security, cost, performance, operations…)।

    **কোন cloud-এ কী নামে:** AWS **AWS Well-Architected** · GCP **Google Cloud Well-Architected Framework** [old: Architecture Framework] · Azure **Azure Well-Architected** · OCI **OCI Well-Architected Framework**

Workload identity federation
:   বাইরের workload (GitHub Actions, অন্য cloud, Kubernetes pod) আগে থেকে বিশ্বাস করা identity দিয়ে cloud-এ অস্থায়ী credential নেয় — স্থায়ী key ছাড়া।

    **কোন cloud-এ কী নামে:** AWS **IAM OIDC provider / IRSA / Pod Identity** · GCP **Workload Identity Federation** · Azure **Federated credentials / Entra Workload ID** · OCI **Identity propagation trust / OKE Workload Identity**

## Z

Zero trust
:   "নেটওয়ার্কের ভেতরে আছে বলেই বিশ্বাস নয়" — প্রতিটি অনুরোধের পরিচয় ও permission যাচাই। Private network + IAM + MFA একসাথে।

Zone
:   (দেখুন Availability Zone) GCP-তে "zone" মানে region-এর ভেতরের এক data center। *DNS zone* আলাদা জিনিস — একটা domain-এর record রাখার ঘর।

    **কোন cloud-এ কী নামে:** AWS **Availability Zone / hosted zone (DNS)** · GCP **Zone / managed zone (DNS)** · Azure **Availability Zone / DNS zone** · OCI **Availability Domain / DNS zone**
