# Cyberinfiniti-Enterprise-Cloud-Security-Using-Microsoft-Azure
This repository provides an Enterprise Cloud Security Assessment within a Microsoft Azure production environment. It covers identity governance, access control, monitoring, detection engineering, threat protection, and incident response, leveraging Azure-native security services.

---
# Project Overview
This repository provides documentation of an Enterprise Cloud Security Assessment performed in a Microsoft Azure production environment. The assessment examines identity governance, access management, monitoring, detection engineering, threat defense, and incident response readiness, leveraging Azure-native security services.
The project aligns technical controls with:
• 	Zero Trust Architecture
• 	Least Privilege Enforcement
• 	Defense-in-Depth Strategy
• 	SOC Modernization Principles

---
# 📑 Table Of Contents
Executive Summary
Scenario & Objectives
Scope of Assessment
Azure Environment Setup
Methodology & Tooling
Lab 01 – RBAC Implementation
Lab 02 – NSG & ASG Segmentation
Lab 03 – Azure Firewall
Lab 04 – ACR & AKS Security
Lab 05 – Service Endpoints & Storage Security
Lab 06 – Log Analytics & DCR
Lab 07 – Microsoft Defender for Cloud
Lab 08 – Just-In-Time VM Access
Lab 09 – Microsoft Sentinel (SIEM & SOAR)
Key Findings
Strategic Recommendations
Conclusion

---
# Executive Summary
This assessment demonstrates how Microsoft Azure security services can be configured and operationalized to defend enterprise cloud workloads.
The environment was evaluated across:
Identity & Access Management Network Segmentation Cloud Workload Protection Monitoring & Logging Privileged Access Management SIEM & Automated Response Results show significant reduction in attack surface and improved detection maturity through Azure-native integration.

---
# Scenario & Objectives
Cyberinfiniti Ltd engaged this assessment to validate its cloud security posture.
Primary Objectives
• 	Enforce least‑privilege role‑based access control (RBAC)
• 	Establish centralized logging and monitoring
• 	Identify misconfigurations and potential threats
• 	Minimize exposure of management ports
• 	Deploy an enterprise‑grade SIEM solution
• 	Deliver executive‑level risk insights

---
# Scope of Assessment
The assessment encompassed the deployment and evaluation of the following Azure services:
• 	Microsoft Entra ID
• 	Azure Role-Based Access Control (RBAC)
• 	Azure Monitor & Log Analytics
• 	Microsoft Defender for Cloud (Plan 2)
• 	Azure Firewall
• 	Azure Container Registry (ACR)
• 	Azure Kubernetes Service (AKS)
• 	Microsoft Sentinel
• 	Azure Logic Apps (SOAR)
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
