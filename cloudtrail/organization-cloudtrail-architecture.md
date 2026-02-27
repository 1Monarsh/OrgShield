\# Organization-Wide CloudTrail Architecture



\## Objective



Establish immutable, centralized audit logging across all AWS accounts to ensure control-plane visibility and forensic integrity.



\## Design Decisions



\### 1. Organization Trail (Not Account-Level Trail)



An organization-wide CloudTrail was created from the Management account to automatically include:



\- All existing accounts

\- All future accounts

\- All enabled regions



This eliminates configuration drift and ensures consistent logging coverage.



---



\### 2. Multi-Region Logging Enabled



CloudTrail was configured to log events across all AWS regions.



This prevents blind spots caused by region-specific activity and supports incident response scenarios.



---



\### 3. Log File Integrity Validation Enabled



Log file validation was enabled to:



\- Detect log tampering

\- Maintain forensic credibility

\- Preserve audit chain-of-custody



---



\### 4. Centralized Log Archive (Security Account)



CloudTrail logs are delivered to a dedicated S3 bucket in the Security account.



This ensures:



\- Log isolation from workload accounts

\- Reduced blast radius

\- Clear separation of duties



---



\### 5. KMS Encryption



SSE-KMS encryption was enabled for log files using a dedicated KMS key.



Benefits:



\- Controlled key access

\- Strong encryption posture

\- Compliance alignment



---



\## Governance Integration



The following SCP enforces audit protection:



\- cloudtrail:StopLogging → Denied



This prevents log tampering at the organization level.



---



\## Security Outcome



OrgShield ensures:



\- Centralized audit visibility

\- Tamper-resistant logging

\- Region-wide coverage

\- Automatic inclusion of new accounts

\- Alignment with AWS Well-Architected Security Pillar

