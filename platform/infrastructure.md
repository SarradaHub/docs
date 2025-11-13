# Platform Infrastructure Blueprint

## Event Backbone
- **Service**: Amazon MSK (Kafka 3.7) with TLS + IAM authentication.
- **Topology**: Three private subnets per environment, auto-scaled instance size per env (`m7g.large` in dev, `m7g.xlarge` in staging/prod).
- **Security**: Dedicated security group (`platform-events-<env>-sg`) plus shared SG for client workloads. IAM policies enforced via SASL/IAM.
- **Monitoring**: Enhanced monitoring at `PER_TOPIC_PER_PARTITION`; broker logs shipped to CloudWatch (`/platform/msk`).

## Schema Registry
- **Service**: AWS Glue Schema Registry seeded with canonical contracts from `platform/contracts`.
- **Compatibility Mode**: Backward by default; override per schema when needed.
- **Integration**: Event gateway validates against registry before publishing; CI pipeline enforces compatibility checks via Terraform apply plan.

## Observability Stack
- **Metrics**: Amazon Managed Prometheus workspace (`platform-events-<env>`). OTel collectors push metrics via IAM role `platform-otel-collector`.
- **Dashboards**: Amazon Managed Grafana workspace per env for unified dashboards (Kafka, gateway, producer/consumer lag).
- **Logs**: CloudWatch log groups for MSK, event gateway, schema services with 30-day retention (configurable via Terraform var).
- **Tracing**: OTel collector role ready for X-Ray or Jaeger exporters (to be configured in app deployments).

## Networking
- **VPC**: `/16` CIDR per env with three AZs. Public subnets host NAT gateways; private subnets host stateful services (Kafka brokers, gateway).
- **Routing**: Public subnets route to internet via IGW; private subnets use per-AZ NAT for outbound updates.
- **Security Groups**: `platform-shared` SG reused across platform workloads; service-specific SGs attach on top.

## Secret Management & Access (Next Steps)
- Use AWS Secrets Manager for Kafka credentials and API keys.
- Integrate IAM roles for service accounts (IRSA) once workloads move to EKS or ECS.
- Bootstrap Vault namespace if advanced secret rotation required.

## Deployment Workflow
1. Select environment directory (e.g., `infra/terraform/envs/dev`).
2. Run `terraform init && terraform plan` (after exporting `AWS_PROFILE`).
3. Review plan; apply to provision network, Kafka, schema registry, and observability resources.
4. Register service IAM roles to access Kafka using SASL/IAM.
5. Import topic definitions via event gateway bootstrap scripts (see `platform/event-gateway`).

## Cost Considerations
- Dev keeps 3-node Kafka with smaller instances; staging uses same node count but larger brokers; prod scales to 6 nodes for HA.
- NAT gateways per AZ incur cost; adjust by consolidating if budgets require.
- Grafana workspace billed hourly; disable when not needed by toggling module variable in Terraform.
