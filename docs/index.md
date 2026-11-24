---
hide:
  - navigation
  - toc
---

<div class="cb-hero" markdown>

# এক টপিক, চার Cloud ☁️

**AWS · Google Cloud · Microsoft Azure · Oracle Cloud (OCI)** — শূন্য থেকে অ্যাডভান্স, সম্পূর্ণ বাংলায়।

প্রতিটি অধ্যায় আগে **ধারণা** বোঝায়, তারপর দেখায় একই কাজ কোন cloud-এ কোন service দিয়ে হয় — পাশাপাশি।

</div>

## এই ডকস কীভাবে আলাদা?

বেশিরভাগ cloud টিউটোরিয়াল একটা প্ল্যাটফর্ম ধরে শেখায়। কিন্তু চাকরিতে গিয়ে দেখবেন কোম্পানি হয়তো Azure চালায়,
ক্লায়েন্ট চায় GCP, আর আপনি শিখেছেন শুধু AWS। আসল কথা হলো — **ধারণাগুলো একই**। Virtual machine, network,
IAM, object storage — সব cloud-এ আছে, শুধু নাম আর খুঁটিনাটি আলাদা।

তাই এখানে আমরা শিখি **টপিক ধরে**:

- ধারণাটা কী, কেন দরকার — cloud ছাড়াই, সহজ উদাহরণে
- **AWS / GCP / Azure / OCI** — ট্যাবে পাশাপাশি service, Console, CLI ও Terraform
- **মিল ও অমিল** — কোথায় মানুষ সবচেয়ে বেশি ভুল করে
- একটি চলমান উদাহরণ — **`shopbd`**, একটি ছোট বাংলাদেশি অনলাইন শপ — চার cloud-এই

!!! tip "আপনার cloud বেছে নিন একবার"
    যেকোনো পাতায় `AWS`, `GCP`, `Azure` বা `OCI` ট্যাবে ক্লিক করলে **পুরো সাইটে** সেই ট্যাবই খোলা থাকবে।
    শুধু একটা cloud শিখতে চাইলে সেটাই বেছে নিন; তুলনা দেখতে চাইলে ট্যাব বদলান।

## কোথা থেকে শুরু করবেন

<div class="grid cards" markdown>

-   :material-school-outline: **একদম নতুন?**

    ---

    [01 · Cloud-এর ভিত্তি](01-cloud-fundamentals.md) → [02 · Account ও Hierarchy](02-accounts-and-hierarchy.md) →
    [03 · IAM](03-identity-and-access.md) → [04 · Virtual Machine](04-compute-vms.md)

-   :material-lan: **App চালাতে চান?**

    ---

    [05 · Networking](05-networking.md), [06 · Storage](06-storage.md), [07 · Database](07-databases.md),
    [10 · Load Balancing, DNS ও CDN](10-load-balancing-scaling-dns-cdn.md)

-   :material-kubernetes: **Modern app ও DevOps**

    ---

    [08 · Container ও Kubernetes](08-containers-kubernetes.md), [09 · Serverless](09-serverless.md),
    [14 · Terraform](14-infrastructure-as-code.md), [15 · CI/CD](15-cicd-devops.md)

-   :material-shield-check-outline: **Production-ready**

    ---

    [12 · Monitoring](12-observability.md), [13 · Security](13-security.md),
    [18 · Cost](18-cost-management.md), [19 · Architecture ও HA/DR](19-architecture-ha-dr.md)

</div>

## প্ল্যাটফর্ম ধরে পড়তে চান?

<div class="grid cards" markdown>

-   <span class="cb aws">AWS</span> **Amazon Web Services** — সবচেয়ে বড় service catalog

    [AWS গাইড →](platforms/aws.md)

-   <span class="cb gcp">GCP</span> **Google Cloud** — data, Kubernetes ও global network

    [GCP গাইড →](platforms/gcp.md)

-   <span class="cb azure">Azure</span> **Microsoft Azure** — enterprise ও Microsoft ecosystem

    [Azure গাইড →](platforms/azure.md)

-   <span class="cb oci">OCI</span> **Oracle Cloud** — Oracle DB, কম egress খরচ, শক্তিশালী free tier

    [OCI গাইড →](platforms/oci.md)

</div>

## দ্রুত তুলনা

| কাজ | AWS | GCP | Azure | OCI |
|---|---|---|---|---|
| Virtual machine | EC2 | Compute Engine | Virtual Machines | Compute |
| Private network | VPC | VPC (global) | Virtual Network (VNet) | Virtual Cloud Network (VCN) |
| Object storage | S3 | Cloud Storage | Blob Storage | Object Storage |
| Managed PostgreSQL | RDS / Aurora | Cloud SQL / AlloyDB | Azure Database for PostgreSQL | OCI Database with PostgreSQL |
| Kubernetes | EKS | GKE | AKS | OKE |
| Functions | Lambda | Cloud Run functions | Azure Functions | OCI Functions |
| Identity | IAM / IAM Identity Center | Cloud IAM | Microsoft Entra ID + Azure RBAC | OCI IAM (Identity Domains) |
| Monitoring | CloudWatch | Cloud Monitoring / Logging | Azure Monitor | OCI Monitoring / Logging |
| Infrastructure as Code | CloudFormation / CDK | Infrastructure Manager | ARM / Bicep | Resource Manager |

পুরো তালিকা: [Service Map](reference/service-map.md) · কমান্ড পাশাপাশি: [CLI Cheatsheet](reference/cli-cheatsheet.md) ·
শব্দের মানে: [Glossary](reference/glossary.md)

## লোকালি চালান

```bash
pip install -r requirements.txt
mkdocs serve   # http://127.0.0.1:8000
```
