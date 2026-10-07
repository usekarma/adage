# AWS Bootstrap Checklist

This checklist walks through the minimum steps required to set up a new AWS account securely and prepare it for deployment using the configuration-driven model.

These steps are typically performed once per management account.

---

## Account Setup

- [ ] Create a new AWS account (or use an existing one)
- [ ] Enable MFA for the root user
- [ ] Configure billing alerts

See: [Cost Management](./cost-management/README.md)

---

## Identity Center

- [ ] Enable IAM Identity Center
- [ ] Create an admin user in IAM Identity Center
- [ ] Create a permission set (e.g., `AdministratorAccess`)
- [ ] Assign the user to the current account
- [ ] Log in via the access portal to verify access

See: [Identity Center Setup](./identity-center/README.md)

---

## Enable AWS Organizations

- [ ] Enable Organizations from the management account
- [ ] Create organizational units (e.g., `dev`, `prod`)
- [ ] Create member accounts under those OUs
- [ ] In each account, create the `/iac/environment` parameter in Parameter Store

See: [Organization Structure](./org-structure/README.md)

---

## Security Baseline

- [ ] Enable CloudTrail (organization-wide)
- [ ] Enable AWS Config
- [ ] Enable GuardDuty
- [ ] (Optional) Set up Security Hub or SCPs

See: [Security Baseline](./security-baseline/README.md)

---

## Tagging & Cost Visibility

- [ ] Define standard tag keys (e.g., `Project`, `Environment`, `Owner`)
- [ ] Activate those tags in the Billing Console
- [ ] Set a budget with email alerts
- [ ] Apply tags consistently in all Terraform modules

See: [Tagging Policy](./tagging-policy/README.md) and [Cost Management](./cost-management/README.md)

---

## After This

Once this checklist is complete, your account is ready to deploy infrastructure using the configuration-driven model.

👉 [Next: Deploy a Serverless Static Website](./quickstarts/serverless-site.md)

---

View all setup guides at [Adage: AWS Deployment Framework](./README.md)

