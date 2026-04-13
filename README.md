# OrgShield – AWS Multi-Account Security Baseline

## Overview

OrgShield is a production-grade AWS multi-account security baseline designed to secure the control plane **before deploying workloads**.

The goal is simple:
Establish governance, logging, detection, and validation first—then build infrastructure on top of a secure foundation.

---

## Architecture Overview

OrgShield follows a centralized control-plane security model across three AWS accounts:

* **Management Account** → Governance boundary and SCP enforcement
* **Security Account** → Centralized logging, GuardDuty delegated admin, AWS Config validation
* **Production Account** → Workload environment governed by organization policies

### Flow

* All account activity is logged via CloudTrail → stored in Security account
* GuardDuty findings aggregate centrally via delegated administrator model
* AWS Config continuously evaluates compliance across accounts
* SCPs enforce preventive controls at the organization level

This design ensures governance, visibility, and threat detection remain **centralized and tamper-resistant**.

---

## Core Components

* Multi-account AWS Organization (Management, Security, Production)
* Centralized access via AWS IAM Identity Center (SSO)
* Organization-wide CloudTrail (multi-region, encrypted, log validation enabled)
* GuardDuty with delegated administrator model
* AWS Config for continuous compliance validation
* Dedicated Security account for logging and monitoring

---

## SCP Guardrails

Minimal but high-impact Service Control Policies prevent critical control-plane risks:

* `organizations:LeaveOrganization`
* `cloudtrail:StopLogging`

These controls ensure that governance boundaries and audit logging cannot be bypassed.

---

## Threat Model

OrgShield is designed to protect against key control-plane risks:

* Unauthorized account exit
* Logging tampering or disablement
* Undetected misconfigurations
* Delayed or fragmented threat visibility

---

## Continuous Compliance Validation

To prevent silent drift, AWS Config is configured with high-signal managed rules:

* `cloud-trail-enabled`
* `root-account-mfa-enabled`
* `s3-bucket-public-read-prohibited`
* `s3-bucket-public-write-prohibited`
* `encrypted-volumes`
* `rds-storage-encrypted`

This ensures:

* Audit logging remains enforced
* Root account is secured
* Encryption standards are maintained
* Public exposure risks are continuously detected

---

## Security Design Principles

* Blast radius isolation
* Centralized observability and logging
* Prevent → Detect → Validate model
* Minimal but meaningful guardrails
* Cost-efficient governance

---

## Validation Scenarios (Proof of Effectiveness)

To verify real-world behavior, the following scenarios were tested:

**1. Organization Exit Attempt**

* Attempt: Leave AWS Organization
* Result: Blocked by SCP

**2. CloudTrail Tampering Attempt**

* Attempt: Stop CloudTrail logging
* Result: Denied by SCP

**3. Public S3 Exposure**

* Attempt: Create public S3 bucket
* Result: Flagged by AWS Config

**4. Threat Detection Simulation**

* Attempt: Simulated suspicious activity
* Result: GuardDuty finding centralized in the Security account

These tests confirm that preventive and detective controls are actively enforced.

---

## Why This Matters

Most architectures secure workloads after deployment.

OrgShield secures the **control plane first**, ensuring governance, logging, and detection are enforced before any infrastructure is deployed.

This approach reduces long-term risk and prevents security gaps that are difficult to fix later.

---

## Estimated Cost

~€3–6/month (low-traffic configuration)

---

## Author

Monarsh


