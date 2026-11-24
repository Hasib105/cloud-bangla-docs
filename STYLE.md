# Writing rules (every page)

## Language
- Bangla prose for explanations. English for technical terms the way Bangladeshi engineers say them
  ("subnet", "load balancer", "policy"). Don't force-translate terms.
- Code blocks, CLI commands, comments inside code, file names and resource names: **English only**.
- Friendly teacher tone, short paragraphs, "আপনি" form. Explain like the reader is a beginner, then go deep.

## Running example
- Only **`shopbd`**: a fictional small Bangladeshi online shop — web frontend, REST API, PostgreSQL,
  product images in object storage, order e-mails, nightly sales report.
- Names: `shopbd-prod-vpc`, `shopbd-images`, `shopbd-api`, `shopbd-db`, `ops@example.com`, `shopbd.example.com`.
- **Never** mention AUREV, Luna, travel, concierge, booking, or anything from `.ref/aws-original`'s example.
  Placeholder IDs: `123456789012`, `my-project-id`, `00000000-0000-0000-0000-000000000000`, `ocid1.compartment.oc1..example`.

## Chapter skeleton (common-topic chapters)
1. `# NN · <Bangla title>` then a `>` blockquote: এই অধ্যায়ে কী শিখবেন (3–6 bullets).
2. `!!! abstract "এক নজরে — কোন cloud-এ কী নাম"` with a table: Concept | AWS | GCP | Azure | OCI.
3. Concept sections, numbered with Bangla digits like `## ১. ...`. Each: what & why (cloud-agnostic),
   an everyday analogy, a mermaid diagram where it helps (```` ```mermaid ````).
4. For hands-on parts, **platform content tabs** — labels exactly `AWS`, `GCP`, `Azure`, `OCI`, always
   in this order (the site links tabs, so a reader who picks GCP sees GCP everywhere):

   ```markdown
   === "AWS"

       Service: **Amazon EC2** — short note.

       ```bash
       aws ec2 describe-instances --region ap-south-1
       ```

   === "GCP"

       ...
   ```
   (Tab content indented 4 spaces.) Console path in one line, CLI commands, important notes.
   Use region `ap-south-1` / `asia-south1` / `centralindia` / `ap-mumbai-1` (closest to Bangladesh)
   unless the topic needs otherwise.
5. Terraform snippets in tabs too, where the topic has resources (provider `aws`, `google`, `azurerm`, `oci`).
6. `## মিল ও অমিল` — a comparison table + the gotchas people actually hit
   (e.g. GCP VPC is global, Azure NSG attaches to subnet *or* NIC, OCI needs compartment OCIDs everywhere).
7. `## Real-world: shopbd` — how shopbd uses this topic, ideally on each cloud.
8. `## Best practices`, `## Common mistakes` (with fix), `## ইন্টারভিউ / সার্টিফিকেশন টিপস`
   (use `??? question "..."` collapsible Q&A), `## সারসংক্ষেপ`.
9. Length: 600–1000 lines. Depth over fluff — cover every concept listed for the chapter in PLAN.md.

## Accuracy
- Use current names (Nov 2026). Mention a renamed/deprecated old name once in brackets if readers meet it.
- No exact prices; explain cost drivers and link the official pricing page.
- Do not invent CLI flags or Terraform arguments. Short, commonly documented commands only.
- Official docs links are welcome (plain https links to aws.amazon.com, cloud.google.com, learn.microsoft.com, docs.oracle.com).

## Markdown / MkDocs
- Admonitions: `!!! tip`, `!!! warning`, `!!! danger`, `!!! example`, `!!! note`, `??? question`.
- Links to other pages: relative file links like `[Networking](05-networking.md)` (from `platforms/` use `../05-networking.md`).
  **No `#anchor` links to other pages** (Bangla slugs break). Same-page TOC is not needed — Material shows it.
- Tables must have a header row. Code blocks must have a language (`bash`, `hcl`, `json`, `yaml`, `python`, `powershell`, `text`).
- Do not touch `mkdocs.yml`, `index.md`, or other writers' files.
