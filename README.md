# Ford infrastructure

Terraform source of truth for infrastructure managed by Ford projects. OCI is
the first provider. Additional cloud and SaaS providers should be isolated in
their own root modules, with separate credentials and state.

## OCI

The initial root module in [`terraform/oci`](terraform/oci) contains the
existing OCI compute, data-volume, and backup-bucket configuration. Resource
creation/management flags default to `false`, and protected storage resources
use `prevent_destroy`.

This public repository currently runs formatting and configuration validation
only. Do not run `apply` from CI. Before adopting it for existing resources,
configure a protected remote state backend, migrate/import state in a reviewed
change, and verify the plan is empty or contains only explicitly approved
changes. Never commit state, plans, credentials, or private keys.

Locally, use the OCI CLI config profile selected by `oci_config_profile` and a
private `terraform.tfvars` file based on `terraform.tfvars.example`. Confirm
the plan and target tenancy before any manual apply.

Terraform supports multiple clouds because each provider is a separately
versioned plugin. A root module must declare the providers it uses; keeping
OCI, Cloudflare, and Grafana configurations in separate roots also keeps
credentials and state isolated.
