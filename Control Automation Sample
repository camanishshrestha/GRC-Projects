```markdown
# GRC Control Automation Design Document

## Document Information
| Field | Value |
|-------|-------|
| Control Name | User Access Review & Privileged Access Management |
| Framework Reference | ISO 27001:2022 (A.8.2, A.8.5), NRB Unified Directives (IT Guidelines Chapter 9), SOC 2 (CC6.1, CC6.2, CC6.3) |
| Author | Rajesh Shrestha, Head of Information Security |
| Version | 1.0 |
| Date | 2025-07-13 |

---

## 1. Control Overview

### 1.1 Control Statement
**ISO 27001 A.8.2 – Privileged Access Rights:** "The allocation and use of privileged access rights shall be restricted and managed."

**ISO 27001 A.8.5 – Secure Authentication:** "Secure authentication technologies and procedures shall be established and implemented based on information access restrictions and the topic-specific policy on access control."

**NRB IT Guidelines Chapter 9.4:** "Banks and Financial Institutions (BFIs) and Payment Service Providers (PSPs) shall conduct periodic user access reviews at least quarterly. Privileged accounts shall be reviewed monthly. Access rights shall follow the principle of least privilege and segregation of duties."

**SOC 2 CC6.1:** "The entity implements logical access security software, infrastructure, and architectures over protected information assets to protect them from security events to meet the entity's objectives."

### 1.2 Control Intent
This control ensures that only authorized personnel have appropriate access to critical systems of Himalayan Digital Payment Solutions Pvt. Ltd. (HDPS), that privileged access is tightly managed, and that access rights are periodically reviewed and revoked when no longer needed.

| Objective | Description |
|-----------|-------------|
| Primary | Ensure only authorized users maintain appropriate access to payment processing systems, core banking middleware, and customer data repositories based on their current job role and responsibilities. |
| Secondary | Detect and remediate orphaned accounts, excessive privileges, segregation-of-duty (SoD) violations, and dormant accounts across all critical systems within defined SLAs. |
| Compliance | Satisfy NRB Unified Directives for PSPs operating in Nepal, maintain ISO 27001:2022 certification (certified by Quality Certification Nepal), and support SOC 2 Type II audit readiness for international partner onboarding. |

### 1.3 Current Implementation
HDPS currently performs user access reviews using a largely manual spreadsheet-based process. The IT Administration team exports user lists from each system individually (Active Directory, core payment application "HimalPay Engine", database servers, AWS console, and network devices). These exports are compiled into Excel workbooks and circulated via email to 12 department managers for review and sign-off. Managers mark users as "retain", "modify", or "revoke" and return the spreadsheets. The IT team then manually processes changes. Privileged accounts are tracked in a separate register maintained by the Senior System Administrator. Evidence is stored as signed PDF printouts in a shared Google Drive folder organized by quarter. The entire cycle takes 3–5 weeks per quarter and frequently misses the NRB-mandated completion deadline.

---

## 2. Current State Analysis

### 2.1 Manual Process Steps

| Step | Action | Owner | Frequency | Time Required |
|------|--------|-------|-----------|---------------|
| 1 | Export user account lists from Active Directory, HimalPay Engine, AWS IAM, PostgreSQL databases, FortiGate firewall, and Cisco switches | IT Administration Team (Suman Tamang) | Quarterly | 2 days |
| 2 | Compile exports into a consolidated Excel workbook with separate tabs per system, cross-reference with HR employee roster from PeopleHR Nepal | IT Administration Team | Quarterly | 3 days |
| 3 | Distribute review workbooks via email to 12 department managers with instructions and a 10-business-day deadline for completion | IT Administration Team | Quarterly | 0.5 days |
| 4 | Department managers review assigned user lists, mark access as retain/modify/revoke, and return signed workbooks via email | Department Managers | Quarterly | 5–15 business days (frequently delayed) |
| 5 | Follow up with managers who have not responded; escalate overdue reviews to CISO (Anita Gurung) after 10 days | IT Administration Team | Quarterly | 3–7 days |
| 6 | Consolidate all manager responses, validate change requests for conflicts or errors, and create change tickets in Jira Service Management | IT Administration Team | Quarterly | 2 days |
| 7 | Execute approved access changes (revocations, modifications) across individual systems manually | System Administrators | Quarterly | 3–5 days |
| 8 | Generate evidence package: compile completed review sheets, change tickets, screenshots of executed changes, and store in Google Drive | IT Administration Team / GRC Analyst (Priya Maharjan) | Quarterly | 2 days |
| 9 | Perform privileged account review: Senior SysAdmin reviews all admin/root/service accounts against privileged access register | Senior System Administrator (Bikash Adhikari) | Monthly | 1 day |
| 10 | GRC Analyst performs completeness check and prepares summary report for CISO sign-off and NRB quarterly submission | GRC Analyst (Priya Maharjan) | Quarterly | 1 day |

### 2.2 Failure Point Analysis

| Step | Failure Mode | Likelihood | Impact | Detection |
|------|--------------|------------|--------|-----------|
| 1 | Incomplete system inventory — new systems or applications not included in the export scope (e.g., a recently deployed microservice with its own auth) | High | High — unreviewed systems may harbor unauthorized access | Late — discovered only during annual external audit |
| 2 | Cross-reference errors — employee ID mismatches between IT system exports and PeopleHR roster due to Nepali/English name transliteration inconsistencies | High | Medium — terminated employees may be incorrectly matched as active | Late — discovered during manager review or not at all |
| 3 | Email delivery failure or workbook version confusion — managers work on outdated versions | Medium | Medium — review decisions based on stale data | Late — discovered when consolidating responses |
| 4 | Manager non-response or rubber-stamping — managers approve all access without genuine review due to workload pressure | High | High — excessive or unauthorized access persists undetected | Never — no mechanism to detect superficial reviews |
| 5 | Escalation delays — CISO unavailable or deprioritizes follow-up during peak periods (e.g., Dashain/Tihar festival season) | Medium | Medium — review cycle extends beyond NRB-mandated deadline | Late — detected only when deadline is missed |
| 6 | Change request errors — incorrect Jira tickets created due to manual data entry; wrong user or wrong system referenced | Medium | High — wrong user's access revoked (business disruption) or wrong user's access retained (security gap) | Late — discovered when user complains or during next review |
| 7 | Incomplete execution — some revocations not applied across all systems; user removed from AD but retains direct database access | High | High — user retains unauthorized access to sensitive payment data | Never — no automated verification of change completion |
| 8 | Evidence gaps — missing screenshots, unsigned review sheets, incomplete audit trail | High | Medium — audit findings, potential NRB penalties (up to NPR 5,00,000 per occurrence) | Late — discovered during external audit |
| 9 | Privileged account review limited to known accounts — undocumented service accounts or locally created admin accounts missed | Medium | High — undiscovered privileged access could be exploited | Never — no automated discovery of privileged accounts |
| 10 | Summary report inaccuracies — manual aggregation errors in statistics reported to CISO and NRB | Medium | Medium — misleading compliance posture; regulatory credibility risk | Late — if NRB cross-checks during on-site inspection |

### 2.3 Current Evidence Collection

| Evidence Type | Collection Method | Storage | Issues |
|---------------|-------------------|---------|--------|
| User account listing exports (CSV/XLSX) | Manual export from each system's admin console | Google Drive shared folder (`/GRC/Access Reviews/FY2081-82/`) | Exports are point-in-time snapshots; no integrity verification; files can be modified after export; naming conventions inconsistent |
| Manager sign-off sheets | Manual — managers print, sign, scan, and email back PDF; some managers use digital annotation | Google Drive same folder | Signature authenticity not verified; some managers send approval via email body text without signing the actual sheet; version control issues |
| Jira Service Management tickets | Semi-automated — tickets created manually but tracked in Jira | Jira Cloud (HDPS workspace) | Tickets not consistently linked to specific review cycle; free-text descriptions make automated analysis impossible |
| Change execution screenshots | Manual — SysAdmin takes screenshots of completed changes | Google Drive same folder | Screenshots can be fabricated; no timestamp verification; missing for ~30% of changes based on last audit |
| Privileged access register | Manual — maintained in Excel by Senior SysAdmin | Google Drive (`/GRC/Privileged Access/`) | Single point of failure; no change history; last known update gap of 45 days identified in FY2081 Q2 |
| Quarterly summary report | Manual — compiled by GRC Analyst in Word document | Google Drive + printed copy to CISO | Manual aggregation errors found in 2 of last 4 reports; NRB submission sometimes delayed |

---

## 3. Automation Design

### 3.1 Architecture Overview

The automated solution leverages an identity governance platform integrated with HDPS's existing infrastructure. The system continuously collects identity and access data from all connected systems, evaluates access against defined policies, orchestrates reviews through a web-based workflow, auto-generates evidence, and provides real-time dashboards.

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        HDPS NETWORK (Kathmandu DC + AWS ap-south-1)     │
│                                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌──────────────┐   │
│  │  Active      │  │  HimalPay   │  │  AWS IAM    │  │  PostgreSQL  │   │
│  │  Directory   │  │  Engine     │  │  (ap-south-1)│  │  (Payment DB)│   │
│  │  (On-Prem)   │  │  (On-Prem)  │  │             │  │              │   │
│  └──────┬───────┘  └──────┬──────┘  └──────┬──────┘  └──────┬───────┘   │
│         │                 │                 │                 │           │
│  ┌──────┴───────┐  ┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴───────┐   │
│  │  FortiGate   │  │  Cisco      │  │  PeopleHR   │  │  Jira Service│   │
│  │  Firewall    │  │  Switches   │  │  Nepal (HR) │  │  Management  │   │
│  └──────┬───────┘  └──────┬──────┘  └──────┴──────┘  └──────┬───────┘   │
│         │                 │                 │                 │           │
│         └────────┬────────┴────────┬────────┘                │           │
│                  │                 │                          │           │
│         ┌────────▼─────────────────▼────────┐                │           │
│         │     IDENTITY COLLECTOR SERVICE     │                │           │
│         │  (Docker Container - On-Prem K8s)  │                │           │
│         │  - LDAP Connector                  │                │           │
│         │  - REST API Connectors             │                │           │
│         │  - Database Connector              │                │           │
│         │  - SNMP/SSH Connector              │                │           │
│         └────────────────┬──────────────────┘                │           │
│                          │                                    │           │
│                  ┌───────▼───────────┐                       │           │
│                  │  MESSAGE QUEUE     │                       │           │
│                  │  (RabbitMQ)        │                       │           │
│                  └───────┬───────────┘                       │           │
│                          │                                    │           │
│              ┌───────────▼────────────────┐                  │           │
│              │   POLICY ENGINE SERVICE     │                  │           │
│              │   (Open Policy Agent - OPA) │                  │           │
│              │   - Least privilege rules   │                  │           │
│              │   - SoD violation rules     │                  │           │
│              │   - Dormant account rules   │                  │           │
│              │   - Orphan account rules    │                  │           │
│              │   - Privileged access rules │                  │           │
│              └───────────┬────────────────┘                  │           │
│                          │                                    │           │
│              ┌───────────▼────────────────┐                  │           │
│              │   CENTRAL DATA STORE        │                  │           │
│              │   (PostgreSQL + TimescaleDB) │                  │           │
│              │   - Identity snapshots      │                  │           │
│              │   - Policy evaluation logs  │                  │           │
│              │   - Review workflow state   │                  │           │
│              │   - Evidence records        │                  │           │
│              └──────┬────────────┬────────┘                  │           │
│                     │            │                            │           │
│         ┌───────────▼──┐  ┌─────▼──────────────┐            │           │
│         │  REVIEW       │  │  EVIDENCE            │            │           │
│         │  WORKFLOW      │  │  GENERATOR           │            │           │
│         │  ENGINE        │  │  SERVICE              │            │           │
│         │  (Custom App)  │  │  - PDF reports        │            │           │
│         │  - Web UI      │  │  - SHA-256 hashing    │◄───────────┘           │
│         │  - Email/SMS   │  │  - Immutable log      │                       │
│         │  - Escalation  │  │  - NRB format export  │                       │
│         └───────┬───────┘  └──────┬────────────────┘                       │
│                 │                  │                                         │
│         ┌───────▼──────────────────▼──────┐                                │
│         │        DASHBOARD & REPORTING     │                                │
│         │        (Grafana + Custom Web UI)  │                                │
│         └───────────────┬─────────────────┘                                │
│                         │                                                   │
│         ┌───────────────▼─────────────────┐                                │
│         │     ALERTING & NOTIFICATION      │                                │
│         │  - Email (SMTP via Sparrow SMS)  │                                │
│         │  - SMS (Sparrow SMS API)         │                                │
│         │  - Microsoft Teams Webhook       │                                │
│         │  - Jira Ticket Auto-Creation     │                                │
│         └─────────────────────────────────┘                                │
└──────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Data Sources

| Source System | Data Collected | Collection Method | Frequency |
|---------------|----------------|-------------------|-----------|
| Active Directory (On-Prem Windows Server 2019) | User accounts, group memberships, last logon timestamp, account status (enabled/disabled), password last set, OU structure | LDAP/LDAPS API (Port 636) | Every 6 hours |
| HimalPay Engine (Core Payment Application) | Application user accounts, assigned roles, transaction permission matrix, last login, module access | REST API (custom-built connector using HimalPay admin API v2.3) | Every 6 hours |
| AWS IAM (ap-south-1 region) | IAM users, roles, policies (inline + managed), access keys, last activity, MFA status, S3 bucket policies | AWS IAM API + AWS CloudTrail (via boto3 SDK) | Every 4 hours |
| PostgreSQL (Payment Transaction Database) | Database users, role grants, schema-level privileges, row-level security policies | Direct database query via read-only service account (pg_catalog, information_schema) | Daily at 01:00 NPT |
| FortiGate Firewall (FortiGate 200F) | Admin accounts, access profiles, trusted hosts, VPN user accounts | FortiOS REST API v2 | Daily at 02:00 NPT |
| Cisco Network Switches (Catalyst 9300 series) | Local user accounts, privilege levels, TACACS+ user mappings | SSH + SNMP v3 (automated script) | Daily at 02:30 NPT |
| PeopleHR Nepal (HR System) | Employee roster, department, designation, reporting manager, employment status (active/resigned/terminated), last working day, date of joining | REST API (PeopleHR API v3) | Every 4 hours |
| Jira Service Management | Access request tickets, change tickets, approval workflows, ticket resolution status | Atlassian REST API v3 | Real-time (webhook) + daily full sync |
| Microsoft Entra ID (Azure AD) | Cloud identity attributes, sign-in logs, conditional access policy assignments (used for O365 and SaaS apps) | Microsoft Graph API | Every 6 hours |

### 3.3 Processing Logic

| Rule ID | Condition | Action | Severity |
|---------|-----------|--------|----------|
| RULE-001 | User exists in any IT system but employment status in PeopleHR is "Terminated" or "Resigned" and last working day is ≥ 1 day ago | Auto-disable AD account immediately; create critical Jira ticket for access revocation across all systems within 4-hour SLA; alert CISO via SMS | Critical |
| RULE-002 | User has not logged into any system for > 90 consecutive days and employment status is "Active" | Flag as dormant account; send notification to user's manager for confirmation; if no response in 5 business days, auto-disable account | Medium |
| RULE-003 | User holds both "Payment Initiator" and "Payment Approver" roles in HimalPay Engine (SoD violation) | Block — prevent role assignment; if detected retroactively, create high-severity Jira ticket; alert Compliance team and CISO | High |
| RULE-004 | Any account added to Domain Admins, Enterprise Admins, or Schema Admins groups in AD, or granted root/superuser in PostgreSQL, or assigned AdministratorAccess policy in AWS IAM | Real-time alert to CISO and IT Manager via SMS + Teams; require retroactive approval within 24 hours or auto-revert; log to privileged access register | Critical |
| RULE-005 | AWS IAM user has active access keys older than 90 days | Create medium-severity ticket for key rotation; if not rotated within 7 days, escalate to high severity; after 14 days, auto-deactivate keys | Medium |
| RULE-006 | AWS IAM user or root account does not have MFA enabled | Create high-severity ticket; block console access after 48 hours if MFA not enabled (via IAM policy attachment) | High |
| RULE-007 | Number of active privileged accounts exceeds baseline threshold (currently: AD Domain Admins ≤ 4, AWS root usage = 0, PostgreSQL superusers ≤ 2) | Alert IT Manager and CISO; require justification for each excess account within 48 hours | High |
| RULE-008 | Quarterly access review cycle not initiated within first 5 business days of the quarter | Auto-initiate review campaign; alert GRC Analyst and CISO | Medium |
| RULE-009 | Manager has not completed assigned access review within 7 business days of assignment | Send daily reminder emails + SMS; after 10 days, escalate to manager's manager and CISO | Medium |
| RULE-010 | Service account password has not been rotated in > 180 days (tracked via credential vault metadata) | Create high-severity ticket for password rotation; alert SysAdmin team | High |
| RULE-011 | New system or application detected in network scan (CMDB mismatch) that is not in the access review scope | Alert GRC Analyst to assess and onboard system into review scope; create Jira ticket | Medium |
| RULE-012 | FortiGate or Cisco device has default or shared admin credentials detected (checked against known-default credential patterns) | Create critical ticket; alert Network team lead and CISO; require password change within 4 hours | Critical |

### 3.4 Detection Rules (YAML Format)

```yaml
# Orphaned Account Detection - Terminated Employees
- rule_id: RULE-001
  name: Orphaned Account - Terminated Employee
  description: Detects active IT accounts belonging to employees whose status in PeopleHR Nepal is Terminated or Resigned and whose last working day has passed. This is the highest priority rule as it directly addresses NRB directive requiring same-day access revocation upon employee separation.
  trigger: real_time
  data_sources:
    - peoplehr_nepal_api
    - active_directory_ldap
    - himalpay_engine_api
    - aws_iam_api
    - postgresql_catalog
    - fortigate_api
    - cisco_ssh
    - azure_ad_graph
  condition:
    - peoplehr.employment_status IN ('Terminated', 'Resigned')
    - peoplehr.last_working_day <= current_date()
    - any_system.account_status == 'Active' OR any_system.account_status == 'Enabled'
  action:
    - severity: critical
    - auto_remediate: true
    - auto_action: disable_ad_account_immediately
    - alert:
        - channel: sms
          recipient: ciso_anita_gurung (+977-98XXXXXXXX)
        - channel: microsoft_teams
          recipient: "#infosec-alerts"
        - channel: email
          recipient: it-admin@hdps.com.np, ciso@hdps.com.np
    - create_jira_ticket:
        project: SECOPS
        issue_type: Incident
        priority: Critical
        assignee: suman.tamang@hdps.com.np
        description: "Auto-generated: Orphaned account detected for terminated employee {employee_name} (EID: {employee_id}). AD account auto-disabled. Complete revocation across all systems required within 4 hours."
  sla: 4_hours
  evidence:
    - snapshot_of_peoplehr_record
    - snapshot_of_all_system_accounts
    - timestamp_of_auto_disable_action
    - jira_ticket_reference

# Dormant Account Detection
- rule_id: RULE-002
  name: Dormant Account Detection
  description: Identifies active user accounts with no login activity across any monitored system for more than 90 days, despite the employee being in active employment status.
  trigger: daily_scan
  schedule: "0 3 * * *"  # 3:00 AM NPT daily
  data_sources:
    - active_directory_ldap (lastLogonTimestamp)
    - himalpay_engine_api (last_login)
    - aws_iam_api (last_activity)
    - azure_ad_graph (signInActivity.lastSignInDateTime)
  condition:
    - peoplehr.employment_status == 'Active'
    - MAX(all_systems.last_login_timestamp) < (current_date() - 90_days)
    - account_status == 'Active' OR account_status == 'Enabled'
  action:
    - severity: medium
    - auto_remediate: false
    - alert:
        - channel: email
          recipient: "{user.manager_email}"
        - channel: microsoft_teams
          recipient: "#access-review-notifications"
    - notification_to_manager:
        message: "User {user_name} ({user_email}) has not logged into any system for {days_inactive} days. Please confirm if this account should remain active. If no response is received within 5 business days, the account will be automatically disabled."
        response_required: true
        response_deadline: 5_business_days
    - auto_disable_on_no_response: true
  sla: 5_business_days
  evidence:
    - login_activity_report
    - manager_notification_timestamp
    - manager_response_or_timeout_record

# Segregation of Duties Violation
- rule_id: RULE-003
  name: SoD Violation - Payment Initiator and Approver
  description: Detects when a single user holds both payment initiation and payment approval roles in HimalPay Engine, violating NRB segregation of duties requirements for Payment Service Providers.
  trigger: real_time
  data_sources:
    - himalpay_engine_api (user_roles)
  condition:
    - user.roles CONTAINS 'Payment_Initiator'
    - user.roles CONTAINS 'Payment_Approver'
  sod_conflict_pairs:
    - [Payment_Initiator, Payment_Approver]
    - [User_Creator, User_Approver]
    - [System_Admin, Audit_Log_Manager]
    - [Reconciliation_Preparer, Reconciliation_Approver]
  action:
    - severity: high
    - auto_remediate: false
    - alert:
        - channel: email
          recipient: compliance@hdps.com.np, ciso@hdps.com.np
        - channel: sms
          recipient: ciso_anita_gurung
        - channel: microsoft_teams
          recipient: "#compliance-alerts"
    - create_jira_ticket:
        project: COMPLIANCE
        issue_type: Risk
        priority: High
        assignee: priya.maharjan@hdps.com.np
        description: "SoD Violation detected: User {user_name} holds conflicting roles: {role_1} and {role_2} in HimalPay Engine. Requires immediate remediation per NRB IT Guidelines Chapter 9."
  sla: 24_hours
  evidence:
    - user_role_snapshot
    - sod_matrix_evaluation_result
    - jira_ticket_reference

# Privileged Access Escalation Detection
- rule_id: RULE-004
  name: Privileged Access Escalation Alert
  description: Detects any addition of accounts to high-privilege groups or roles across all monitored systems in real-time.
  trigger: real_time
  data_sources:
    - active_directory_ldap (security event 4728, 4732, 4756)
    - aws_cloudtrail (AttachUserPolicy, AddUserToGroup, CreateRole events)
    - postgresql_catalog (pg_auth_members changes)
  condition:
    - ad.group_membership_change IN ('Domain Admins', 'Enterprise Admins', 'Schema Admins', 'Account Operators', 'Backup Operators')
    - OR aws.iam_policy_attached IN ('AdministratorAccess', 'IAMFullAccess', 'PowerUserAccess')
    - OR postgresql.role_granted IN ('rds_superuser', 'pg_execute_server_program', 'pg_write_all_data')
  action:
    - severity: critical
    - auto_remediate: false
    - alert:
        - channel: sms
          recipient: ciso_anita_gurung, it_manager_dipak_kc
        - channel: microsoft_teams
          recipient: "#infosec-alerts"
        - channel: email
          recipient: ciso@hdps.com.np, it-manager@hdps.com.np
    - require_approval:
        approver: ciso@hdps.com.np
        deadline: 24_hours
        auto_revert_on_no_approval: true
    - create_jira_ticket:
        project: SECOPS
        issue_type: Change
        priority: Critical
  sla: 24_hours
  evidence:
    - event_log_snapshot
    - before_after_privilege_comparison
    - approval_or_revert_record

# AWS Access Key Rotation
- rule_id: RULE-005
  name: Stale AWS Access Key Detection
  description: Detects AWS IAM access keys that have not been rotated within the 90-day policy window.
  trigger: daily_scan
  schedule: "0 4 * * *"  # 4:00 AM NPT daily
  data_sources:
    - aws_iam_api (access_key_last_rotated)
  condition:
    - aws.access_key.status == 'Active'
    - aws.access_key.create_date < (current_date() - 90_days)
  action:
    - severity: medium
    - auto_remediate: false
    - alert:
        - channel: email
          recipient: "{aws_user.owner_email}"
        - channel: microsoft_teams
          recipient: "#devops-notifications"
    - create_jira_ticket:
        project: SECOPS
        issue_type: Task
        priority: Medium
    - escalation:
        - after: 7_days
          new_severity: high
          notify: it-manager@hdps.com.np
        - after: 14_days
          auto_action: deactivate_access_key
          new_severity: critical
          notify: ciso@hdps.com.np
  sla: 7_days
  evidence:
    - access_key_age_report
    - rotation_confirmation

# MFA Enforcement
- rule_id: RULE-006
  name: MFA Not Enabled on AWS IAM User
  description: Detects AWS IAM users and console-enabled accounts without MFA configured.
  trigger: daily_scan
  schedule: "0 4 30 * * *"
  data_sources:
    - aws_iam_api (mfa_devices, login_profile)
  condition:
    - aws.iam_user.login_profile_exists == true
    - aws.iam_user.mfa_devices_count == 0
  action:
    - severity: high
    - auto_remediate: false
    - alert:
        - channel: email
          recipient: "{aws_user.owner_email}", devops@hdps.com.np
        - channel: microsoft_teams
          recipient: "#infosec-alerts"
    - create_jira_ticket:
        project: SECOPS
        issue_type: Task
        priority: High
    - enforcement:
        after: 48_hours
        action: attach_deny_console_policy
        notify: ciso@hdps.com.np
  sla: 48_hours
  evidence:
    - mfa_status_report
    - enforcement_action_log

# Quarterly Access Review Campaign Auto-Trigger
- rule_id: RULE-008
  name: Quarterly Access Review Campaign Initiator
  description: Automatically initiates the quarterly access review campaign if not manually started within the first 5 business days of each quarter (Nepali fiscal year quarters - Q1 starts Shrawan).
  trigger: scheduled
  schedule: "0 9 * * 1-5"  # Check every weekday at 9 AM NPT during first 2 weeks of quarter
  condition:
    - current_date() >= quarter_start_date + 5_business_days
    - review_campaign.status != 'Active'
    - review_campaign.quarter == current_quarter
  action:
    - severity: medium
    - auto_remediate: true
    - auto_action: initiate_review_campaign
    - alert:
        - channel: email
          recipient: priya.maharjan@hdps.com.np, ciso@hdps.com.np
        - channel: microsoft_teams
          recipient: "#grc-notifications"
  sla: immediate
  evidence:
    - campaign_initiation_timestamp
    - system_auto_trigger_log

# Manager Review Overdue
- rule_id: RULE-009
  name: Access Review Manager Response Overdue
  description: Tracks manager response times during quarterly access review campaigns and escalates overdue reviews.
  trigger: daily_scan
  schedule: "0 9 * * 1-5"
  condition:
    - review_assignment.status == 'Pending'
    - review_assignment.assigned_date < (current_date() - 7_business_days)
  action:
    - severity: medium
    - auto_remediate: false
    - alert:
        - channel: email
          recipient: "{manager_email}"
          message: "REMINDER: Your access review for {department} is overdue by {days_overdue} days. Please complete it at https://iam.hdps.com.np/review/{review_id}"
        - channel: sms
          recipient: "{manager_phone}"
    - escalation:
        - after: 10_business_days
          notify: "{manager.reporting_manager_email}", ciso@hdps.com.np
          severity: high
        - after: 15_business_days
          notify: ceo@hdps.com.np
          severity: critical
  sla: 10_business_days
  evidence:
    - assignment_timestamp
    - reminder_delivery_log
    - escalation_log
    - completion_timestamp
```

### 3.5 Alert and Response Workflow

```
                    ┌──────────────────────┐
                    │   Event Detected by   │
                    │   Policy Engine (OPA)  │
                    └──────────┬───────────┘
                               │
                    ┌──────────▼───────────┐
                    │  Severity Assessment  │
                    │  & Classification     │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
     ┌────────▼──────┐ ┌──────▼───────┐ ┌──────▼────────┐
     │   CRITICAL     │ │   HIGH        │ │  MEDIUM/LOW   │
     │   (RULE-001,   │ │   (RULE-003,  │ │  (RULE-002,   │
     │    004, 012)   │ │    006, 007,  │ │   005, 008,   │
     │                │ │    010)       │ │   009, 011)   │
     └────────┬──────┘ └──────┬───────┘ └──────┬────────┘
              │                │                │
     ┌────────▼──────┐ ┌──────▼───────┐ ┌──────▼────────┐
     │ Immediate:     │ │ Within 1hr:  │ │ Within 4hrs:  │
     │ • Auto-action  │ │ • SMS + Teams│ │ • Email       │
     │   (if config)  │ │   + Email    │ │ • Teams       │
     │ • SMS to CISO  │ │ • Jira High  │ │ • Jira Medium │
     │ • SMS to IT Mgr│ │              │ │               │
     │ • Teams alert  │ │              │ │               │
     │ • Jira Critical│ │              │ │               │
     └────────┬──────┘ └──────┬───────┘ └──────┬────────┘
              │                │                │
     ┌────────▼──────┐ ┌──────▼───────┐ ┌──────▼────────┐
     │ SLA: 4 hours   │ │ SLA: 24 hrs  │ │ SLA: 5-7 days │
     └────────┬──────┘ └──────┬───────┘ └──────┬────────┘
              │                │                │
              └────────────────┼────────────────┘
                               │
                    ┌──────────▼───────────┐
                    │  Response Tracking    │
                    │  (Jira + Dashboard)   │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼──────────────┐
              │                │              │
     ┌────────▼──────┐ ┌──────▼─────┐ ┌──────▼────────┐
     │ Resolved       │ │ Escalated   │ │ Auto-Resolved │
     │ within SLA     │ │ (SLA breach)│ │ (auto-action) │
     └────────┬──────┘ └──────┬─────┘ └──────┬────────┘
              │                │              │
              └────────────────┼──────────────┘
                               │
                    ┌──────────▼───────────┐
                    │  Evidence Package     │
                    │  Auto-Generated       │
                    │  (SHA-256 hashed,     │
                    │   timestamped,        │
                    │   stored immutably)   │
                    └──────────────────────┘
```

| Alert Type | Recipient | Response SLA | Escalation |
|------------|-----------|--------------|------------|
| Critical — Orphaned Account (terminated employee) | CISO (Anita Gurung), IT Admin Team Lead (Suman Tamang) via SMS + Teams + Email | 4 hours | If unresolved in 4 hrs → CEO (Ramesh Pandey) + auto-report to NRB incident log |
| Critical — Unauthorized Privilege Escalation | CISO, IT Manager (Dipak KC) via SMS + Teams | 24 hours (with auto-revert if no approval) | If no approval in 24 hrs → auto-revert change; notify CEO |
| Critical — Default Credentials on Network Device | Network Team Lead (Sunil Basnet), CISO via SMS + Teams | 4 hours | If unresolved in 4 hrs → CISO takes direct action; network segment isolation considered |
| High — SoD Violation | Compliance Officer (Priya Maharjan), CISO via Email + Teams | 24 hours | If unresolved in 48 hrs → Head of Operations + CEO |
| High — MFA Not Enabled | DevOps Lead (Arun Thapa), IT Manager via Email + Teams | 48 hours (then auto-enforce) | After 48 hrs → auto-attach deny policy; notify CISO |
| High — Privileged Account Count Exceeded | IT Manager, CISO via Email + Teams | 48 hours | If unresolved in 72 hrs → CEO |
| High — Stale Service Account Password | SysAdmin Team (Bikash Adhikari) via Email | 7 days | After 7 days → IT Manager; after 14 days → CISO |
| Medium — Dormant Account | User's Direct Manager via Email | 5 business days | If no response → auto-disable; notify HR |
| Medium — Stale AWS Access Key | Key Owner + DevOps Lead via Email | 7 days | After 7 days → High; after 14 days → auto-deactivate |
| Medium — Review Campaign Overdue | GRC Analyst (Priya Maharjan), CISO via Email + Teams | Immediate auto-trigger | N/A — auto-resolved |
| Medium — Manager Review Overdue | Respective Manager via Email + SMS | 7 business days | After 10 days → Manager's manager + CISO; after 15 days → CEO |

---

## 4. Evidence Generation

### 4.1 Automated Evidence Types

| Evidence Type | Format | Generation | Storage | Retention |
|---------------|--------|------------|---------|-----------|
| Complete User Access Inventory Snapshot | JSON (machine-readable) + PDF (human-readable summary) | Daily at 05:00 NPT | PostgreSQL (TimescaleDB for time-series) + encrypted S3 bucket (ap-south-1) with versioning enabled | 7 years (per NRB record retention directive) |
| Access Review Campaign Records (decisions, timestamps, reviewer identity) | JSON + PDF (auditor-ready report) | Real-time (as each decision is made) + end-of-campaign summary | PostgreSQL + S3 | 7 years |
| Privileged Account Register (automated) | JSON + CSV + PDF | Daily snapshot at 05:30 NPT | PostgreSQL + S3 | 7 years |
| SoD Violation Detection Log | JSON + PDF incident report | Real-time per detection | PostgreSQL + S3 | 7 years |
| Orphaned Account Detection & Remediation Log | JSON + PDF (with before/after screenshots auto-captured) | Real-time per detection | PostgreSQL + S3 | 7 years |
| Policy Evaluation Audit Trail (every rule execution with inputs, outputs, decision rationale) | JSON (structured log) | Real-time | TimescaleDB + replicated to S3 daily | 5 years |
| Manager Review Response Tracking (assignment, reminders sent, response received, escalations) | JSON + PDF timeline | Real-time during campaign | PostgreSQL + S3 | 7 years |
| Alert and Incident Response Records (from detection to resolution) | JSON + linked Jira ticket export (PDF) | Real-time | PostgreSQL + Jira + S3 | 7 years |
| System Configuration Baseline Comparison (access control configurations across all systems) | JSON diff + PDF summary | Weekly (Saturday 02:00 NPT) | PostgreSQL + S3 | 5 years |
| NRB Quarterly Compliance Submission Package | PDF (NRB prescribed format) + supporting CSV data files | Quarterly (auto-generated on campaign completion) | S3 + printed copy archive | 7 years |

### 4.2 Auditor-Ready Reports

| Report Name | Contents | Frequency | Automation Level |
|-------------|----------|-----------|------------------|
| Quarterly Access Review Summary Report | Total users reviewed, decisions breakdown (retain/modify/revoke), completion rate, average response time, SLA compliance, SoD violations found, orphaned accounts remediated, exceptions granted with justification | Quarterly | Full — auto-generated upon campaign completion |
| Monthly Privileged Access Report | Count and list of all privileged accounts per system, changes from previous month (new additions, removals), justification for each privileged account, compliance with baseline thresholds | Monthly | Full |
| Real-time Access Risk Dashboard Export | Current orphaned accounts, dormant accounts, SoD violations, pending reviews, overdue items, privileged account status, MFA compliance rate | On-demand (exportable anytime) | Full |
| Annual User Access Trend Report | Year-over-year metrics: total accounts, privileged accounts, average review completion time, SoD violation trends, orphaned account detection rate, compliance score trend | Annually | Full |
| NRB IT Examination Readiness Package | All quarterly reports, privileged access registers, incident records, policy documents, procedure evidence, training records — compiled per NRB examination checklist | On-demand (pre-examination) | Partial — auto-compiled but requires GRC Analyst to verify completeness and add narrative sections |
| ISO 27001 Clause A.8 Evidence Package | Compiled evidence mapped to ISO 27001:2022 Annex A controls A.8.2, A.8.3, A.8.4, A.8.5 with cross-references to HDPS ISMS policies | Annually / On-demand for surveillance audits | Partial — auto-compiled evidence with manual policy mapping narrative |
| SOC 2 CC6 Evidence Package | Evidence mapped to SOC 2 CC6.1, CC6.2, CC6.3 criteria with testing procedure documentation | Annually for SOC 2 Type II audit period | Partial — evidence auto-compiled; auditor walkthrough documentation added manually |
| Exception and Waiver Register Report | All access exceptions granted, approver, justification, expiry date, review status | Monthly | Full |

### 4.3 Evidence Integrity

Evidence integrity is maintained through a multi-layered approach:

1. **Cryptographic Hashing:** Every evidence artifact (JSON record, PDF report, CSV export) is hashed using SHA-256 at the moment of generation. The hash is stored in a separate integrity database table and also written to an append-only log.

2. **Immutable Storage:** Evidence files stored in AWS S3 are protected by:
   - S3 Object Lock (Compliance mode) — objects cannot be deleted or overwritten for the retention period
   - S3 Versioning enabled — all versions preserved
   - Bucket policy preventing deletion by any principal except a break-glass role requiring 2-person authorization
   - Server-side encryption with AWS KMS (customer-managed key, key rotation every 365 days)

3. **Database Audit Trail:** PostgreSQL audit logging (pgAudit extension) is enabled on all evidence-related tables. Any SELECT, INSERT, UPDATE, DELETE is logged with the executing user, timestamp, and query text. These audit logs are shipped to a separate read-only logging database.

4. **Tamper Detection:** A daily integrity verification job computes SHA-256 hashes of all stored evidence files and compares them against the hash registry. Any mismatch triggers a critical alert to CISO and GRC Analyst.

5. **Chain of Custody:** Each evidence record includes: generator identity (system service account), generation timestamp (NTP-synchronized from ntp.ntc.net.np), associated control/rule ID, review campaign reference, and digital signature using the system's X.509 certificate.

6. **Access Controls:** Evidence storage access is restricted to:
   - GRC Analyst (Priya Maharjan) — read-only
   - CISO (Anita Gurung) — read-only
   - External Auditors — read-only via time-limited pre-signed S3 URLs generated per engagement
   - System service account — write-only (cannot delete)
   - Break-glass access — requires approval from both CISO and CEO

---

## 5. What Remains Manual

### 5.1 Manual vs Automated Comparison

| Activity | Current State | Target State | Justification for Manual |
|----------|---------------|--------------|-------------------------|
| User account data collection from all systems | Manual (2 days) | Automated | N/A |
| Cross-referencing IT accounts with HR employee roster | Manual (3 days) | Automated | N/A |
| Distribution of review assignments to managers | Manual (0.5 days via email) | Automated (web-based workflow with auto-assignment) | N/A |
| Manager access review decisions | Manual (5-15 days via spreadsheet) | Semi-Automated (web-based interface with AI-suggested decisions based on usage patterns; manager confirms/overrides) | Manager judgment is legally and regulatorily required — NRB mandates that the "data owner" (functional manager) makes access decisions; automation provides recommendations but the human must make and own the decision |
| Follow-up and escalation for overdue reviews | Manual (3-7 days) | Automated (auto-reminders and escalation per defined SLA timelines) | N/A |
| Consolidation of review decisions | Manual (2 days) | Automated | N/A |
| Execution of access changes (revocation/modification) | Manual (3-5 days) | Semi-Automated (auto-execution for standard revocations in AD, AWS IAM; manual for HimalPay Engine due to lack of write API, and for network devices) | HimalPay Engine vendor (Nepal Digital Systems Pvt. Ltd.) does not currently expose a user management write API; changes require admin console access. Network device changes require manual SSH sessions due to risk of automated misconfiguration causing payment network outage. Vendor API development is on roadmap for FY2082-83. |
| Evidence package compilation | Manual (2 days) | Automated | N/A |
| Privileged account discovery | Manual (1 day, incomplete) | Automated (continuous scanning) | N/A |
| New system/application onboarding into review scope | Manual | Manual | Requires architectural assessment, connector development, policy definition, and testing for each new system. Cannot be fully automated as each system has unique integration requirements. GRC Analyst performs initial assessment, Engineering develops connector. |
| Exception/waiver approval for access reviews | Manual | Manual | Requires human judgment from CISO to evaluate business justification for access exceptions, compensating controls, and risk acceptance. This is a governance decision that should not be automated. |
| Policy and rule definition/updates | Manual | Manual | Requires human expertise to interpret regulatory changes (NRB directives, ISO standard updates), translate them into technical rules, and validate correctness. GRC Analyst + Engineering collaboration required. |
| Annual access review process effectiveness assessment | Manual | Manual | Requires holistic assessment considering organizational context, risk appetite changes, regulatory landscape, and stakeholder feedback. Produces process improvement recommendations. |
| NRB examination response and auditor queries | Manual | Manual (with automated evidence retrieval) | Regulatory examinations require human interaction, explanation of controls, and professional judgment in responding to examiner questions. System provides rapid evidence retrieval to support responses. |

### 5.2 Human Decision Points

| Decision Point | Automation Support | Human Responsibility |
|----------------|-------------------|---------------------|
| Individual user access retain/modify/revoke during quarterly review | System provides: last login date across all systems, role-to-job-function mapping score, peer comparison analysis, access usage heatmap, risk score, AI recommendation (retain/modify/revoke with confidence %) | Manager makes final decision. Must provide justification if overriding system recommendation to "revoke". Decision is timestamped and attributed to the manager for audit trail. |
| Privileged access approval (new privileged account requests) | System provides: current privileged account count vs. baseline, requestor's role/department, existing access profile, SoD conflict analysis, risk assessment | CISO approves or denies based on principle of least privilege, business necessity, and compensating controls. All approvals require documented justification. |
| Exception/waiver for access that violates policy (e.g., temporary SoD exception) | System provides: specific policy violation details, risk rating, recommended compensating controls, similar past exceptions and their outcomes | CISO reviews and approves with mandatory: business justification, compensating controls, expiry date (max 90 days), and planned remediation timeline. |
| New system onboarding into access review scope | System detects new systems via network scanning and CMDB comparison, flags for review | GRC Analyst assesses criticality, data classification, regulatory applicability, and determines: review frequency, connector requirements, and policy rules to apply. |
| Rule tuning (adjusting thresholds, adding new detection rules) | System provides: false positive/negative rates, alert volume trends, rule effectiveness metrics | GRC Analyst and Engineering Lead jointly review monthly and adjust rules. Changes go through change management process with CISO approval. |
| Response to critical alerts requiring investigation | System provides: full context including user profile, access history, related events timeline, automated initial triage data | IT Security team (under CISO direction) investigates, determines root cause, assesses impact, and decides on remediation actions beyond automated responses. |
| Risk acceptance for residual risks identified by automation | System provides: risk description, quantified impact (where possible), affected systems, proposed mitigations | CISO presents to Risk Committee (CEO, CFO, CISO, Head of Operations) for formal risk acceptance decisions documented in risk register. |

---

## 6. Technology Stack

### 6.1 Component Selection

| Component | Technology Options Evaluated | Selected | Justification |
|-----------|-------------------|----------|---------------|
| Data Collection / Identity Connectors | Apache NiFi, Custom Python scripts, Mulesoft, open-source IAM connectors | **Custom Python microservices (FastAPI) deployed on Docker/Kubernetes** | HDPS has strong Python engineering capability; allows tailored connectors for HimalPay Engine's non-standard API; lower licensing cost than commercial tools; full control over data flow; existing team expertise reduces training overhead |
| Data Storage (Operational) | PostgreSQL, MySQL, MongoDB | **PostgreSQL 16 with TimescaleDB extension** | Already used in HDPS infrastructure (team expertise); TimescaleDB provides time-series optimization for access snapshots and audit trails; strong ACID compliance for evidence integrity; pgAudit extension for database-level audit logging |
| Data Storage (Evidence Archive) | AWS S3, MinIO (on-prem), Azure Blob | **AWS S3 (ap-south-1 Mumbai region)** with Object Lock | Closest AWS region to Nepal; Object Lock compliance mode ensures immutability; cost-effective for long-term retention; integrates with existing AWS infrastructure; KMS encryption available |
| Policy Engine | Open Policy Agent (OPA), Drools, custom rule engine | **Open Policy Agent (OPA) with Rego policies** | Industry standard for policy-as-code; declarative language (Rego) allows GRC team to understand and review rules; well-documented; active community; integrates easily with Python services via REST API; no licensing cost |
| Message Queue | RabbitMQ, Apache Kafka, AWS SQS | **RabbitMQ** | Sufficient throughput for HDPS scale (~500 user accounts, ~10 systems); simpler operations than Kafka; existing team knowledge; supports message persistence and acknowledgment; Docker-friendly |
| Workflow Engine (Review Campaigns) | Custom web application, Camunda, Activiti | **Custom web application (React frontend + FastAPI backend)** | Tailored to HDPS review workflow; integrates tightly with evidence generation; allows Nepali language support for manager-facing UI; existing React/Python team capability |
| Alerting & Notification | PagerDuty, Opsgenie, custom integration | **Custom integration layer:** Sparrow SMS API (for SMS in Nepal), Microsoft 365 SMTP (email), Microsoft Teams Incoming Webhook, Jira REST API | Sparrow SMS is the most reliable SMS gateway in Nepal with NTC/Ncell coverage; Teams already used by HDPS; no additional licensing cost for alerting tools; Jira already in use for ITSM |
| Dashboards & Visualization | Grafana, Kibana, Metabase, custom | **Grafana (OSS)** with PostgreSQL/TimescaleDB data source | Excellent time-series visualization for access trends; customizable dashboards; alerting integration; no licensing cost for OSS edition; team has existing Grafana skills from infrastructure monitoring |
| Evidence Generation (Reports) | JasperReports, custom Python (ReportLab, Jinja2), WeasyPrint | **Custom Python service using Jinja2 templates + WeasyPrint (PDF generation)** | Full control over report format and content; supports NRB-prescribed formats; can generate Nepali-language sections; integrates with existing Python stack; WeasyPrint produces professional PDF output |
| Container Orchestration | Docker Compose, Kubernetes (K3s), AWS ECS | **K3s (lightweight Kubernetes)** on on-premises servers | HDPS already runs K3s for other services; provides container orchestration, health checks, auto-restart; appropriate scale for HDPS; avoids full Kubernetes complexity |
| Secrets Management | HashiCorp Vault, AWS Secrets Manager, custom | **HashiCorp Vault (OSS)** | Manages all service account credentials, API keys, database passwords used by the automation platform; dynamic credential generation for database connectors; audit logging of all secret access; existing deployment at HDPS |
| Version Control (Policy-as-Code) | GitHub, GitLab, Bitbucket | **GitHub (HDPS organization account)** | All OPA Rego policies, detection rules (YAML), connector configurations stored in Git; pull request workflow for rule changes; audit trail of all policy modifications; existing HDPS practice |

### 6.2 Integration Points

| System | Integration Type | Data Flow | Authentication | Notes |
|--------|-----------------|-----------|----------------|-------|
| Active Directory (On-Prem) | LDAP/LDAPS (Port 636) | Inbound (read user/group data) | Service account (svc-iam-collector) with read-only permissions on relevant OUs; certificate-based TLS | Connector runs every 6 hours; also subscribes to AD change notifications for real-time privilege escalation detection |
| HimalPay Engine | REST API (HTTPS) | Inbound (read user/role data) | API key + IP whitelisting; key stored in Vault; rotated every 90 days | Read-only API; no write API available currently — vendor developing write API for FY2082-83 |
| AWS IAM | AWS SDK (boto3) | Inbound (read IAM data) + Outbound (auto-remediate: disable keys, attach deny policies) | IAM role with minimal permissions (iam:List*, iam:Get*, iam:UpdateAccessKey, iam:AttachUserPolicy for specific deny policies); assumed via cross-account role | Separate IAM role for read vs. write operations; write operations require additional approval token from Vault |
| AWS CloudTrail | AWS SDK (boto3) + S3 event notifications | Inbound (read management events) | Same IAM role as above with cloudtrail:LookupEvents permission | Near real-time event streaming for privilege escalation detection |
| PostgreSQL (Payment DB) | Direct database connection (psycopg2) | Inbound (read pg_catalog, information_schema) | Dedicated read-only service account (svc_iam_audit); SSL required; password rotated via Vault dynamic credentials | Runs daily; queries pg_roles, pg_auth_members, information_schema.role_table_grants |
| FortiGate Firewall | FortiOS REST API v2 (HTTPS) | Inbound (read admin accounts, access profiles) | API token; IP whitelisting; token stored in Vault | Daily scan; read-only access |
| Cisco Switches | SSH (Paramiko library) + SNMP v3 | Inbound (read local users, privilege levels) | SSH key-based auth for service account; SNMP v3 with authPriv (SHA + AES256); credentials in Vault | Daily scan; show running-config parsed for user accounts |
| PeopleHR Nepal | REST API (HTTPS) | Inbound (read employee data) | OAuth 2.0 client credentials; client secret in Vault | Every 4 hours; provides authoritative HR data for joiner/mover/leaver correlation |
| Microsoft Entra ID (Azure AD) | Microsoft Graph API (HTTPS) | Inbound (read user profiles, sign-in logs) | OAuth 2.0 app registration with Application permissions (User.Read.All, AuditLog.Read.All); client certificate auth | Every 6 hours; provides cloud identity and sign-in activity data |
| Jira Service Management | Atlassian REST API v3 (HTTPS) | Inbound (read tickets) + Outbound (create/update tickets) | API token (Atlassian account); stored in Vault | Real-time webhook for incoming ticket events; REST API for creating automated tickets |
| Microsoft Teams | Incoming Webhook (HTTPS) | Outbound (send alerts/notifications) | Webhook URL stored in Vault; channel-specific webhooks for #infosec-alerts, #grc-notifications, #access-review-notifications, #compliance-alerts, #devops-notifications | Adaptive cards format for rich notifications |
| Sparrow SMS | REST API (HTTPS) | Outbound (send SMS alerts) | API token + sender identity; stored in Vault | Used for critical/high severity alerts to CISO and IT Manager; supports NTC and Ncell networks |
| Grafana | PostgreSQL data source (direct DB connection) | Inbound (read dashboard data) | Dedicated read-only database user for Grafana; LDAP authentication for Grafana user login | Real-time dashboards; embedded in internal portal |
| HashiCorp Vault | REST API / Agent | Both (read secrets, write audit logs) | AppRole authentication for each microservice; TLS mutual auth | Central secrets management for all integration credentials |

---

## 7. Residual Risk Assessment

### 7.1 Risks Addressed by Automation

| Risk | Before Automation | After Automation | Reduction |
|------|-------------------|------------------|-----------|
| Orphaned accounts (terminated employees retaining access) | High — average detection time 30+ days; 12 orphaned accounts found in last NRB examination | Low — real-time detection with auto-disable within minutes; full remediation within 4-hour SLA | ~95% risk reduction |
| Incomplete access reviews (not all systems covered, not all users reviewed) | High — 2-3 systems routinely missed; ~15% of users not reviewed per cycle due to manager non-response | Low — automated system inventory ensures all in-scope systems included; escalation workflow ensures 100% completion | ~90% risk reduction |
| Late review completion (exceeding NRB quarterly deadline) | High — 3 of last 4 quarters exceeded deadline; NRB verbal warning received | Low — auto-initiation, auto-reminders, escalation ensure on-time completion | ~85% risk reduction |
| SoD violations undetected | High — no systematic SoD checking; 4 violations discovered during last ISO audit | Low — real-time SoD detection with defined conflict matrix; prevention at role assignment time | ~95% risk reduction |
| Excessive privileged access (more admins than necessary) | Medium — privileged access register manually maintained and often outdated | Low — continuous monitoring with baseline thresholds; real-time alerting on changes | ~80% risk reduction |
| Dormant accounts (unused but active accounts) | High — no systematic detection; estimated 25-30 dormant accounts across systems | Low — daily automated scanning with 90-day inactivity threshold | ~90% risk reduction |
| Evidence gaps and integrity issues | High — missing evidence for ~30% of changes; evidence tampering possible; inconsistent formats | Low — automated evidence generation with cryptographic integrity verification; immutable storage | ~95% risk reduction |
| Manager rubber-stamping reviews | High — no mechanism to detect or prevent cursory reviews | Medium — system provides usage data and AI recommendations, tracks review time per user, flags suspiciously fast bulk approvals for GRC Analyst review | ~50% risk reduction (inherently difficult to fully eliminate) |
| Stale AWS access keys and missing MFA | Medium — periodic manual checks often delayed | Low — daily automated scanning with auto-enforcement deadlines | ~85% risk reduction |
| NRB reporting inaccuracies | Medium — manual compilation errors in 50% of reports | Low — auto-generated reports with validated data | ~90% risk reduction |

### 7.2 Residual Risks

| Risk | Description | Mitigation | Acceptance |
|------|-------------|------------|------------|
| Manager rubber-stamping despite automation support | Even with AI recommendations and usage data, managers may still approve all access without genuine review due to time pressure or lack of security awareness. Automation can detect suspiciously fast reviews but cannot force thoroughness. | 1. Track and report per-manager review quality metrics (time spent, override rate vs. AI recommendations). 2. Annual security awareness training with specific module on access review responsibility (mandatory per NRB). 3. CISO quarterly review of manager review quality metrics. 4. Random sampling: GRC Analyst independently verifies 10% of manager decisions each quarter. | Required — CISO accepts residual risk with compensating controls; reviewed quarterly |
| Systems not integrated into automation platform | Some systems may not be covered: new applications deployed without notifying GRC team, legacy systems without APIs (e.g., old SPARROW interbank system), vendor-managed systems where HDPS has limited visibility. | 1. RULE-011 detects new systems via network scanning. 2. Change management process requires GRC assessment for all new system deployments. 3. Maintain manual review process for non-integrated systems (currently 2 systems). 4. Quarterly reconciliation of CMDB against automation platform system inventory. | Required — GRC Lead accepts; target is <5% of systems unintegrated at any time |
| HimalPay Engine access changes remain manual | Until vendor provides write API, access revocations in HimalPay Engine require manual admin console action, introducing delay and human error risk. | 1. Auto-generated Jira tickets with exact change instructions. 2. Mandatory screenshot evidence of completed changes. 3. Post-change verification scan to confirm execution. 4. Vendor contract amendment requiring write API delivery by FY2082-83 Q2. | Required — IT Manager accepts; vendor timeline tracked monthly |
| Insider threat by automation platform administrators | The platform administrators (Engineering team) have access to modify rules, disable alerts, or tamper with evidence generation logic. | 1. Separation of duties: GRC team defines rules (Rego policies in GitHub); Engineering team deploys infrastructure. 2. All policy changes require pull request approval from GRC Lead. 3. Platform admin actions logged via Vault audit log and K8s audit log. 4. Quarterly review of platform admin access and actions by CISO. 5. Evidence integrity verification runs on a separate system outside platform admin control. | Required — CISO accepts with compensating controls |
| Data quality dependency on PeopleHR Nepal | If HR system data is inaccurate or delayed (e.g., termination not entered promptly), orphaned account detection is delayed. | 1. SLA with HR department: employment status changes must be updated in PeopleHR within 4 business hours. 2. Daily reconciliation report comparing PeopleHR active employee count with previous day's count to detect unusual patterns. 3. Physical security team (badge deactivation) process provides secondary signal. 4. Monthly HR data quality audit. | Required — CISO and HR Director jointly accept |

### 7.3 Automation-Introduced Risks

| Risk | Description | Mitigation |
|------|-------------|------------|
| Automation failure / service outage | If the Identity Collector Service, Policy Engine, or Workflow Engine goes down, access reviews halt and real-time detections stop. A silent failure (service running but not processing correctly) is particularly dangerous. | 1. K8s health checks with auto-restart on failure. 2. Dedicated Grafana dashboard monitoring all service health metrics (see Section 9). 3. Heartbeat alerts: if no data received from any collector for > 2x expected interval, critical alert sent. 4. Fallback: manual process documentation maintained and tested annually. 5. Monthly chaos testing: randomly kill one service and verify detection/recovery. |
| False positives causing alert fatigue | Excessive false alerts (e.g., name matching errors, temporary system account false flags) may cause CISO/managers to ignore genuine alerts. | 1. Initial 30-day tuning period in "monitor-only" mode before enforcement. 2. Monthly rule tuning sessions based on false positive rate metrics. 3. Target false positive rate: <5% for critical alerts, <15% for medium alerts. 4. Alert severity tiering ensures critical alerts remain low-volume and high-fidelity. 5. Feedback mechanism: responders can mark alerts as "false positive" which feeds back into rule tuning. |
| False negatives (missed detections) | Automation may miss genuine violations due to: incomplete data collection, rule logic gaps, or novel attack patterns not covered by existing rules. | 1. Quarterly red team exercise: intentionally create test violations and verify detection. 2. Annual rule coverage assessment against updated threat model. 3. Manual spot-checks by GRC Analyst (10% random sampling). 4. External audit findings used as feedback to identify detection gaps. |
| Auto-remediation causing business disruption | Automated actions (e.g., auto-disabling accounts, auto-deactivating access keys, auto-attaching deny policies) may impact legitimate business operations if triggered incorrectly. | 1. Auto-remediation limited to well-understood, low-ambiguity scenarios (terminated employee accounts only for auto-disable). 2. "Soft" enforcement for most rules: create ticket + alert rather than auto-act. 3. Auto-remediation includes 5-minute delay with cancel option via Teams notification. 4. Break-glass procedure: IT Manager can immediately re-enable an incorrectly disabled account with post-facto justification. 5. Every auto-remediation action is logged and reviewed weekly. |
| Single point of failure — centralized platform | Concentrating all access control monitoring into one platform creates dependency; if compromised, attacker could manipulate evidence or suppress alerts. | 1. Platform access restricted to minimum necessary personnel. 2. Evidence integrity verification runs on a separate isolated system. 3. Critical alerts simultaneously delivered via multiple independent channels (SMS, Teams, email) — attacker would need to compromise all channels. 4. Quarterly external penetration test of the platform. 5. Network segmentation: platform on dedicated VLAN with firewall rules restricting inbound/outbound traffic. |
| Credential compromise of service accounts | Service accounts used by collectors have access to sensitive systems. If Vault is compromised or a service account credential is leaked, attacker could access multiple systems. | 1. Dynamic credentials from Vault (short-lived, rotated automatically). 2. Principle of least privilege: each service account has minimum necessary permissions. 3. IP allowlisting for all service accounts where supported. 4. Service account credential access logged in Vault audit trail. 5. Anomaly detection on service account behavior (unusual query patterns, off-hours access). |

---

## 8. Implementation Plan

### 8.1 Phases

| Phase | Scope | Duration | Dependencies |
|-------|-------|----------|--------------|
| **Phase 1: Foundation & Core Connectors** | Deploy base infrastructure (K3s cluster, PostgreSQL/TimescaleDB, RabbitMQ, Vault, Grafana). Develop and deploy connectors for: Active Directory, PeopleHR Nepal, AWS IAM. Implement RULE-001 (orphaned accounts) and RULE-004 (privilege escalation) in monitor-only mode. Set up alerting channels (Sparrow SMS, Teams, email). Build evidence storage layer with SHA-256 hashing and S3 integration. | 8 weeks | K3s cluster provisioned (existing); AWS S3 bucket created; Sparrow SMS account; AD service account created by IT Admin; PeopleHR API access granted by HR; Vault deployed (existing) |
| **Phase 2: Extended Connectors & Review Workflow** | Develop and deploy connectors for: HimalPay Engine, PostgreSQL Payment DB, FortiGate, Cisco switches, Microsoft Entra ID. Build the web-based access review workflow application (React + FastAPI). Implement all remaining rules (RULE-002, 003, 005-012). Deploy OPA policy engine with full Rego policy set. Switch RULE-001 and RULE-004 from monitor-only to enforcement mode. | 10 weeks | Phase 1 complete; HimalPay Engine API documentation from vendor; FortiGate API access enabled by Network team; Cisco SSH access configured; Review workflow UX design approved by CISO |
| **Phase 3: Review Campaign Pilot & Evidence Automation** | Run first automated quarterly access review campaign (FY2082 Q1) in parallel with manual process. Build all automated report templates (quarterly summary, privileged access, NRB submission, ISO/SOC 2 packages). Implement evidence integrity verification system. Develop and launch Grafana dashboards. Conduct user training for managers (review workflow) and IT team (alert response). | 8 weeks | Phase 2 complete; Manager training materials prepared; NRB report format confirmed; ISO 27001 auditor (Quality Certification Nepal) briefed on new evidence format |
| **Phase 4: Full Production & Optimization** | Decommission manual process. Enable all auto-remediation features. Conduct first month of full automated operation with intensive monitoring. Monthly rule tuning for 3 months. Conduct first quarterly red team test against the platform. Prepare for first NRB examination with automated evidence. External penetration test of the platform. | 6 weeks + ongoing optimization for 3 months | Phase 3 pilot successful (>95% accuracy); CISO sign-off on full production; Manager training completed (>90% attendance); Rollback plan tested |

### 8.2 Success Criteria

| Metric | Current (Baseline) | Target (Post Phase 4) | Measurement Method |
|--------|---------|--------|-------------|
| Time to detect orphaned account (terminated employee) | 30+ days (average) | < 1 hour | Measured from PeopleHR termination entry timestamp to alert generation timestamp; tracked in platform metrics |
| Time to complete full quarterly access review | 21-35 calendar days | ≤ 10 business days | Measured from campaign initiation to final manager sign-off; tracked by workflow engine |
| Access review completion rate (% of users reviewed) | ~85% (estimated) | 100% | Reviewed users / total users across all in-scope systems; auto-calculated by platform |
| False positive rate (critical alerts) | N/A (no automated detection) | < 5% | (False positive critical alerts / total critical alerts) per month; tracked via responder feedback |
| False positive rate (all alerts) | N/A | < 15% | Same as above for all severities |
| Evidence coverage (% of access changes with complete evidence) | ~70% | 100% | (Changes with complete evidence package / total changes) per quarter; auto-calculated |
| Manual effort per quarterly review cycle | ~120 person-hours (estimated across IT Admin, GRC, managers) | < 30 person-hours (primarily manager review time) | Time tracking in Jira + self-reported; target represents 75% reduction |
| SoD violations detected per quarter | 0-1 (found reactively during audits) | All violations detected in real-time (target: detect 100% of defined SoD conflicts) | Number of SoD violations detected by automation vs. found by auditors post-deployment |
| NRB quarterly submission timeliness | 3 of 4 quarters late in FY2081-82 | 100% on-time | NRB submission timestamp vs. deadline |
| Privileged account baseline compliance | Unknown (not continuously monitored) | ≥ 95% time in compliance with defined thresholds | (Hours in compliance / total hours per month) × 100; auto-measured |
| Mean time to remediate critical access violations | Unknown (estimated 7-14 days) | < 4 hours | Measured from alert to remediation confirmation; tracked in Jira |
| Platform uptime | N/A | ≥ 99.5% (excluding planned maintenance) | K8s health checks + Grafana uptime dashboard |

### 8.3 Rollback Plan

**Scenario 1: Partial automation failure (single component)**
- K8s auto-restart handles transient failures (target: <5 min recovery)
- If a specific connector fails, affected system falls back to manual export by IT Admin using documented manual procedures
- All other systems continue automated processing
- GRC Analyst notified to track manual gap

**Scenario 2: Critical platform failure (Policy Engine or Workflow Engine down)**
- Automated alerting via Grafana detects service down within 5 minutes
- Engineering on-call responds per incident procedure
- If recovery exceeds 4 hours:
  - CISO activates manual fallback process
  - IT Admin team executes pre-documented manual procedures for critical rules (RULE-001, RULE-004)
  - Manual procedures documentation maintained in `\\fileserver\GRC\Manual-Fallback-Procedures\` and tested quarterly
  - GRC Analyst initiates daily manual checks using saved SQL queries and scripts
- Evidence generated during manual fallback period is tagged as "manual-fallback" in the evidence repository

**Scenario 3: Full platform rollback (automation deemed unfit for purpose)**
- Decision authority: CISO with CEO approval
- Trigger criteria: platform accuracy below 80% after 90 days of optimization, or critical security incident caused by automation
- Rollback steps:
  1. Disable all auto-remediation actions (immediate)
  2. Switch all alerts to "monitor-only" (immediate)
  3. Reactivate manual spreadsheet-based review process (1-2 days)
  4. Maintain platform in read-only mode for evidence retrieval during transition
  5. Conduct root cause analysis and remediation plan within 30 days
  6. Re-evaluate platform with CISO, Engineering Lead, and external consultant before re-deployment

**Manual fallback procedures are tested:**
- Quarterly: tabletop exercise with IT Admin and GRC team
- Annually: full manual process execution (one quarter runs both manual and automated in parallel)

---

## 9. Monitoring and Maintenance

### 9.1 Automation Health Monitoring

| Metric | Threshold | Alert |
|--------|-----------|-------|
| Identity Collector job execution success rate | < 95% success rate over 24-hour window | Medium alert to Engineering Lead (Arun Thapa) via Teams + Email |
| Data freshness — time since last successful collection per source system | > 2× expected collection interval (e.g., > 12 hours for a 6-hourly job) | High alert to Engineering Lead + IT Manager via Teams + SMS |
| Policy Engine (OPA) response time | > 500ms average over 5-minute window | Medium alert to Engineering Lead |
| Policy Engine evaluation errors | Any evaluation error (OPA returns error instead of decision) | High alert to Engineering Lead + GRC Analyst |
| Evidence generation job completion | Any evidence generation failure | High alert to GRC Analyst + Engineering Lead |
| Evidence integrity verification (daily hash check) | Any hash mismatch detected | Critical alert to CISO + GRC Analyst + Engineering Lead via SMS + Teams |
| Alert delivery confirmation (SMS/email/Teams) | Any alert delivery failure (Sparrow SMS API error, Teams webhook failure, SMTP error) | High alert to Engineering Lead (via backup channel) |
| RabbitMQ queue depth | > 1000 messages in any queue (indicates processing backlog) | Medium alert to Engineering Lead |
| PostgreSQL database disk usage | > 80% disk utilization | Medium alert to SysAdmin team |
| PostgreSQL connection pool usage | > 80% pool utilization | Medium alert to Engineering Lead |
| K8s pod restart count | > 3 restarts for any pod in 1-hour window | High alert to Engineering Lead |
| S3 replication lag (for evidence backup) | > 1 hour replication lag | Medium alert to Engineering Lead |
| Vault seal status | Vault sealed (should never be sealed in production) | Critical alert to Engineering Lead + CISO via SMS |
| Certificate expiry (TLS certs for service communication) | < 30 days until expiry | Medium alert to Engineering Lead |
| Certificate expiry | < 7 days until expiry | Critical alert to Engineering Lead + IT Manager |

### 9.2 Ongoing Maintenance

| Activity | Frequency | Owner |
|----------|-----------|-------|
| Detection rule tuning (adjust thresholds, address false positives/negatives, update SoD conflict matrix) | Monthly (first Tuesday of each month) | GRC Analyst (Priya Maharjan) + Engineering Lead (Arun Thapa); changes approved by CISO (Anita Gurung) via GitHub pull request |
| Integration health check (verify all connectors functioning, test authentication, validate data completeness) | Weekly (automated) + Monthly (manual deep review) | Engineering Lead (Arun Thapa) — automated checks; SysAdmin (Bikash Adhikari) — monthly manual verification |
| Evidence integrity spot-check (manual verification of random evidence samples against source systems) | Monthly (10 samples per month) | GRC Analyst (Priya Maharjan) |
| OPA policy repository review (ensure rules align with current NRB directives, ISO 27001 controls, and HDPS policies) | Quarterly; ad-hoc when regulatory changes published | GRC Analyst + CISO |
| Service account credential rotation verification | Monthly | SysAdmin (Bikash Adhikari) — verify Vault dynamic credential rotation working correctly |
| Grafana dashboard review and update | Quarterly | GRC Analyst + Engineering Lead |
| Platform security patching (OS, Docker images, Python dependencies, PostgreSQL, OPA, Grafana) | Monthly (regular patches); immediate for critical CVEs | Engineering Lead + SysAdmin |
| Backup and restore testing (PostgreSQL database, Vault, platform configuration) | Quarterly | SysAdmin (Bikash Adhikari) |
| Disaster recovery test (simulate Kathmandu DC failure, verify AWS-based evidence accessibility) | Annually | Engineering Lead + SysAdmin; witnessed by CISO |
| Manual fallback procedure test | Quarterly (tabletop); Annually (full execution) | GRC Analyst + IT Admin Team |
| Capacity planning review (storage growth, compute utilization, user/system count trends) | Bi-annually | Engineering Lead |
| Vendor API change monitoring (HimalPay Engine, PeopleHR, FortiGate, etc. — check for API deprecations or changes) | Monthly | Engineering Lead |
| Red team test — create deliberate access violations to test detection | Quarterly | External consultant (coordinated by CISO) |
| External penetration test of automation platform | Annually | External security firm (e.g., Vairav Technology or CryptoGen Nepal) |
| User training refresh for managers (access review workflow) | Annually + onboarding for new managers | GRC Analyst + HR (training coordination) |
| Compliance mapping update (map automation evidence outputs to updated framework requirements) | Annually; ad-hoc when frameworks updated | GRC Analyst |

---

## 10. Appendices

### A. Current Process Flow Diagram

```
┌──────────┐    ┌──────────────┐    ┌───────────────┐    ┌──────────────┐
│ Quarter   │───▶│ IT Admin      │───▶│ Export user    │───▶│ Compile into │
│ begins    │    │ initiates     │    │ lists from     │    │ Excel work-  │
│           │    │ (often late)  │    │ each system    │    │ book (3 days)│
└──────────┘    └──────────────┘    │ (2 days)       │    └──────┬───────┘
                                     └───────────────┘           │
                    ┌────────────────────────────────────────────▼──────┐
                    │ Email workbooks to 12 dept managers (0.5 days)    │
                    └────────────────────────┬─────────────────────────┘
                                             │
                    ┌────────────────────────▼─────────────────────────┐
                    │ Managers review (5-15 business days)              │
                    │ • Some respond promptly                          │
                    │ • Some never respond without escalation          │
                    │ • Some rubber-stamp "approve all"                │
                    │ • Version confusion common                       │
                    └────────────────────────┬─────────────────────────┘
                                             │
                    ┌────────────────────────▼─────────────────────────┐
                    │ Follow up & escalation (3-7 days)                │
                    └────────────────────────┬─────────────────────────┘
                                             │
                    ┌────────────────────────▼─────────────────────────┐
                    │ Consolidate responses & create Jira tickets      │
                    │ (2 days)                                         │
                    └────────────────────────┬─────────────────────────┘
                                             │
                    ┌────────────────────────▼─────────────────────────┐
                    │ Execute access changes manually (3-5 days)       │
                    │ • ~30% missing change evidence                   │
                    └────────────────────────┬─────────────────────────┘
                                             │
                    ┌────────────────────────▼─────────────────────────┐
                    │ Compile evidence package (2 days)                 │
                    │ • Manual, error-prone, incomplete                 │
                    └────────────────────────┬─────────────────────────┘
                                             │
                    ┌────────────────────────▼─────────────────────────┐
                    │ TOTAL ELAPSED: 21-35 calendar days                │
                    │ TOTAL EFFORT: ~120 person-hours                   │
                    │ RESULT: Often late, incomplete, unreliable        │
                    └──────────────────────────────────────────────────┘
```

### B. Proposed Architecture Diagram

See Section 3.1 for the detailed architecture diagram.

### C. Rule Specification Details

See Section 3.3 for the processing logic table and Section 3.4 for the full YAML rule specifications.

**SoD Conflict Matrix (for RULE-003):**

| Role A | Role B | Conflict Reason | System |
|--------|--------|-----------------|--------|
| Payment_Initiator | Payment_Approver | Maker-checker violation; NRB requires dual control for payment transactions | HimalPay Engine |
| User_Creator | User_Approver | Maker-checker violation for user management | HimalPay Engine |
| System_Admin | Audit_Log_Manager | Admin should not be able to modify audit logs that record their own actions | HimalPay Engine |
| Reconciliation_Preparer | Reconciliation_Approver | Maker-checker violation for reconciliation process | HimalPay Engine |
| Domain Admins | Regular business user with payment access | Infrastructure admin should not have business transaction capability | Active Directory + HimalPay Engine (cross-system) |

**Dormant Account Thresholds (for RULE-002):**

| System | Inactivity Threshold | Special Considerations |
|--------|---------------------|----------------------|
| Active Directory | 90 days since lastLogonTimestamp | Exclude service accounts (identified by OU = "Service Accounts" or naming convention "svc-*") |
| HimalPay Engine | 90 days since last_login | Exclude system integration accounts |
| AWS IAM | 90 days since last activity (console or API) | Exclude programmatic-only accounts with recent API activity |
| PostgreSQL | 90 days since last connection (tracked via pg_stat_activity logging) | Exclude application service accounts |

### D. Evidence Samples

**Sample 1: Orphaned Account Detection Evidence (RULE-001)**

```json
{
  "evidence_id": "EVD-2082-001-00042",
  "rule_id": "RULE-001",
  "generated_at": "2082-04-15T14:23:07+05:45",
  "generated_by": "svc-policy-engine",
  "sha256_hash": "a3f2b8c1d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1",
  "employee": {
    "employee_id": "HDPS-EMP-0147",
    "full_name": "Sarita Thapa",
    "department": "Customer Support",
    "designation": "Senior Support Executive",
    "employment_status": "Terminated",
    "last_working_day": "2082-04-14",
    "termination_entered_in_peoplehr": "2082-04-14T17:30:00+05:45",
    "reporting_manager": "Deepa Shrestha (HDPS-EMP-0089)"
  },
  "affected_accounts": [
    {
      "system": "Active Directory",
      "username": "sarita.thapa",
      "account_status_before": "Enabled",
      "groups": ["Domain Users", "Customer Support", "VPN Users"],
      "last_logon": "2082-04-14T16:45:22+05:45",
      "auto_action_taken": "Account disabled",
      "auto_action_timestamp": "2082-04-15T14:23:15+05:45"
    },
    {
      "system": "HimalPay Engine",
      "username": "s.thapa",
      "account_status_before": "Active",
      "roles": ["Support_View", "Transaction_Query"],
      "last_login": "2082-04-14T16:30:00+05:45",
      "auto_action_taken": "None (manual remediation required - no write API)",
      "jira_ticket_created": "SECOPS-1234"
    },
    {
      "system": "Microsoft Entra ID",
      "username": "sarita.thapa@hdps.com.np",
      "account_status_before": "Active",
      "licenses": ["Microsoft 365 Business Basic"],
      "last_sign_in": "2082-04-14T16:50:00+05:45",
      "auto_action_taken": "Sign-in blocked",
      "auto_action_timestamp": "2082-04-15T14:23:18+05:45"
    }
  ],
  "alerts_sent": [
    {
      "channel": "SMS",
      "recipient": "CISO (Anita Gurung) +977-98XXXXXXXX",
      "sent_at": "2082-04-15T14:23:20+05:45",
      "delivery_status": "Delivered"
    },
    {
      "channel": "Microsoft Teams",
      "recipient": "#infosec-alerts",
      "sent_at": "2082-04-15T14:23:21+05:45",
      "delivery_status": "Delivered"
    },
    {
      "channel": "Email",
      "recipient": "it-admin@hdps.com.np, ciso@hdps.com.np",
      "sent_at": "2082-04-15T14:23:22+05:45",
      "delivery_status": "Delivered"
    }
  ],
  "jira_ticket": {
    "key": "SECOPS-1234",
    "priority": "Critical",
    "assignee": "suman.tamang@hdps.com.np",
    "sla_deadline": "2082-04-15T18:23:07+05:45",
    "status": "In Progress"
  },
  "remediation_target_sla": "4 hours",
  "evidence_integrity": {
    "hash_algorithm": "SHA-256",
    "hash_value": "a3f2b8c1d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1",
    "signed_by": "platform-evidence-signer (X.509 cert CN=evidence.iam.hdps.com.np)",
    "stored_in": "s3://hdps-grc-evidence/2082/Q1/RULE-001/EVD-2082-001-00042.json"
  }
}
```

**Sample 2: Quarterly Access Review Campaign Summary (Excerpt)**

```json
{
  "report_id": "RPT-QAR-2082-Q1",
  "report_type": "Quarterly Access Review Summary",
  "period": "FY2082 Q1 (Shrawan-Ashoj 2082)",
  "generated_at": "2082-07-28T09:00:00+05:45",
  "campaign_metrics": {
    "campaign_initiated": "2082-04-02T09:00:00+05:45",
    "campaign_completed": "2082-04-09T16:30:00+05:45",
    "total_calendar_days": 7,
    "total_users_reviewed": 487,
    "total_systems_in_scope": 9,
    "completion_rate": "100%",
    "decisions": {
      "retain": 451,
      "modify": 18,
      "revoke": 12,
      "new_exceptions_granted": 6
    },
    "manager_response_metrics": {
      "total_managers": 12,
      "responded_within_sla": 10,
      "required_one_reminder": 2,
      "required_escalation": 0,
      "average_response_time_days": 4.2
    },
    "violations_detected": {
      "orphaned_accounts": 3,
      "dormant_accounts": 8,
      "sod_violations": 1,
      "excessive_privileges": 2
    },
    "nrb_submission": {
      "submitted_on": "2082-04-10T10:00:00+05:45",
      "deadline": "2082-04-15",
      "status": "On-time"
    }
  }
}
```

---

## Approval

| Role | Name | Date | Signature |
|------|------|------|-----------|
| GRC Lead / CISO | Anita Gurung | 2025-07-13 | _________________ |
| Engineering Lead | Arun Thapa | 2025-07-13 | _________________ |
| Compliance / GRC Analyst | Priya Maharjan | 2025-07-13 | _________________ |
| IT Manager | Dipak KC | 2025-07-13 | _________________ |
| CEO (Final Approval) | Ramesh Pandey | 2025-07-13 | _________________ |
```
