Below is a comprehensive, step-by-step lab to integrate Microsoft Defender for Endpoint (MDE) with Microsoft Intune and then bring that telemetry into Microsoft Defender XDR. This lab walks you through environment preparation, device onboarding via Intune, connecting Defender for Endpoint, and finally ensuring Defender XDR ingests and leverages endpoint data for enhanced threat detection and automated remediation.

---

## **Step 1. Prerequisites and Environment Preparation**

1. **Licensing and Access:**  
   - **Microsoft Defender for Endpoint:** Ensure you have a license (typically bundled with Microsoft 365 E5, Windows E5, or a standalone license) and administrative access to the Defender Security Center.  
   - **Microsoft Intune:** Verify that you have an active Intune subscription and access through the Microsoft Endpoint Manager admin center.  
   - **Microsoft Defender XDR:** Confirm that your Defender XDR tenant is active with rights to configure custom data connectors.

2. **Test Devices:**  
   - Identify one or more devices (Windows 10/11) for lab deployment. These devices should run a supported operating system and have connectivity to both Intune and Defender for Endpoint services.

3. **Administrative Tools:**  
   - Access to the Microsoft Endpoint Manager admin center (Intune portal).  
   - Access to the Microsoft Defender Security Center for Endpoint management.  
   - Access to the Defender XDR console to configure custom data ingestion.

---

## **Step 2. Onboarding Devices to Microsoft Intune and Microsoft Defender for Endpoint**

1. **Enroll Devices in Intune:**  
   - Log into the [Microsoft Endpoint Manager admin center](https://endpoint.microsoft.com).  
   - Navigate to **Devices > Enroll devices**.  
   - If necessary, create an enrollment profile (especially for Windows Autopilot, if applicable) to ensure devices are automatically enrolled in Intune when they join your network.
   - Verify that the test device appears under **Devices > All devices** as enrolled.

2. **Deploy a Defender for Endpoint Onboarding Policy via Intune:**  
   - In the Endpoint Manager admin center, proceed to **Devices > Windows > Configuration profiles**.  
   - Create a new profile with the following settings:  
     - **Platform:** Windows 10 and later  
     - **Profile type:** Templates > Endpoint protection  
   - Within the profile, configure the onboarding settings to enable Defender for Endpoint. Microsoft now offers automated onboarding that might be as simple as toggling “Automatic onboarding” (for Windows Defender antivirus) on supported devices or using a custom onboarding package.
   - Assign the profile to the test device group.
   - On the device, confirm that the onboarding package installs. You can check this via an elevated PowerShell prompt by running the onboarding status script provided in the Defender for Endpoint portal.

3. **Validate Device Onboarding:**  
   - In the **Microsoft Defender Security Center**, navigate to **Settings > Device inventory**.  
   - Confirm that the newly enrolled device appears, and its Defender status is set to “Onboarded.”  
   - Check for recent alerts or telemetry data to ensure the Defender sensor is active.

---

## **Step 3. Configuring Defender XDR to Ingest Defender for Endpoint Data**

1. **Establish the Connector in Defender XDR:**  
   - In the Defender XDR console, navigate to **Settings > Data Sources > Custom Connectors** (or the equivalent section for endpoint integrations).  
   - Select to add a new connector and choose the **Microsoft Defender for Endpoint connector**.  
   - Provide the required details such as API endpoints and credentials. Often, this involves:  
     - Retrieving API tokens or service principal credentials from the Defender Security Center (consult Microsoft’s documentation for generating API access tokens for MDE).  
     - Entering those credentials and the endpoint URL (for example, *https://api.securitycenter.windows.com*) into Defender XDR.

2. **Define Polling and Data Ingestion Settings:**  
   - Set the polling interval (e.g., every 5–10 minutes) so Defender XDR continuously ingests new telemetry or alert data from Defender for Endpoint.
   - Optionally, configure filters to only ingest certain events (for example, high-severity alerts or specific threat detections).

3. **Test the Integration:**  
   - In the Defender XDR dashboard, check that endpoint data from Defender for Endpoint (such as device inventory, threat alerts, or behavioral telemetry) is visible.
   - Verify field mapping and that key fields (e.g., device ID, alert type, severity, timestamp) are properly parsed.

---

## **Step 4. Creating Security Policies and Automated Responses**

1. **Define Detection Rules in Defender XDR:**  
   - Leverage the ingested endpoint data to create custom detection rules. For example:  
     - Trigger an alert if multiple high-severity threats are reported from a single device over a short interval.  
     - Correlate indicators such as unusual process execution or unauthorized changes with Defender for Endpoint alerts.
   - Use the mapping (e.g., Defender’s telemetry field for “device id” and “risk score”) to build these rules.

2. **Configure Automated Remediation Actions via Intune:**  
   - In the Defender XDR console, integrate automated response playbooks that can reach out to Microsoft Endpoint Manager.  
   - Example workflow:  
     - When a critical alert is triggered from MDE data, automatically send a command to Intune to isolate the affected device or mark it as non-compliant.  
     - Configure a PowerShell remediation script via Intune that can disable network interfaces or trigger a more detailed investigation.
   - Test the workflow by simulating a threat or using a test alert to confirm the automated response.

---

## **Step 5. End-to-End Validation and Fine-Tuning**

1. **Simulate an Endpoint Threat:**  
   - On the test device, simulate a benign test scenario (Microsoft offers safe exercises such as running the Microsoft Defender test scripts) that should trigger an alert in Defender for Endpoint.
   - Verify that the simulated event is logged within Defender for Endpoint.

2. **Monitor Data Flow and Alerts in Defender XDR:**  
   - Confirm that the alert shows up in Defender XDR with correct field mappings.  
   - Check the response playbook, ensuring that the automated remediation command is sent to Intune and that the device receives the remediation policy.

3. **Fine-Tuning:**  
   - Adjust detection thresholds and parsing rules within Defender XDR based on the behavior of test events.  
   - Ensure that non-critical events are filtered appropriately to reduce alert fatigue.
   - Review Intune policies and ensure they align with your organization’s security baseline for endpoint isolation or remediation.

4. **Document and Train:**  
   - Record all configurations, API endpoints, credentials (securely stored), and mapping rules.  
   - Develop training documentation and incident response guidelines for your security operations team based on the lab outcomes.

---

## **Additional Considerations and Next Steps**

- **Continuous Monitoring:** Regularly review dashboards in all three systems—the Defender Security Center, Intune (Endpoint Manager), and Defender XDR—to ensure data flows correctly and to refine detection rules based on operational experience.  
- **Expanding Integrations:** As your environment matures, consider expanding the integration of Microsoft Defender XDR with other data sources (such as Microsoft Cloud App Security or network telemetry) to create a more comprehensive security picture.  
- **Automation Enhancements:** Explore advanced automation and orchestration features within your SOAR (Security Orchestration, Automation, and Response) platform that might integrate with both Defender for Endpoint and Intune, allowing for multi-step incident response scenarios.

This lab establishes a robust framework for utilizing Microsoft Defender for Endpoint data—onboarded via Intune—within Microsoft Defender XDR, allowing you to detect threats earlier and respond with automated remediation across your managed endpoints. 