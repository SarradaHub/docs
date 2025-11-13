# IaC Coverage Audit

| Resource | Current Provisioning | IaC Status | Notes |
| --- | --- | --- | --- |
| AWS MSK (Kafka) | Terraform modules under `infra/terraform/modules/kafka` | Partially covered | Need workspace per env + state backend configured. |
| AWS Glue Schema Registry | Terraform module `modules/schema_registry` | Partially covered | Extend module to register schemas from each repo automatically. |
| Observability Stack (AMP/Grafana/CloudWatch) | Terraform `modules/observability` | Partially covered | Pending IAM wiring for collectors + alerts. |
| Pickup Game Manager Infra (Kamal servers) | Kamal deploy yaml | Not codified in shared IaC | Capture host provisioning, load balancer, secrets via Terraform or Ansible. |
| SarradaBet Render Service | Provisioned through Render dashboard | Not in IaC | Use Render API/terraform provider to codify web service + env vars. |
| Vercel Frontends (`sarradabet`, `saturday-league-football`) | Managed manually | Not in IaC | Use Vercel Terraform provider to create projects, domains, env vars. |
| Supabase Project (`gvoidjhjmidljabjgydb`) | Manual through console | Not in IaC | Evaluate Supabase management via Terraform or scripts; at minimum export settings. |
| Shared Secrets | Stored per platform (Vercel, Render, Kamal) | Fragmented | Standardize on GitHub Actions secrets + env sync automation. |

## Action Items
1. Select Terraform Cloud/Backend for remote state (e.g., S3 + DynamoDB or Terraform Cloud workspaces).
2. Create Terraform modules/providers for Vercel, Render, Supabase; integrate into env stacks under `infra/terraform/envs/*`.
3. Document secrets mapping and automation script to push to Vercel/Render/Supabase from GitHub (e.g., using `dotenv-vault` or custom CLI).
