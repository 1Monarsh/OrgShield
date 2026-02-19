\# AWS Config – High-Signal Conformance Layer



OrgShield implements a scoped AWS Config validation layer focused on high-impact governance controls.



Rather than enabling all managed rules (which increases noise and cost), only high-signal security validations were selected.



\## Objectives



\- Continuous validation of audit logging

\- Root account protection enforcement

\- Encryption posture verification

\- Public exposure prevention



\## Implemented Managed Rules



\- cloud-trail-enabled

\- root-account-mfa-enabled

\- s3-bucket-public-read-prohibited

\- s3-bucket-public-write-prohibited

\- encrypted-volumes

\- rds-storage-encrypted



\## Design Philosophy



\- Prevent excessive compliance noise

\- Focus on foundational security posture

\- Maintain low operational cost

\- Align with AWS Well-Architected Security Pillar



