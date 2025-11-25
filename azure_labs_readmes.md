# Azure Administrator Labs (2025) — README Collection
Comprehensive README.md files for all 10 Azure labs. Each section below represents the contents of the README.md that can be placed into its respective lab folder.

---

# 1. Azure AD User & Group Management — README.md

## Overview
This lab focuses on Azure Active Directory (Entra ID) user lifecycle operations such as user creation, bulk import, dynamic groups, MFA enforcement, and basic identity governance.

## Architecture
- Azure AD Tenant
- Users & Groups
- Conditional Access Policies
- PowerShell Automation

## Prerequisites
- Azure subscription
- Azure AD administrator permissions
- PowerShell 7+

## Steps
1. Add a custom domain.
2. Create users manually.
3. Perform bulk user creation with CSV.
4. Create static and dynamic security groups.
5. Configure Conditional Access to enforce MFA.
6. Export MFA status via PowerShell.

## Validation
- Users appear as expected.
- Dynamic groups auto-populate.
- MFA enforced during login.

## Cleanup
- Remove test users
- Delete Conditional Access policies

---

# 2. Azure VM Deployment & Hardening — README.md

## Overview
Deploy a Windows Server VM using Bicep, configure NSGs, secure access using Bastion, and perform hardening including IIS installation.

## Architecture
- VNet + Subnet
- Windows Server 2022 VM
- NSG
- Azure Bastion

## Steps
1. Deploy VM via Bicep.
2. Configure NSG rules.
3. Deploy Azure Bastion.
4. Install IIS using Custom Script Extension.
5. Enable Azure Disk Encryption.

## Validation
- VM reachable via Bastion.
- IIS webpage loads.
- RDP blocked publicly.

## Cleanup
- Delete resource group.

---

# 3. Azure Storage Account — Static Website — README.md

## Overview
This lab demonstrates how to deploy a **static website** using an **Azure Storage Account**, configure **Static Website Hosting**, enable **Azure CDN**, and set up diagnostic logging for performance and access tracking.

This scenario aligns with real-world Azure Administrator tasks such as secure hosting, cost‑optimized website delivery, and basic DevOps website publishing.

## Architecture Diagram
```
[ Client Browser ] → [ Azure CDN Endpoint ] → [ Azure Storage Static Website ]
```

## Learning Objectives
- Create an Azure Storage Account (General Purpose v2)
- Enable static website hosting
- Upload web content (HTML/CSS/images)
- Configure Azure CDN for global caching
- Enable logging & diagnostics
- Validate content availability via CDN URL

---

## Prerequisites
- Azure subscription
- Basic HTML/CSS files (sample included)
- Azure CLI or Azure Portal access

---

## Step-by-Step Implementation

### **1. Create a Storage Account**
Using Azure Portal:
1. Search → *Storage Accounts* → **Create**
2. Select:
   - Performance: Standard
   - Redundancy: **RA-GRS** or LRS
   - Enable public access (default)

Azure CLI option:
```bash
az storage account create \
  --name mystaticwebgulab \
  --resource-group rg-staticweb \
  --location centralindia \
  --sku Standard_LRS \
  --kind StorageV2
```

---

### **2. Enable Static Website Hosting**
1. Open the Storage Account
2. Navigate to **Static Website**
3. Enable the feature
4. Set:
   - Index document: `index.html`
   - Error document: `404.html`

Copy the **Primary Web Endpoint** (e.g., `https://<account>.z13.web.core.windows.net`).

---

### **3. Upload Website Content**
Upload files into the **$web** container:
- index.html
- styles.css
- images/

Azure CLI example:
```bash
az storage blob upload-batch \
  --destination '$web' \
  --account-name mystaticwebgulab \
  --source ./website
```

---

### **4. Configure Azure CDN**
1. Create CDN Profile → Standard Microsoft
2. Create CDN Endpoint:
   - Origin type: *Storage Static Website*
   - Origin hostname: Primary Web Endpoint

3. Wait 5–10 minutes for propagation.
4. Access CDN URL:
   - `https://<endpoint>.azureedge.net`

---

### **5. Enable Logs & Diagnostics**
Navigate to:
**Monitoring → Diagnostic Settings → Add diagnostic setting**

Enable:
- Blob logs
- Storage Read/Write/Delete logs
- Send to:
  - Log Analytics Workspace
  - Storage Account (optional)

---

## Validation
### Validate Static Site
Visit:
- Static Website URL
- CDN Endpoint URL

You should see the deployed site load successfully.

### Validate CDN Caching
Run:
```bash
curl -I https://<endpoint>.azureedge.net
```
Look for:
- `X-Cache: TCP_HIT` → CDN successfully caching

---

## Troubleshooting
- **404 error** → Ensure index.html is inside **$web** container
- **CDN shows old version** → Purge CDN cache
- **Access Denied** → Static website hosting must be enabled

---

## Cleanup
To avoid charges:
```bash
az group delete --name rg-staticweb --yes --no-wait
```
---


# 4. Azure Backup & Recovery — README.md

## Overview
Protect Azure VMs using Recovery Services Vault and validate restore functionality.

## Architecture
- Recovery Services Vault
- Azure VM

## Steps
1. Create Recovery Services Vault.
2. Configure Backup policy.
3. Perform on-demand backup.
4. Restore to new VM.

## Validation
- Restore point created.
- Restored VM boots successfully.

## Cleanup
- Delete restored VMs.
- Remove backup items.

---

# 5. Hub-Spoke VNet Architecture — README.md

## Overview
Build secure hub-spoke topology with Bastion, enabling controlled connectivity.

## Architecture
- Hub VNet
- Spoke1 VNet
- Spoke2 VNet
- VNet Peering
- Bastion in Hub

## Steps
1. Create Hub + Spoke VNets.
2. Configure VNet peering.
3. Deploy Bastion.
4. Deploy VMs in Spokes.
5. Validate routing.

## Validation
- VM-to-VM connectivity.
- Internet access controlled.

## Cleanup
- Delete resource group.

---

# 6. Azure Monitor & Log Analytics — README.md

## Overview
Configure monitoring for Azure VMs using Log Analytics workspace and create alert rules.

## Architecture
- Log Analytics Workspace
- Azure Monitor
- VM Insights

## Steps
1. Create Log Analytics workspace.
2. Connect VM Insights.
3. Create alert (CPU > 80%).
4. Add Action Group.

## Validation
- Logs visible in workspace.
- Alerts triggered.

## Cleanup
- Remove alert rules.
- Delete workspace.

---

# 7. Azure Identity Protection — README.md

## Overview
Detect risky sign-ins and enforce MFA using Identity Protection policies.

## Architecture
- Identity Protection
- Conditional Access

## Steps
1. Review risky sign-ins.
2. Create Sign-in Risk policy.
3. Create User Risk policy.
4. Block legacy authentication.

## Validation
- Risk events appear.
- Policies enforce MFA.

## Cleanup
- Remove policies.

---

# 8. Azure App Service Deployment — README.md

## Overview
Deploy web app using Azure App Service and automate deployments using GitHub Actions.

## Architecture
- App Service Plan
- App Service
- Staging Deployment Slot
- GitHub Actions CI/CD

## Steps
1. Create App Service.
2. Connect GitHub repo.
3. Configure deployment pipeline.
4. Create staging slot.
5. Perform slot swap.

## Validation
- Pipeline succeeds.
- App runs in production slot.

## Cleanup
- Remove App Service resources.

---

# 9. Azure Load Balancer HA — README.md

## Overview
Deploy high-availability IIS servers behind Azure Load Balancer.

## Architecture
- 2× Windows VMs
- Standard Load Balancer
- Backend Pool
- Health Probes

## Steps
1. Deploy VMs.
2. Install IIS.
3. Deploy Standard Load Balancer.
4. Configure backend pool.
5. Create health probe.

## Validation
- Load distributed between VMs.

## Cleanup
- Delete resource group.

---

# 10. Azure Key Vault + SQL Integration — README.md

## Overview
Use Key Vault to secure SQL DB credentials and access them via Managed Identity.

## Architecture
- Azure SQL Database
- Key Vault
- VM/App with Managed Identity

## Steps
1. Deploy SQL Database.
2. Store connection string in Key Vault.
3. Enable Managed Identity.
4. Retrieve secrets programmatically.

## Validation
- Application connects without storing secrets.

## Cleanup
- Delete SQL DB.
- Delete Key Vault.

---

# End of README Collection

