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

## Security Design Principles

- Blast radius isolation
- Centralized observability
- Prevent → Detect → Validate model
- Minimal high-impact guardrails
- Cost-aware implementation

## Diagram
![Architecture Diagram](architecture.png)

## Estimated Monthly Cost
~€3–6/month (low-traffic configuration)

## Author
Monarsh

