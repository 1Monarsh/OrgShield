# OrgShield

## Overview
Designed and implemented a production-grade AWS multi-account security architecture using AWS Organizations, centralized identity, logging, threat detection, and foundational guardrails.

## Architecture

- Multi-account AWS Organization (Management, Security, Production)
- Centralized access via AWS IAM Identity Center (SSO)
- Organization-wide CloudTrail (multi-region, encrypted, log validation enabled)
- GuardDuty delegated administrator model
- Dedicated Security account for monitoring
- Minimal SCP preventing:
  - organizations:LeaveOrganization
  - cloudtrail:StopLogging
  
### Continuous Compliance Validation (AWS Config)

To enforce continuous governance validation, AWS Config was enabled with a scoped set of high-impact managed rules.

Rather than enabling all available rules (which increases operational noise and cost), OrgShield focuses on foundational security posture validation.

Selected managed rules include:

- cloud-trail-enabled
- root-account-mfa-enabled
- s3-bucket-public-read-prohibited
- s3-bucket-public-write-prohibited
- encrypted-volumes
- rds-storage-encrypted

This ensures:

- Audit logging cannot drift from compliant state
- Root account protections remain enforced
- Storage encryption posture is continuously validated
- Public exposure risks are detected automatically


## Security Design Principles

- Blast radius isolation
- Centralized observability
- Prevent → Detect → Validate model
- Minimal high-impact guardrails
- Cost-aware implementation

## Security Validation Scenarios

To validate the effectiveness of OrgShield controls, the following scenarios were tested:

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

## Diagram
![Architecture Diagram](architecture.png)

## Estimated Monthly Cost
~€3–6/month (low-traffic configuration)

## Author
Monarsh

