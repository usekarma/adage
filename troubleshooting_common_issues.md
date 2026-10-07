# Troubleshooting Common Issues

## Table of Contents

- [AWS SSO or Profile Not Set Up](#aws-sso-or-profile-not-set-up)
- [Missing Tools or Dependencies](#missing-tools-or-dependencies)
- [Parameter Store Path Not Found](#parameter-store-path-not-found)
- ["site_name" Misunderstood](#site_name-misunderstood)
- [CloudFront Output Missing or Wrong](#cloudfront-output-missing-or-wrong)
- [Route 53 Not Working](#route-53-not-working)
- [Conflicting DNS Records (CNAME from ImprovMX)](#conflicting-dns-records-cname-from-improvmx)
- [Email Forwarding Problems](#email-forwarding-problems)
- [Destroy Command Didn’t Remove Everything](#destroy-command-didnt-remove-everything)


This guide outlines common problems you might encounter when deploying a serverless static site using the Adage framework, and how to fix them.

---

## AWS SSO or Profile Not Set Up

**Symptom:** Commands fail silently or show access denied errors.

**Fix:**
1. Make sure you've configured your AWS CLI named profiles:
   [AWS CLI Profile Setup](./setup/aws-cli-profiles.md)
2. Authenticate before using any `AWS_PROFILE`:

```sh
aws sso login --profile management
```

Then use your role-based profile (e.g. `dev-iac`).

---

## Missing Tools or Dependencies

**Symptom:** Commands like `jq`, `terraform`, or Python scripts fail.

**Fix:** Install the required tools:

- [AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [jq](https://stedolan.github.io/jq/)
- Python 3.9+

---

## Parameter Store Path Not Found

**Symptom:** Terraform errors out with "parameter not found" or similar.

**Fix:**
- Ensure you've run the config deployment step **first**:

```sh
cd aws-config/
AWS_PROFILE=dev-iac ./scripts/deploy.sh serverless-site <nickname>
```

- Double-check that your nickname matches your folder path:
  `aws-config/iac/dev/serverless-site/<nickname>/config.json`

---

## "site_name" Misunderstood

**Symptom:** Broken custom domain routing or failed CloudFront aliasing.

**Fix:**
- `"site_name"` must be the **actual domain name**, like `www.example.com`, not a label.
- If you don't own the domain, set `"enable_custom_domain": false`.

---

## CloudFront Output Missing or Wrong

**Symptom:** You publish but don’t get a `custom_domain` or CloudFront URL.

**Fix:**
- Run `aws ssm get-parameter` to inspect the runtime output:

```sh
aws ssm get-parameter \
  --name "/iac/serverless-site/<nickname>/runtime" \
  --with-decryption \
  --query 'Parameter.Value' \
  --output text | jq .
```

- Look for:
  - `cloudfront_distribution_domain`
  - `custom_domain`

---

## Route 53 Not Working

**Symptom:** Domain not resolving, or HTTPS certificate validation fails.

**Fix:**
- Confirm your domain is delegated to AWS Route 53.
- Use the `route53-zone` component to manage DNS.
- See: [Route53-Zone Component](https://github.com/usekarma/aws-iac/tree/main/components/route53-zone)
  - Includes GoDaddy delegation example.

---

## Conflicting DNS Records (CNAME from ImprovMX)

**Symptom:** Route 53 fails to create a DNS record (e.g. `A` or `MX`) due to a conflicting `CNAME`.

**Fix:**
- If you signed up for [ImprovMX](https://improvmx.com/) and selected a default domain redirect (e.g. from `@` or `www`), it may have automatically added a `CNAME` record in Route 53.
- Visit **Route 53 → Hosted Zones → your domain** and manually delete any CNAME records you didn’t create (especially for `@` or `www`).
- Then re-run the `route53-zone` deployment:

```sh
cd aws-iac/
AWS_PROFILE=dev-iac ./scripts/deploy.sh route53-zone <nickname>
```

**Tip:** The Adage `route53-zone` config defines only the records you explicitly include — it won’t clean up third-party records.

---

## Email Forwarding Problems

**Symptom:** MX record causes validation errors or doesn't work.

**Fix:**
- If not using email, remove the `MX` record from your `route53-zone` config.
- If using ImprovMX, create an account at [improvmx.com](https://improvmx.com/) before applying the zone.

---

## Destroy Command Didn’t Remove Everything

**Symptom:** You used `--destroy` but some resources or parameters remain.

**Fix:**
- `--destroy` only removes infrastructure.
- Config and runtime parameters in Parameter Store remain unless removed manually.
- You can safely leave them for future redeploys.

---

Still stuck? [Open an issue](https://github.com/usekarma/adage/issues) or review the full [Quickstart Guide](./quickstarts/serverless-site.md).


