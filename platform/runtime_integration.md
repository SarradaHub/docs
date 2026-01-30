# Runtime Integration Plan

## Event Gateway Deployment
- **Container Image**: Build from `platform/event-gateway/Dockerfile`; publish to ECR (`platform-event-gateway` repo).
- **Host Platform**: ECS Fargate (preferred) or Render container service. Requires outbound access to MSK (via VPC peering or PrivateLink) and AWS Glue.
- **Environment Variables**: See `docs/platform/env_matrix.md` (gateway section). Leverage IAM roles for MSK/Glue rather than static credentials when running in ECS.
- **Networking**: Place gateway in private subnets with AWS ALB exposing HTTPS. Use AWS ACM cert; enforce mTLS for internal producers (optional but recommended).
- **Scaling**: Configure target tracking on ALB requests per target (e.g., 200 RPS). Enable autoscaling between 2-6 tasks.

## Service Wiring
| Service | Action Items |
| --- | --- |
| Pickup Game Manager (Rails) | Set `EVENT_GATEWAY_URL` to internal load balancer URL. Store API key in Kamal secrets. Ensure background job queue can reach gateway. |
| Saturday League API (Rails) | Same as above; confirm migrations run to populate `platform_uid`. |
| SarradaBet API (Render) | Configure Render env vars for gateway URL/API key. Expose Kafka brokers via AWS MSK public endpoints or set up Render private service using Wireguard/VPC connector. Consumer `MatchEventConsumer` requires outbound connectivity on port 9092. |
| Supabase | No runtime changes; ensure service role created via Terraform injected into Render service for DB access. |
| Vercel Frontends | Set `VITE_EVENT_STREAM_URL` / `NEXT_PUBLIC_EVENT_STREAM_URL` to gateway SSE endpoint (via public HTTPS). |

## Deployment Workflow
1. Build & push gateway image via GitHub Actions (`platform/event-gateway` workflow dispatch).
2. Terraform apply for environment (`infra/terraform/envs/{env}`) to create ECS service, ALB, security groups.
3. Update service secrets (Render, Kamal, Vercel) with gateway URL/API key using secrets sync job.
4. Redeploy applications:
   - Kamal deploy for Rails apps.
   - Render deploy for SarradaBet API (ensure `npm run prisma:migrate:deploy`).
   - Vercel promote latest build to production.
5. Verify event flow using smoke script: create match → check bet market → front-end ticker update.

## Operational Considerations
- Implement DLQ topics in Kafka (e.g., `*.deadletter`) and configure gateway to route errors.
- Monitor consumer lag and gateway 5xx rates; add automatic pause/resume if schema validation failure count exceeds threshold.
- Document rollback for gateway (previous image tag) and service toggles (feature flags to disable event publish temporarily).
