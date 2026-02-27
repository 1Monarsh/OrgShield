# OrgShield

## Overview
OrgShield is a production-grade AWS multi-account security baseline that I designed and implemented to secure the control plane before deploying workloads.
The goal was simple: establish governance, logging, detection, and validation first — then think about infrastructure.

## Architecture

- Multi-account AWS Organization (Management, Security, Production)
- Centralized access via AWS IAM Identity Center (SSO)
- Organization-wide CloudTrail (multi-region, encrypted, log validation enabled)
- GuardDuty configured using the delegated administrator model
- Dedicated Security account for monitoring and log isolation
- Minimal but high-impact SCP enforcing:
  - organizations:LeaveOrganization
  - cloudtrail:StopLogging
  
### Continuous Compliance Validation (AWS Config)

To prevent silent drift, I enabled AWS Config with a focused set of high-signal managed rules.
Instead of enabling every available rule (which adds noise and cost), I selected only foundational controls that directly impact governance integrity.

Selected managed rules include:

- cloud-trail-enabled
- root-account-mfa-enabled
- s3-bucket-public-read-prohibited
- s3-bucket-public-write-prohibited
- encrypted-volumes
- rds-storage-encrypted

This ensures:

- Audit logging cannot be disabled unnoticed
- Root account protections remain enforced
- Encryption posture stays compliant
- Public exposure risks are continuously flagged


## Security Design Principles

- Blast radius isolation
- Centralize observability and logging
- Prevent → Detect → Validate model
- Use minimal but meaningful guardrails
- Keep governance cost-efficient

## Security Validation Scenarios

To verify that the controls were actually effective, I tested the following scenarios:

### 1. Attempted Organization Exit
Test: Attempted to leave the AWS Organization from a member account.
Result: Blocked by SCP (organizations:LeaveOrganization denied).

### 2. CloudTrail Tampering Attempt
Test: Attempted to stop CloudTrail logging.
Result: Denied by SCP (cloudtrail:StopLogging).

### 3. Public S3 Exposure Test
Test: Created an S3 bucket with public access.
Result: Flagged by AWS Config managed rules.

### 4. Threat Detection Simulation
Test: Triggered GuardDuty finding via suspicious activity simulation.
Result: Finding centralized in Security account via delegated administrator model.

## Architecture Overview

OrgShield follows a centralized control-plane security model across three accounts:

- Management Account → Governance boundary & SCP enforcement
- Security Account → Centralized logging, GuardDuty delegated admin, AWS Config validation
- Production Account → Workload isolation governed by organization policies

CloudTrail logs flow into the Security account.
GuardDuty findings aggregate centrally.
SCP guardrails protect against control-plane tampering.

## Estimated Monthly Cost
~€3–6/month (low-traffic configuration)

## Author
Monarsh

