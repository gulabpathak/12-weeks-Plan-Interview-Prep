📘 PROJECT-SUMMARY.md
Hybrid Identity & IAM Architecture using Microsoft Entra ID (Azure AD)

Designed & Implemented by: Gulab Pathak
🔗 Portfolio: https://gulabpathak.github.io/Me/

🔗 LinkedIn: https://www.linkedin.com/in/gulab-pathak-8a5a6511b

📌 Executive Summary

This project demonstrates the design and implementation of a secure Hybrid Identity architecture using Microsoft Entra ID (Azure AD) integrated with on-prem Active Directory.
It follows Zero Trust principles and focuses on modern authentication, Conditional Access, Identity Protection, Privileged Identity Management, and SAML-based SSO for enterprise applications.

The environment simulates what a real organization would deploy while transitioning from traditional on-premises identity to a cloud-first IAM model.

This summary highlights the goals, architecture, and measurable outcomes of the project.

🎯 Project Goals
1. Establish modern cloud identity with secure authentication
2. Integrate on-prem AD DS with Microsoft Entra ID
3. Protect identities using MFA, Conditional Access & Identity Protection
4. Build Zero Trust–aligned access control policies
5. Implement Privileged Identity Management (PIM) to secure admin roles
6. Enable SAML-based Single Sign-On (SSO) for SaaS apps
7. Deploy lifecycle governance & audit logging for compliance
🏛️ Architecture Overview
Identity Components

On-prem Active Directory (AD DS)

Azure AD Connect (Password Hash Sync + Seamless SSO)

Microsoft Entra ID (Cloud Identity Provider)

Security Layers

Multi-Factor Authentication (MFA)

Conditional Access policies

Identity Protection (risk-based sign-in policies)

Privileged Identity Management (PIM)

Applications & Devices

Azure AD-joined and Hybrid AADJ devices

SAML-based Enterprise Application

OAuth2/OIDC applications (optional extension)

Governance

Access Packages

Automated license assignment

Audit logs exported to Log Analytics

🔧 Implementation Phases
🔹 Phase 1 — Hybrid Identity Setup

Installed AD DS domain (gulab.lab)

Deployed Azure AD Connect (PHS + SSO)

Configured OU filtering and sync validation

🔹 Phase 2 — Device Identity & Compliance

Onboarded Azure AD Join (AADJ) and Hybrid AADJ devices

Evaluated compliance via Intune trial

🔹 Phase 3 — Conditional Access + MFA

Enforced MFA for all users

Blocked legacy authentication

Required compliant devices for admin roles

Tested using CA “What If” tool and sign-in logs

🔹 Phase 4 — Identity Protection

Enabled Sign-in Risk & User Risk policies

Simulated risky sign-ins (TOR, impossible travel)

Verified automated actions (MFA/Password reset)

🔹 Phase 5 — Privileged Identity Management (PIM)

Enabled JIT-based access for critical roles

Configured MFA for role activation

Set approval workflows & access reviews

🔹 Phase 6 — SAML-based SSO Integration

Created Enterprise App for SSO

Configured SAML settings (Entity ID, Reply URL)

Mapped SAML claims (UPN + department)

Validated SSO via logs and SAML tracer

🔹 Phase 7 — Governance & Auditing

Implemented Access Packages for contractors

Automated license assignments using groups

Exported logs to Log Analytics (optional)

📈 Key Outcomes
✔ Full hybrid identity integration (AD DS ↔ Azure AD)
✔ Secure authentication using MFA + Conditional Access
✔ Risk-based identity protection configured
✔ Admin privileges protected with PIM
✔ Enterprise-grade SAML SSO deployed
✔ Devices enrolled with compliance evaluation
✔ Governance workflows established
✔ Audit-ready identity & access environment
🧠 Skills Demonstrated
Identity & Access Management

Azure AD / Microsoft Entra

Conditional Access

MFA (Authenticator, FIDO2)

Identity Protection

Hybrid Identity & Directory Services

Active Directory

Azure AD Connect

Password Hash Sync

Device trust models

Privileged Access

PIM

Least privilege

JIT access

Authentication Technologies

SAML 2.0

OAuth2 / OpenID Connect

SCIM provisioning (optional)

Governance & Automation

Access packages

Group-based licensing

Log Analytics integration

📞 Contact

Gulab Pathak
🔗 LinkedIn: https://www.linkedin.com/in/gulab-pathak-8a5a6511b

🌐 Portfolio: https://gulabpathak.github.io/Me/

📧 Email: (add your email here)