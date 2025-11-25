⭐ Azure Infrastructure Monitoring, Networking & Security Hardening – Enterprise Architecture (2025)
Designed & Implemented by: Gulab Pathak

🔗 Portfolio: https://gulabpathak.github.io/Me/

🔗 LinkedIn: https://www.linkedin.com/in/gulab-pathak-8a5a6511b

📌 Project Summary

This project implements a secure, highly available, and fully monitored Azure infrastructure following enterprise-grade practices. It includes:

Multi-tier Azure VNet design

NSG segmentation + Azure Firewall

WAF-enabled Application Gateway

VM Scale Set with autoscaling + Load Balancer

End-to-end monitoring with Azure Monitor + Log Analytics

KQL analytics + alerts

Azure Policy governance + RBAC model

Defender for Cloud security posture

Key Vault–based secrets & certificate protection

It demonstrates real-world skills needed for Azure Administrator, Cloud Infra Engineer, Cloud Security Engineer, and Cloud Support roles.

🏗️ High-Level Architecture Diagram (Text)
Azure Resource Group: RG-Prod
 ├── Virtual Network (10.0.0.0/16)
 │     ├── Subnet-Web (WAF + AG)
 │     ├── Subnet-App (VM Scale Set + LB)
 │     ├── Subnet-AzureFirewall
 │     └── Subnet-Management (Bastion)
 │
 ├── NSGs (Tier Segmentation)
 │     ├── Web: Allow 80/443
 │     └── App: Allow only from WebSubnet
 │
 ├── Application Gateway (WAF)
 ├── Azure Firewall (Outbound filtering)
 ├── Load Balancer + VM Scale Set (Autoscaling)
 ├── Log Analytics Workspace
 ├── Azure Monitor Alerts & Workbooks
 ├── Azure Policy + RBAC
 ├── Defender for Cloud
 └── Azure Key Vault

🎯 Project Goals
✔ Design a Production-Style Azure Network
✔ Implement end-to-end monitoring + alerting
✔ Apply Zero Trust network segmentation
✔ Harden infrastructure with WAF, Firewall & NSGs
✔ Automate governance using Azure Policy
✔ Implement secret protection with Key Vault
✔ Improve Secure Score in Defender for Cloud
🔧 Implementation Breakdown
🔹 Phase 1 — Azure Networking Foundation
Tasks

Created VNet VNet-Prod (10.0.0.0/16)

Designed subnets:

WebSubnet

AppSubnet

AzureFirewallSubnet

MgmtSubnet

Applied NSGs:

NSG-Web → Allow HTTP/HTTPS only

NSG-App → Allow only WebSubnet traffic

MgmtSubnet → Bastion-only rules

Configured routing + diagnostics

🔹 Phase 2 — VM Scale Set + Load Balancer
Tasks

Deployed VMSS (Linux/Windows)

Configured autoscaling:

Scale-out: CPU > 70%

Scale-in: CPU < 30%

Configured Azure Load Balancer:

Health probe

Backend pool

NAT rules

Outputs

Autoscale event logs

LB health probe logs

🔹 Phase 3 — Application Gateway (WAF) + Azure Firewall
Application Gateway (WAF)

WAF mode: Prevention

Rules configured:

OWASP 3.2 defaults

Custom block rules

Configured HTTPS listener

Bound self-signed SSL certificate

Azure Firewall

Deployed to dedicated subnet

Configured:

App rules (repos, updates, APIs)

DNAT rules

Network rules

Logged all Firewall events to Log Analytics

🔹 Phase 4 — Monitoring, Log Analytics & Alerts
Monitoring Components

VM Insights

NSG Flow Logs → Storage → Log Analytics

Activity Logs export

Custom dashboards (Azure Monitor Workbooks)

Azure Alerts Configured

High CPU

Low disk

VMSS instance unhealthy

WAF blocked attacks

Firewall rule hits

Kusto Query Examples
🔎 Failed NSG Flows
AzureNetworkAnalytics_CL
| where FlowStatus_s == "D"

🔎 WAF Blocked Requests
AzureDiagnostics
| where ResourceType == "APPLICATIONGATEWAYS"
| where action_s == "Blocked"

🔎 Average CPU (VMss)
Perf
| where CounterName == "% Processor Time"
| summarize AvgCPU = avg(CounterValue) by bin(TimeGenerated, 5m)

🔹 Phase 5 — Security Hardening (Azure Policy + RBAC)
Azure Policies Applied

Enforce HTTPS for App Gateway

Require diagnostic logs on all resources

Deny public IP creation (except approved)

Audit insecure NSG rules

Enforce encryption at rest on Storage

Require VM backup policy

RBAC Model
Role	Scope	Purpose
Reader	RG	Monitoring
Network Contributor	VNet	Manage NSGs, routing
VM Contributor	RG	Compute ops
Key Vault Secrets User	KV	Retrieve secrets
Security Admin	Subscription	Governance
🔹 Phase 6 — Defender for Cloud Integration
Configured

Defender for Servers

Defender for App Service

JIT VM Access

Endpoint Protection

Secure Score tracking

Threat detection alerts

Observed Alerts (Lab Simulated)

Port scan attempts

RDP brute-force (Windows VM)

Suspicious outbound traffic

Malware signature detection

🔹 Phase 7 — Key Vault + Secrets Management
Tasks

Created Azure Key Vault

Secured:

VM Admin Credentials

SSL Certificate for App Gateway

Application configuration secrets

Enabled:

Firewall restrictions

Private endpoint

RBAC-based access

🧠 Key Skills Demonstrated
Azure Networking

VNets, subnets

NSGs (zero trust segmentation)

Azure Firewall

Application Gateway (WAF)

Bastion

Monitoring & Analytics

Log Analytics

Azure Monitor Alerts

Workbooks

KQL query design

Security & Governance

Azure Policy

Defender for Cloud

RBAC

Secure Score improvement

JIT Access

Key Vault + Private Endpoints

Compute & Load Balancing

VM Scale Sets

Autoscaling

Load Balancer

Custom Script Extensions

📄 Resume Highlights (Copy-Paste)

Designed and deployed a multi-tier Azure network with NSGs, Azure Firewall, Application Gateway (WAF), Bastion, and private endpoints.

Implemented centralized monitoring using Log Analytics, Azure Monitor, NSG flow logs & custom KQL dashboards.

Built a scalable app layer using VM Scale Sets with autoscaling and Load Balancer health probes.

Enforced security governance using Azure Policies and improved Secure Score with Defender for Cloud.

Implemented Key Vault–based secret and certificate protection with RBAC + firewall lockdown.

📁 Repository Structure
Azure-Infra-Monitoring-Security/
│
├── architecture-diagrams/
│   ├── azure-network-architecture.png
│   ├── monitoring-flow.png
│   └── waf-firewall-flow.png
│
├── labs/
│   ├── 01-Network-Foundation.md
│   ├── 02-VMSS-LoadBalancer.md
│   ├── 03-AppGateway-WAF.md
│   ├── 04-AzureFirewall.md
│   ├── 05-Monitoring-LogAnalytics.md
│   ├── 06-AzurePolicy-Governance.md
│   └── 07-DefenderForCloud.md
│
├── queries/
│   ├── nsg-deny-logs.kql
│   ├── waf-blocked-requests.kql
│   └── vmss-cpu-alert.kql
│
├── scripts/
│   ├── deploy-nsgs.ps1
│   ├── diag-settings.ps1
│   └── vmss-autoscale.ps1
│
└── README.md

📞 Connect With Me

If you're hiring for Azure Admin, Cloud Infra, Networking, or Cloud Security roles, I'm happy to walk through this project live.

🔗 LinkedIn:
https://www.linkedin.com/in/gulab-pathak-8a5a6511b

🌐 Portfolio:
https://gulabpathak.github.io/Me/

📧 Email: (add your email)