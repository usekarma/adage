# Getting Started

**New here?** Start with _**this Getting Started guide**_ first, then continue with the [Serverless Site Quickstart](./quickstarts/serverless-site.md).

Welcome to the Adage deployment framework. This guide walks you through the initial steps needed to prepare your AWS environment for configuration-driven deployments.

---

## Prerequisites

Before proceeding, make sure you have:

- An AWS account with admin access  
- [AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)  
- [Terraform](https://developer.hashicorp.com/terraform/downloads) (**v1.3+ recommended**)  
- [Terragrunt](https://terragrunt.gruntwork.io/docs/getting-started/install/) (**v0.45.x recommended**)  
- [`jq`](https://stedolan.github.io/jq/) for working with JSON on the command line  
- Python 3.9+ for running helper scripts

To verify your installations:

```sh
terraform version
terragrunt --version
```

We recommend using **Terraform v1.3+** and **Terragrunt v0.45.x** for compatibility with this framework.

---

## AWS SSO and CLI Profile Setup

You’ll need to configure named profiles in your AWS CLI for authentication.

Follow the guide here:

[AWS CLI Profile Setup →](./setup/aws-cli-profiles.md)

Once set up, you can log in:

```sh
aws sso login --profile management
```

> **Note:** If you're starting from a brand-new AWS account and haven't yet enabled IAM Identity Center, you’ll need to do that in the AWS Console first.  
> See [Account Bootstrapping →](./bootstrap-checklist.md) for instructions on enabling SSO and creating your first admin user.

For a deeper explanation of how and why Adage uses this login pattern, see:

[Understanding the AWS Login Strategy →](./aws-login-strategy.md)

---

## First-Time Account Bootstrapping

Next, set up your AWS Organization, create required accounts, and enable IAM Identity Center.

Follow the steps in:

[Account Bootstrapping →](./bootstrap-checklist.md)

---

## When Things Go Wrong

If something doesn’t work as expected, check the following troubleshooting guide:

[Troubleshooting Common Issues →](./troubleshooting_common_issues.md)

---

## Next Steps

Once these steps are complete, you should validate your setup:

[Validate Your Setup →](./validate_setup.md)

Then you can deploy your first component:

[Serverless Static Site Quickstart →](./quickstarts/serverless-site.md)


