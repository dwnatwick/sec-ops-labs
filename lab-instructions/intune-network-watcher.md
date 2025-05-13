Below is an example lab guide outlining how you might integrate Azure Network Watcher with Intune-managed endpoints by leveraging Azure policies and automation scripts. Keep in mind that Network Watcher is designed primarily to capture traffic on Azure-based resources (for example, virtual machines in a virtual network), so when dealing with endpoints managed by Intune—which are often situated outside your Azure Virtual Network—you’ll be creating a workaround that brings local packet capture data into your centralized Azure monitoring environment.

Read on for a walkthrough that explains the process, key prerequisites, and step-by-step instructions.

---

## 1. Prerequisites

- **Azure Subscription & Network Watcher:**  
  Ensure you have an active Azure subscription with Network Watcher enabled in your region. (For example, enable it from the Azure portal by navigating to *Network Watcher > Packet Capture*.)  

- **Intune-Managed Devices:**  
  Confirm that your endpoints (typically Windows 10/11 devices) are enrolled in Microsoft Intune and joined to Azure AD.  
 
- **Administrative Permissions:**  
  You should have sufficient rights in both the Azure portal (for creating policies and automation tasks) and the Intune console (to deploy scripts to endpoints).  

- **Required Modules & Tools:**  
  Install the necessary PowerShell modules (e.g., `Az.NetworkWatcher` for interacting with Network Watcher and Azure Automation modules) and ensure that endpoints can run your chosen packet capture commands (such as using `netsh` or Windows’ built-in diagnostic tools).

---

## 2. Designing the Packet Capture Solution

Because Network Watcher’s native packet capture is targeted at Azure VMs, you’ll need an alternate method to collect traffic data on Intune-managed endpoints:

- **Local Packet Capture Agent:**  
  Develop a PowerShell script that uses Windows’ native commands (for example, `netsh trace start …` and `netsh trace stop`) to capture a network trace locally on the device. This script should:
  - Initiate a packet capture session with your desired filters (ports, protocols, duration, etc.).
  - Stop the capture after a set period or data threshold.
  - Save the capture file (a `.etl` or `.cap` file) to a predetermined folder.

- **Uploading Captured Data to Azure:**  
  Once the capture file is generated, modify the script to upload that file automatically to an Azure Storage account or a Log Analytics workspace. You could use AzCopy or the Azure Storage REST API for this purpose. This “push” mechanism is your bridge to the central Azure-based analysis that Network Watcher typically provides.

---

## 3. Deploying and Automating Using Intune

### A. **Script Deployment via Intune**

1. **Create a PowerShell Script:**  
   Develop your PowerShell script based on the design above. Make sure it has logging or status reporting so that you can monitor whether it successfully captured and uploaded data.

2. **Deploy the Script in Intune:**  
   - Log in to the Microsoft Endpoint Manager admin center.
   - Navigate to **Devices > Scripts** and add a new Windows PowerShell script.  
   - Upload your script and configure its settings (for example, whether it runs in the system or user context, and if it should run silently).
   - Assign the script to a target group of devices.  
   - Monitor execution via the Intune reporting dashboard.

### B. **Integrate with Azure Automation**

1. **Set Up an Azure Automation Account:**  
   Create an Automation Account in the Azure portal if you don’t already have one. This account will host runbooks that monitor your uploaded capture files.

2. **Create a Runbook for Monitoring and Analysis:**  
   Develop a runbook to:
   - Periodically scan your Azure Storage account or Log Analytics workspace for new packet capture files.
   - Optionally, trigger alerts or further automated analysis (or even re-run captures if necessary).
   - Use available cmdlets such as `Start-AzNetworkWatcherPacketCapture` if you have any endpoints hosted on Azure or simply process and analyze the uploaded data.

3. **Schedule and Link Automation:**  
   Schedule the runbook to run at regular intervals. Link any findings or alerts to Azure Monitor for centralized notifications.

---

## 4. Enforcing Compliance with Azure Policy

Using Azure Policy can help verify that all endpoints are “compliant” with your packet capture solution. Two approaches include:

1. **Policy for Log Upload Compliance:**  
   Create a **custom Azure Policy definition** that checks for a specific diagnostic artifact (such as a recently uploaded capture file) in your storage account or Log Analytics workspace. This policy can audit if an enrolled device last pushed a packet capture in, say, the past 24 hours.

2. **Policy for Agent/Script Deployment:**  
   Alternatively, design a policy that verifies if the required configuration (or if a management extension is installed) exists on your endpoints. Though Azure Policy traditionally governs Azure resources, with custom definitions you can audit metadata or resource tags that signal a successful script run.

3. **Remediation Tasks:**  
   Attach remediation tasks to your custom policies that trigger the deployment of your capture script (via Intune or via an Azure Automation runbook) if non-compliance is detected, ensuring that devices automatically update their configuration for future captures.

---

## 5. Monitoring and Validation

- **Centralized Dashboard:**  
  In Azure Monitor or Log Analytics, build a dashboard that displays the status of packet capture uploads, device compliance based on the Azure Policy audit, and any alerts triggered by unusual network activity.

- **Review Automation Logs:**  
  Regularly check the logs generated by the Intune script deployments and Azure Automation runbooks to ensure captures are running as scheduled and uploads are completing successfully.

- **Test and Iterate:**  
  Run your packet capture on a small group of devices first. Validate that you can retrieve and analyze the log files in Azure, then iterate on the policy definitions or automation scripts based on your findings.

---

## Summary

By deploying a PowerShell script through Intune that runs locally on an endpoint to capture network packets and then uploads these files to a centralized Azure resource, you create a bridge between your Intune-managed devices and Azure Network Watcher–style analysis. Azure Automation can schedule and analyze these uploads, while custom Azure policies ensure compliance by auditing that each device meets your set criteria. This solution not only allows for proactive network diagnostics on diverse endpoints but also centralizes the review and analysis process within Azure.

This guide is an adaptable framework; you may need to refine each step based on your organization’s environment, regulatory requirements, and operational goals.

---

additional links for network watcher packet capture:  
https://learn.microsoft.com/en-us/azure/network-watcher/packet-capture-overview
https://learn.microsoft.com/en-us/azure/network-watcher/packet-capture-inspect
