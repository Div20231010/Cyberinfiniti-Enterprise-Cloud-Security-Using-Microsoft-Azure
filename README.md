# Cyberinfiniti-Enterprise-Cloud-Security-Using-Microsoft-Azure
This repository provides an Enterprise Cloud Security Assessment within a Microsoft Azure production environment. It covers identity governance, access control, monitoring, detection engineering, threat protection, and incident response, leveraging Azure-native security services.

---
# Project Overview
## Enterprise Cloud Security Assessment
This repository documents an Enterprise Cloud Security Assessment conducted in a Microsoft Azure production environment. The assessment evaluated identity governance, access management, monitoring, detection engineering, threat defense, and incident response readiness, leveraging Azure-native security services.
The project aligns technical controls with:
- Zero Trust Architecture
- Least Privilege Enforcement
- Defense-in-Depth Strategy
- SOC Modernization Principles

---
# 📑 Table Of Contents
[Executive Summary](#executive-summary)  
[Scenario & Objectives](#scenario--objectives)  
[Scope of Assessment](#scope-of-assessment)  
[Azure Environment Setup](#azure-environment-setup)  
[Methodology & Tooling](#methodology--tooling)  
[Lab 01 – RBAC Implementation](#lab-01--role-based-access-control)  
[Lab 02 – NSG & ASG Segmentation](#lab-02--nsg--asg-segmentation)  
[Lab 03 – Azure Firewall](#lab-03--azure-firewall)  
[Lab 04 – ACR & AKS Security](#lab-04--acr--aks-security)  
[Lab 05 – Service Endpoints & Storage Security](#lab-05--service-endpoints--storage-security)  
[Lab 06 – Log Analytics & DCR](#lab-06--log-analytics--data-collection-rules)  
[Lab 07 – Microsoft Defender for Cloud](#lab-07--microsoft-defender-for-cloud)  
[Lab 08 – Just-In-Time VM Access](#lab-08--just-in-time-vm-access)  
[Lab 09 – Microsoft Sentinel (SIEM & SOAR)](#lab-09--microsoft-sentinel-siem--soar)  
[Key Findings](#key-findings)  
[Strategic Recommendations](#strategic-recommendations)  
[Conclusion](#conclusion)

---
# Executive Summary
This assessment demonstrates how Microsoft Azure security services can be configured and operationalized to defend enterprise cloud workloads.

The environment was evaluated across:
- Identity & Access Management
- Network Segmentation
- Cloud Workload Protection
- Monitoring & Logging
- Privileged Access Management
- SIEM & Automated Response

Results show significant reduction in attack surface and improved detection maturity through Azure-native integration.

---
# Scenario & Objectives

Cyberinfiniti Ltd engaged this assessment to validate its cloud security posture.

Primary Objectives:
- Enforce least‑privilege role‑based access control (RBAC)
- Establish centralized logging and monitoring
- Identify misconfigurations and potential threats
- Minimize exposure of management ports
- Deploy an enterprise‑grade SIEM solution
- Deliver executive‑level risk insights

---
# Scope of Assessment

The assessment encompassed the deployment and evaluation of key Azure security services:

- Microsoft Entra ID
- Azure Role-Based Access Control (RBAC)
- Azure Monitor & Log Analytics
- Microsoft Defender for Cloud (Plan 2)
- Azure Firewall
- Azure Container Registry (ACR)
- Azure Kubernetes Service (AKS)
- Microsoft Sentinel
- Azure Logic Apps (SOAR)

All resources were provisioned in the East US region within segmented resource groups under the TEAM8 environment.

---
# Azure Environment Setup
### Subscription Model
- Azure Pay‑As‑You‑Go plan
- Role assignments for Owner and Contributor
- Dedicated Microsoft Entra ID tenant
### Design Principles
- Enforcement of least‑privilege access
- Group‑based RBAC for streamlined management
- Segmentation through resource groups
- Adoption of the Azure‑native security stack

---
# Lab 01 – Role-Based Access Control (RBAC)
### 🎯 Objective
Implement least-privilege access using group-based role assignments.

### 🔹 Create Security Group (Portal)
Navigate to Microsoft Entra ID
Create User
Create Security Group
Add user to group

### 🔹 PowerShell Example
# PowerShell
New-AzADUser -DisplayName "Isabel Garcia" `
             -UserPrincipalName "isabel@domain.com" `
             -AccountEnabled $true

New-AzADGroup -DisplayName "Junior Admins" `
              -MailNickname "JuniorAdmins"

### 🔹 Assign VM Contributor Role (Azure CLI)
az role assignment create \
  --assignee <GroupObjectID> \
  --role "Virtual Machine Contributor" \
  --resource-group TEAM8

## Validation

The assessment included validation of role assignments and enforcement of least-privilege principles to ensure secure access controls.

### Key validation steps:
- Confirm role assignment under Access Control (IAM)
- Validate least-privilege enforcement
- Ensure users cannot escalate permissions

---
# Lab 02 – Network Security Groups & Application Security Groups
### Objective
Segment workloads using NSGs and ASGs to enforce granular traffic control.

### Steps Implemented:
- Create Virtual Network and Subnet
- Create Application Security Groups:
  - WebServers
  - ManagementServers
- Deploy Network Security Group (NSG)
- Associate NSG with Subnet
- Configure Security Rules:
  - Allow RDP (3389) → ManagementServers
  - Deny RDP → WebServers
  - Allow HTTP (80) → WebServers
- Deploy two virtual machines
- Associate NICs with respective ASGs

### Validation:
- RDP accessible only to Management Server
- Web server accessible via HTTP
- Direct RDP to Web Server blocked

---
# Lab 03 – Azure Firewall
### Objective
Implement centralized outbound traffic inspection and filtering.

### Deployment Steps
Deploy Azure Firewall
Create Public IP
Configure Firewall Subnet (AzureFirewallSubnet)
Create Route Table
Force default route (0.0.0.0/0) through Firewall

#### Application Rule Example
- Allow: www.bing.com
- Protocol: HTTPS
- Source: Internal subnet

### Network Rule Example
- Allow DNS (Port 53)
- Protocol: UDP/TCP

### Validation
- Only approved FQDN accessible
- All other outbound traffic denied
- DNS resolution functioning

---
 # Lab 04 – Azure Container Registry & AKS Security
 ### Objective
 Secure container registry and Kubernetes workloads.
 ### 🔹 Create Azure Container Registry
```bash
az acr create \
  --resource-group TEAM8 \
  --name team8acr \
  --sku Standard 
  ```

### Create AKS Cluster with Managed Identity
```bash
az aks create \
  --resource-group TEAM8 \
  --name team8aks \
  --node-count 2 \
  --enable-managed-identity
```

### 🔹 Attach ACR to AKS
```bash
az aks update \
  --name team8aks \
  --resource-group TEAM8 \
  --attach-acr team8acr
```
### Deploy Application
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

# Security Enhancements
- Managed Identity enforced
- Registry access restricted
- Controlled external exposure
- Kubernetes service validation

---

# Lab 05 – Service Endpoints & Storage Security
### Objective
Restrict storage account access to specific Azure subnets.

### Steps Implemented
- Create Virtual Network with two subnets
- Enable Microsoft.Storage Service Endpoint
- Deploy Storage Account
- Restrict network access to selected subnet
- Deploy test VMs in both subnets

# Validation
- Private subnet: Access Allowed
- Public subnet: Access Denied
- Public network access disabled

---

# Lab 06 – Log Analytics & Data Collection Rules
 ### Objective
Centralize telemetry collection and enable log analysis.

### 🔹 Deployment Process
- Deploy Virtual Machine
- Create Log Analytics Workspace
- Install Azure Monitor Agent
- Create Data Collection Rule (DCR)
- Associate DCR with VM

### Sample KQL Query
```bash
SecurityEvent
| where EventLevelName == "Information"
| limit 10
```

### Validation

- Logs successfully captured and visible in the workspace
- Agent connection established and verified
- Telemetry centralized for unified monitoring

---
# Lab 07 – Microsoft Defender for Cloud
## Objective
Strengthen protection for advanced cloud workloads.

### Configuration
- Activate Defender for Servers (Plan 2)
- Review and assess Secure Score
- Enable vulnerability scanning
- Verify endpoint protection coverage

### Outcomes
- Real-time threat detection operational
- Security recommendations generated
- Behavioral analytics in effect

---
# Lab 08 – Just-In-Time (JIT) VM Access
### Objective
Minimize attack surface by restricting access to management ports.

### Configuration Steps
- Open Defender for Cloud
- Select the target virtual machine
- Enable Just-In-Time (JIT) access
- Configure parameters:
  - RDP (3389)
  - Access duration (1–3 hours)
  - Approved IP ranges

### Validation
- RDP access closed by default
- Access granted only upon authorized request
- Ports automatically closed after the defined expiration

---
# Lab 09 – Microsoft Sentinel (SIEM & SOAR)
### Objective
Deploy centralized SIEM with automated response capabilities.

# 🔹 Deployment Steps
- Create Log Analytics Workspace
- Enable Microsoft Sentinel
- Connect Azure Activity Logs
- Create Analytics Rule
- Deploy Logic App Playbook

### Sample Detection Query
```bash
AzureActivity
| where OperationNameValue contains "Delete"
| summarize count() by Caller
```

### Automation
- Alerts sent to SOC team
- Incident triggered on suspicious activity
- Playbook executes automated response
  
# Validation
- SOC visibility confirmed
- Playbook executed
- Incident generated successfully

  # Key Findings
- Robust IAM governance enforced using RBAC
- Effective segmentation strengthened network security
- Centralized monitoring enhanced visibility
- Defender Plan 2 improved detection capabilities
- Just-In-Time access significantly reduced exposed attack surface
- Sentinel enabled automated SOC workflows

 # Strategic Recommendations
- Expand automation through Sentinel playbooks
- Enforce governance via policy controls
- Conduct regular RBAC access reviews
- Maintain a centralized logging framework
- Continue developing cloud security expertise

  ---
  # Conclusion
This project demonstrates a scalable, enterprise-grade Azure cloud security architecture aligned with Zero Trust principles.

### By integrating:

- Microsoft Entra ID
- Azure RBAC
- Defender for Cloud
- Azure Monitor
- Microsoft Sentinel
- Cyberinfiniti Ltd achieved a defensible, monitored, and resilient cloud security posture suitable for modern enterprise operations.

Prepared by: Divine Junior Ezewele , (Cyber Security Analyst TEAM 8 CYBLACK)

  
