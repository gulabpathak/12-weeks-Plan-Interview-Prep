# Azure Administrator Labs (2025) — README Collection
Comprehensive README.md files for all 10 Azure labs. Each section below represents the contents of the README.md that can be placed into its respective lab folder.

---

# 1. Azure AD User & Group Management — README.md

## Overview
This lab provides hands-on practice with Azure Active Directory (Entra ID) user lifecycle management, including user creation, bulk import, dynamic groups, MFA enforcement, and identity governance configuration. These are essential skills for Azure Administrator and Cloud Support roles.

## Architecture Diagram
```
[ Admin ] → [ Azure AD / Entra ID ] → [ Users | Groups | Policies ]
```

## Learning Objectives
- Add custom domain to Azure AD
- Create and manage cloud-only users
- Perform bulk creation of users using CSV + PowerShell
- Create static and dynamic groups
- Assign users to groups automatically via rules
- Enforce MFA using Conditional Access
- Export MFA registration and sign-in logs

---

## Prerequisites
- Azure subscription
- Global Administrator / User Administrator role
- PowerShell 7+ with AzureAD / Microsoft Graph modules
- CSV file for bulk user import

---

## Step-by-Step Implementation

### **1. Add a Custom Domain (Optional)**
1. Go to **Azure AD → Custom domain names**
2. Add domain (e.g., `contoso.in`)
3. Add TXT record in DNS
4. Complete domain verification

---

### **2. Create Users Manually**
Portal:
1. Azure AD → Users → *New User*
2. Provide name, UPN, password
3. Assign group memberships if needed

PowerShell:
```powershell
Connect-AzureAD
New-AzureADUser -DisplayName "Test User1" -UserPrincipalName "test1@contoso.in" -AccountEnabled $true -PasswordProfile @{Password="Password@123"}
```

---

### **3. Bulk User Creation via CSV**
Sample CSV (`bulk-users.csv`):
```
DisplayName,UserPrincipalName,Password
Alice Smith,alice@contoso.in,Password@123
Bob Kumar,bob@contoso.in,Password@123
```

PowerShell import:
```powershell
$users = Import-Csv "bulk-users.csv"
foreach ($u in $users) {
    New-AzureADUser -DisplayName $u.DisplayName -UserPrincipalName $u.UserPrincipalName -AccountEnabled $true -PasswordProfile @{Password=$u.Password}
}
```

---

### **4. Create Static & Dynamic Groups**
#### Static Group
Portal → Groups → New Group → *Security Group* → Add members.

#### Dynamic Group
Dynamic rule example (Department = Sales):
```
(user.department -eq "Sales")
```
Azure Portal → Groups → New Group → Enable Dynamic Membership → Add rule.

---

### **5. Configure Conditional Access for MFA**
1. Azure AD → Security → Conditional Access
2. Create new policy:
   - Assign to users
   - Cloud apps: All
   - Grant: Require MFA
   - Enable policy

---

### **6. Export MFA Status and Sign-in Logs**
Using Graph PowerShell:
```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
Connect-MgGraph -Scopes "AuditLog.Read.All","Directory.Read.All"
Get-MgUserAuthenticationMethod -UserId user@contoso.in
```

To export all sign-ins:
```powershell
Get-MgAuditLogSignIn | Export-Csv signins.csv -NoTypeInformation
```

---

## Validation
- Users appear in Azure AD
- Dynamic group auto-populates
- Sign-in attempts require MFA
- CSV-created users appear correctly

---

## Troubleshooting
- **Dynamic group not updating** → Wait 1–2 minutes or validate rule syntax
- **MFA not triggered** → Confirm CA policy assignment
- **Bulk import fails** → Validate CSV headers and UPN format

---

## Cleanup
- Delete test users: Portal or PowerShell
- Remove Conditional Access policy
- Delete dynamic groups

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
This lab walks through deploying a **high-availability web server environment** using Azure Virtual Machines behind a **Standard Load Balancer**. This lab mirrors real-world production setups for availability, resiliency, and traffic distribution.

## Architecture Diagram
```
            +---------------------------+
            |     Azure Load Balancer   |
            |  (Frontend + Health Probe)|
            +--------------+------------+
                           |
                  +--------+--------+
                  |                 |
           [ VM1 - IIS ]     [ VM2 - IIS ]
```

## Learning Objectives
- Deploy two Windows Server VMs
- Install IIS and configure custom pages
- Deploy Azure Standard Load Balancer
- Configure backend pools & health probes
- Validate session distribution across VMs

---

## Prerequisites
- Azure subscription
- Resource Group
- Basic networking knowledge

---

## Step-by-Step Implementation

### **1. Deploy VNet & Subnet (Optional)**
Azure CLI:
```bash
az network vnet create \
  --name vnet-ha \
  --resource-group rg-ha \
  --address-prefix 10.0.0.0/16 \
  --subnet-name websubnet \
  --subnet-prefix 10.0.1.0/24
```

---

### **2. Deploy Two Windows Server VMs**
Use the same subnet.

Azure CLI:
```bash
az vm create -n vm1 -g rg-ha --image Win2022AzureEdition --admin-username azureuser --admin-password "Password@123" --vnet-name vnet-ha --subnet websubnet

az vm create -n vm2 -g rg-ha --image Win2022AzureEdition --admin-username azureuser --admin-password "Password@123" --vnet-name vnet-ha --subnet websubnet
```

---

### **3. Install IIS on Both VMs**
Using Custom Script Extension:
```powershell
Install-WindowsFeature -Name Web-Server
Set-Content -Path "C:\\inetpub\\wwwroot\\index.html" -Value "<h1>Server: VM1</h1>"
```
Repeat with VM2 (change identifier).

Bash example using extension:
```bash
az vm extension set \
  --publisher Microsoft.Compute \
  --version 1.10 \
  --name CustomScriptExtension \
  --vm-name vm1 \
  --resource-group rg-ha \
  --settings '{"commandToExecute":"powershell Install-WindowsFeature Web-Server"}'
```

---

### **4. Create Azure Standard Load Balancer**
```bash
az network lb create \
  --resource-group rg-ha \
  --name lb-ha \
  --sku Standard \
  --frontend-ip-name lb-frontend \
  --backend-pool-name lb-bepool
```

---

### **5. Add VMs to Backend Pool**
```bash
az network lb address-pool address add \
  --lb-name lb-ha \
  --pool-name lb-bepool \
  --resource-group rg-ha \
  --vnet vnet-ha \
  --subnet websubnet \
  --ip-address 10.0.1.4
```
Repeat for VM2.

---

### **6. Create Health Probe**
```bash
az network lb probe create \
  --resource-group rg-ha \
  --lb-name lb-ha \
  --name http-probe \
  --protocol tcp \
  --port 80
```

---

### **7. Create Load Balancer Rule**
```bash
az network lb rule create \
  --resource-group rg-ha \
  --lb-name lb-ha \
  --name http-rule \
  --protocol tcp \
  --frontend-port 80 \
  --backend-port 80 \
  --frontend-ip-name lb-frontend \
  --backend-pool-name lb-bepool \
  --probe-name http-probe
```

---

## Validation
### **Test via Browser**
Open the LB Public IP.
- Refresh multiple times → observe content switching between **VM1** and **VM2**.

### **Curl Test**
```bash
curl http://<LB_Public_IP>
```
Expect alternating server responses.

---

## Troubleshooting
- **IIS not loading** → Ensure port 80 allowed in NSG
- **LB not distributing traffic** → Check health probe status
- **VM not appearing in backend** → Confirm NIC association

---

## Cleanup
```bash
az group delete -n rg-ha --yes --no-wait
```
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

