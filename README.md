# sec-ops-labs
Helpful labs for Microsoft Defender XDR Security Operations and Incident Response

[Instruction folder has documents from a two-day Security Operations and Analyst class.
](https://github.com/dwnatwick/sec-ops-labs/tree/main/lab-instructions)


Additional helpful information and links are below:

Security Analyst – Defender XDR helpful links

**Day One:**
SC-200 MS Learn

**[Microsoft Certified: Security Operations Analyst Associate - Certifications | Microsoft Learn](https://learn.microsoft.com/en-us/credentials/certifications/security-operations-analyst/?practice-assessment-type=certification)**

Modules covered in class:
-	https://learn.microsoft.com/training/paths/sc-200-mitigate-threats-using-microsoft-365-defender/
-	https://learn.microsoft.com/training/paths/sc-200-mitigate-threats-using-microsoft-copilot-for-security/
-	https://learn.microsoft.com/training/paths/sc-200-utilize-kql-for-azure-sentinel/
-	Labs
    - Module 01-04
    - Module 06
    - Module 10

[Microsoft Defender XDR roles and permissions:
](https://github.com/dwnatwick/sec-ops-labs/blob/main/lab-instructions/email-permissions-lab-detail.md)

[Microsoft Defender XDR Unified role-based access control (RBAC) - Microsoft Defender XDR | Microsoft Learn
](https://learn.microsoft.com/en-us/defender-xdr/manage-rbac)

[Microsoft Defender for Office 365 permissions in the Microsoft Defender portal - Microsoft Defender for Office 365 | Microsoft Learn](https://learn.microsoft.com/en-us/defender-office-365/mdo-portal-permissions)

[Threat hunting queries – KQL
](https://github.com/dwnatwick/sec-ops-labs/blob/main/lab-instructions/kql-lab-detail.md)

[GitHub - cyb3rmik3/KQL-threat-hunting-queries: A repository of KQL queries focused on threat hunting and threat detecting for Microsoft Sentinel & Microsoft XDR (Former Microsoft 365 Defender).
](https://github.com/cyb3rmik3/KQL-threat-hunting-queries)

[Windows security event sets that can be sent to Microsoft Sentinel | Microsoft Learn
](https://learn.microsoft.com/en-us/azure/sentinel/windows-security-event-id-reference)

Lab github for simulation links:
[GitHub - MicrosoftLearning/SC-200T00A-Microsoft-Security-Operations-Analyst 
](https://github.com/MicrosoftLearning/SC-200T00A-Microsoft-Security-Operations-Analyst)

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/parsejsonfunction

**Day Two:**
SC-5004 MS Learn

**[Defend against cyberthreats with Microsoft Defender XDR - Training | Microsoft Learn
](https://learn.microsoft.com/en-us/training/paths/sc-5004-defend-against-cyberthreats-defender/)**

[Trellix – forensics
](https://github.com/dwnatwick/sec-ops-labs/blob/main/lab-instructions/trellix-lab-detail.md)

[AV – finding and pulling down information from SharePoint
](https://github.com/dwnatwick/sec-ops-labs/blob/main/lab-instructions/av-inspection-sharepoint-lab-detail.md)

[Microsoft Applied Skills: Defend against cyberthreats with Microsoft Defender XDR - Applied Skills | Microsoft Learn
](https://learn.microsoft.com/en-us/credentials/applied-skills/defend-against-cyberthreats-with-microsoft-defender-xdr/)

**Additional content:**

SC-5001 MS Learn

[Configure SIEM security operations using Microsoft Sentinel SC-5001 - Training | Microsoft Learn
](https://learn.microsoft.com/en-us/training/paths/configure-security-information-event-management-operations-using-microsoft-sentinel/)

[Microsoft Applied Skills: Configure SIEM security operations using Microsoft Sentinel - Applied Skills | Microsoft Learn
](https://learn.microsoft.com/en-us/credentials/applied-skills/configure-siem-security-operations-using-microsoft-sentinel/)

1.	Splunk SIEM/SOAR: Microsoft Defender integrates with Splunk through the Splunk Add-on for Microsoft Security. This allows ingestion of incidents and alerts from Defender products into Splunk's Common Information Model (CIM). It supports bi-directional updates, enabling changes in Splunk to reflect in Defender and vice versa. (Syslog solution or use SIEM Migration) 
[Integrate your SIEM tools with Microsoft Defender XDR - Microsoft Defender XDR | Microsoft Learn](https://learn.microsoft.com/en-us/defender-xdr/configure-siem-defender)

[Export Splunk data to target platform - Microsoft Sentinel | Microsoft Learn
](https://learn.microsoft.com/en-us/azure/sentinel/migration-splunk-historical-data)

2.	Palo Alto: Defender for IoT can integrate with Palo Alto Networks to provide multidimensional visibility for SOC analysts. This integration allows viewing Defender and Palo Alto data in one place or using Defender data to configure blocking actions in Palo Alto firewalls. (Solution available)

[Set up Microsoft Defender XDR for Integration
](https://docs.paloaltonetworks.com/iot/integration/endpoint-protection/integrate-iot-security-with-microsoft-defender-xdr/set-up-microsoft-defender-xdr-for-integration)

[Integrate Palo Alto with Microsoft Defender for IoT - Microsoft Defender for IoT | Microsoft Learn
](https://learn.microsoft.com/en-us/azure/defender-for-iot/organizations/tutorial-palo-alto)

3. Tenable: Microsoft Defender integrates with Tenable through the Tenable data connector in Microsoft Security Exposure Management. This enables the retrieval of vulnerability data and asset information from Tenable, helping organizations assess and manage risks effectively. (Solution available)
   
[Integrate Tenable data connector in Microsoft Security Exposure Management - Microsoft Security Exposure Management | Microsoft Learn
](https://learn.microsoft.com/en-us/security-exposure-management/tenable-data-connector)

4.	Archer/Splunk Case Management: Defender supports integration with case management systems like Splunk through APIs. This allows creating and updating tickets or alerts in external systems, ensuring seamless collaboration between security and IT teams.
Manage cases natively in Microsoft's unified SecOps platform -

[Microsoft's unified security operations platform | Microsoft Learn](https://learn.microsoft.com/en-us/unified-secops-platform/cases-overview)

[Integrate your SIEM tools with Microsoft Defender XDR - Microsoft Defender XDR | Microsoft Learn
](https://learn.microsoft.com/en-us/defender-xdr/configure-siem-defender)

5.	Defender for Endpoint and Intune: Defender for Endpoint integrates with Intune to enforce device compliance and manage security settings. This integration enables Conditional Access policies, device risk assessments, and remediation of vulnerabilities identified by Defender's Threat & Vulnerability Management.
   
[Onboard and Configure Devices with Microsoft Defender for Endpoint via Microsoft Intune | Microsoft Learn
](https://learn.microsoft.com/en-us/intune/intune-service/protect/advanced-threat-protection-configure)

[Use Microsoft Defender for Endpoint in Microsoft Intune | Microsoft Learn
](https://learn.microsoft.com/en-us/intune/intune-service/protect/advanced-threat-protection)


[Permissions - Visibility into Exchange Email
](https://github.com/dwnatwick/sec-ops-labs/blob/main/lab-instructions/roles-permissions-lab-detail.md)

