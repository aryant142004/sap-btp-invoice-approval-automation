# sap-btp-invoice-approval-automation

# 🏢 Enterprise Accounts Payable Governance & Automated Invoice Approval Workflow

[![SAP Build Process Automation](https://img.shields.io/badge/SAP%20Build-Process%20Automation-0A6ED1?style=for-the-badge&logo=sap)](https://www.sap.com/)
[![SAP BTP](https://img.shields.io/badge/SAP%20BTP-Subaccount%20Services-008FD3?style=for-the-badge&logo=sap)](https://account.hanatrial.ondemand.com/)
[![Clean Core Extension](https://img.shields.io/badge/Architecture-Clean%20Core%20Extension-2EA44F?style=for-the-badge)](https://www.sap.com/products/erp/clean-core.html)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

## 📌 Business Problem & Objective

In traditional enterprise operational models, manual Accounts Payable (AP) processing creates significant financial bottlenecks. Unstructured email chains, manual data-entry handoffs, and delayed manager reviews result in:
* **Extended Cycle Times:** Invoices take days or weeks to move from intake to approval.
* **Human Errors:** Manual data copying leads to misplaced values, incorrect routing, and compliance gaps.
* **Lack of Transparency:** No audit trail or centralized visibility into running invoice approvals.

### 📜 The Solution
This project delivers a side-by-side automated invoice governance and approval workflow engineered on **SAP Business Technology Platform (SAP BTP)** using **SAP Build Process Automation**. 

By orchestrating the workflow outside the core ERP system, this architecture preserves SAP’s **Clean Core Strategy** while digitizing decision pathways, rendering dynamic reviewer forms inside **SAP My Inbox**, and dispatching real-time SMTP notifications.

---

## 🏗️ Architecture & Technical Workflow 

[ Vendor / AP Clerk ]│▼┌─────────────────────────────────────────────────────────────┐│ Trigger: VenderInvoiceInputForm                             ││ (Vendor Name, Invoice #, Amount, Department, Email)         │└───────────────────────────┬─────────────────────────────────┘│▼┌─────────────────────────────────────────────────────────────┐│ Orchestration: Invoice_Approval_Process                     ││ ├── BTP Context Mapping                                     ││ └── Threshold & Branching Logic (Approve / Reject / End)    │└───────────────────────────┬─────────────────────────────────┘│┌─────────────┴─────────────┐▼                           ▼┌───────────────────────────┐ ┌───────────────────────────┐│ User Task:                │ │ Notification Engine:      ││ ManagerApprovalTask UI    │ │ BTP Destination           ││ (Rendered in SAP My Inbox)│ │ sap_process_automation_mail│└───────────────────────────┘ └─────────────┬─────────────┘│ (SMTP / STARTTLS)▼┌───────────────────────────┐│ Vendor / Stakeholder Email││ (Decision Notification)   │└───────────────────────────┘
### Key Technical Capabilities
1. **Dynamic Form Intake (`VenderInvoiceInputForm`):** Serves as the initial entry point, gathering structured invoice metadata and spawning process instances.
2. **Contextual Task UI (`ManagerApprovalTask`):** Explicitly maps incoming workflow payload variables directly into a custom task UI, preventing blank reviewer panes in **SAP My Inbox**.
3. **Deterministic Condition Paths:** Ensures all conditional evaluation branches (Approved, Rejected, Default) terminate at valid activities or end states without dangling process steps.
4. **Secure BTP Destination Infrastructure (`sap_process_automation_mail`):** Encapsulates connection configurations (STARTTLS/TLS encryption, port `587`, SMTP basic authentication) inside BTP Subaccount settings for secure mail delivery.
5. **Lifecycle Governance:** Full environment management covering artifact validation, version control releases (`v1.0.3`), deployment to target BTP runtimes, and execution tracking in SAP Process Monitoring.

---

## 🛠️ Tech Stack & Environment Specs

| Component | Technology / Protocol | Description |
| :--- | :--- | :--- |
| **PaaS Infrastructure** | SAP BTP Subaccount | Enterprise cloud runtime and service hosting |
| **Low-Code Automation** | SAP Build Process Automation | Workflow modeling, condition engine, and forms |
| **Client Interface** | SAP My Inbox / SAP Build Lobby | Centralized task management and workflow execution |
| **Integration Layer** | BTP Connectivity Destinations | Service endpoint and credential management |
| **Transport Security** | SMTP / STARTTLS (Port 587) | Encrypted automated email notifications |

---

🚀 Deployment & Configuration Guide

Step 1: Import Project PackageDownload the Enterprise_Invoice_Approval_v1.0.3.zip archive file from the src/ directory of this repository.Open your SAP Build Lobby.Click Import at the top right and upload the .zip archive.Open the project to verify that the VenderInvoiceInputForm, Invoice_Approval_Process, and ManagerApprovalTask artifacts are visible.

Step 2: Configure BTP Mail DestinationTo enable automated email delivery, set up the SMTP destination in your SAP BTP Cockpit:Navigate to your Subaccount > Connectivity > Destinations.Click Create Destination and set the general properties:Name: sap_process_automation_mailType: MAILProxy Type: InternetAuthentication: BasicAuthenticationUser: <your-smtp-email-address>Password: <your-16-character-app-password>Add the following key-value pairs under Additional Properties:KeyValuePurposemail.smtp.hostsmtp.gmail.comSMTP Server Hostnamemail.smtp.port587Encrypted Transport Portmail.smtp.authtrueEnables SMTP Credentials Authmail.smtp.starttls.enabletrueEnables Transport Layer Securitymail.smtp.starttls.requiredtrueMandates TLS Handshakemail.smtp.from<your-smtp-email-address>Configures Mandatory Sender Addressmail.transport.protocolsmtpSets Communications ProtocolClick Save.

Step 3: Release, Deploy & TestReturn to SAP Build Process Automation.Open Invoice_Approval_Process and confirm zero design errors are present.Click Release to set a new version tag (e.g., 1.0.3).Click Deploy and target your active BTP Subaccount runtime environment.Launch the VenderInvoiceInputForm trigger link, submit a sample invoice payload, and navigate to SAP My Inbox to claim, review, and approve the task.Verify the completed execution log inside Process and Workflow Monitoring.

## 📂 Repository Structure

```text
sap-btp-invoice-approval-automation/
├── src/
│   └── Enterprise_Invoice_Approval_v1.0.3.zip  # Exported SAP Build project archive file
├── docs/
│   ├── BTP_Destination_Setup.md                # Comprehensive destination config reference
│   └── Architecture_Overview.pdf               # System flow and data binding topology
├── screenshots/
│   ├── 01_Input_Form_Trigger.png               # Deployed vendor input form
│   ├── 02_My_Inbox_Task_Review.png             # Manager approval task pane in My Inbox
│   ├── 03_BTP_Destination_Config.png           # BTP Destination parameters configuration
│   └── 04_Monitoring_Success_Log.png           # Process and Workflow Monitoring green status log
├── LICENSE                                     # MIT License
└── README.md                                   # Repository documentation


🚀 Deployment & Configuration GuideStep 1: Import Project PackageDownload the Enterprise_Invoice_Approval_v1.0.3.zip archive file from the src/ directory of this repository.Open your SAP Build Lobby.Click Import at the top right and upload the .zip archive.Open the project to verify that the VenderInvoiceInputForm, Invoice_Approval_Process, and ManagerApprovalTask artifacts are visible.
Step 2: Configure BTP Mail DestinationTo enable automated email delivery, set up the SMTP destination in your SAP BTP Cockpit:Navigate to your Subaccount > Connectivity > Destinations.Click Create Destination (or From Scratch) and set the general properties:Name: sap_process_automation_mail (Must match exact system identifier)Type: MAILProxy Type: InternetAuthentication: BasicAuthenticationUser: <your-smtp-email-address>Password: <your-16-character-app-password>Add the following key-value pairs under Additional Properties:KeyValuePurposemail.smtp.hostsmtp.gmail.comSMTP Server Hostnamemail.smtp.port587Encrypted Transport Portmail.smtp.authtrueEnables SMTP Credentials Authmail.smtp.starttls.enabletrueEnables Transport Layer Securitymail.smtp.starttls.requiredtrueMandates TLS Handshakemail.smtp.from<your-smtp-email-address>Configures Mandatory Sender Addressmail.transport.protocolsmtpSets Communications ProtocolClick Save.
Step 3: Release, Deploy & TestReturn to SAP Build Process Automation.Open Invoice_Approval_Process and confirm zero design errors are present.Click Release to set a new version tag (e.g., 1.0.3).Click Deploy and target your active BTP Subaccount runtime environment.Launch the VenderInvoiceInputForm trigger link, submit a sample invoice payload, and navigate to SAP My Inbox to claim, review, and approve the task.Verify the completed execution log inside Process and Workflow Monitoring.

📸 Solution Screenshots1. Form Trigger (VenderInvoiceInputForm)Ingests vendor metadata and initiates process execution.(Place your screenshot at screenshots/01_Input_Form_Trigger.png)2. Task Interface (SAP My Inbox)Context-bound approval form displaying all process parameters to the manager.(Place your screenshot at screenshots/02_My_Inbox_Task_Review.png)3. Execution Logs (Process Monitoring)Audit history confirming successful task resolution and email notification execution.(Place your screenshot at screenshots/04_Monitoring_Success_Log.png)

💡 Key Architectural TakeawayThis implementation highlights the practical execution of SAP's Clean Core Paradigm:Core ERP stability is maintained because no custom Z-tables or ABAP workflow modifications were introduced in standard modules.Process agility is maximized by running business rules, forms, and decision routing on SAP BTP.Operational maintenance is reduced through centralized monitoring, low-code lifecycle releases, and standardized integration endpoints.

📄 LicenseThis repository is licensed under the MIT License. Feel free to use, modify, and distribute this template for enterprise automation research and development.
***

