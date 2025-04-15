Below is a step‐by‐step lab that demonstrates how to review antivirus logs and then pull down relevant file or metadata information from SharePoint. In this lab you will:

1. Verify that antivirus logging is enabled and collected centrally.  
2. Use Kusto Query Language (KQL) in Microsoft Defender XDR (Advanced Hunting) to review antivirus events.  
3. Extract key details (such as file name or hash) from antivirus logs.  
4. Use Microsoft Graph API (or PowerShell) to query SharePoint for files that match the event details.  
5. Correlate the two sets of data as part of an incident investigation workflow or forensic analysis.

> **Note:**  
> This lab assumes you have an environment with Microsoft Defender for Endpoint (or Microsoft 365 Defender) enabled and that antivirus events are being ingested into Advanced Hunting. You also need sufficient permissions (via an Azure AD app or your account) to call the Microsoft Graph API for SharePoint.

---

## **Step 1. Verify Antivirus Log Collection and Environment Setup**

1. **Ensure Antivirus Logging is Enabled:**  
   - Verify that your endpoints (for example, running Windows Defender Antivirus or a third-party solution) have logging enabled.  
   - Confirm that events such as detections, quarantines, and scan results are being forwarded either to Windows Event logs or collected into your Microsoft Defender for Endpoint telemetry.

2. **Centralize Logs (Optional):**  
   - If not already in place, configure Windows Event Forwarding or a SIEM (such as Azure Sentinel) so that antivirus logs are centrally available.  
   - In Microsoft Defender for Endpoint, antivirus-related events are ingested into tables (for example, in Advanced Hunting you can query the **DeviceEvents** table).

---

## **Step 2. Query Antivirus Logs Using KQL in Microsoft Defender XDR**

1. **Access Advanced Hunting:**  
   - Log into the [Microsoft 365 Defender Portal](https://security.microsoft.com).  
   - Navigate to **Advanced Hunting** from the left-hand menu.

2. **Build a Query to Review Antivirus Events:**  
   - Below is an example query that targets antivirus detection events. Adjust the table name and field filters to suit your environment if necessary.
   
   ```kql
   // Example: Query for antivirus detection events
   DeviceEvents
   | where ActionType == "AntivirusDetection"
   | project Timestamp, DeviceName, FileName, FolderPath, AdditionalFields, ReportId, InitiatingProcessFileName
   | order by Timestamp desc
   ```
   
   - **Explanation:**  
     • The query filters the **DeviceEvents** table for events marked as an “AntivirusDetection” (this may vary depending on how your solution logs antivirus events).  
     • The `project` operator selects key fields such as the time stamp, device name, file name, and other details.  
     • The results are ordered with the most recent events appearing first.

3. **Run and Save the Query:**  
   - Click **Run query** and review the output.  
   - Identify any columns (for instance, file hash or file name) that might help correlate with SharePoint documents.  
   - Save the query (using a descriptive name like “Antivirus Detections Overview”) for future investigations.

---

## **Step 3. Extract Key Details from Antivirus Logs**

1. **Identify Critical Indicators:**  
   - Look for file names, file hashes, or other identifiers embedded in the antivirus event details that might represent a malicious file incident.
   
2. **Refine the Query if Needed:**  
   - For example, if you’re interested in events involving a suspicious file name pattern, further refine your query as follows:
   
   ```kql
   DeviceEvents
   | where ActionType == "AntivirusDetection"
   | where FileName contains "suspicious"
   | project Timestamp, DeviceName, FileName, FolderPath, ReportId
   | order by Timestamp desc
   ```
   
   - Once you have a clear list of events, note a value (such as a file name or hash) that you’d like to search for in SharePoint later.

---

## **Step 4. Retrieve Information from SharePoint Using Microsoft Graph API**

1. **Set Up Your Environment for Graph API Calls:**  
   - Ensure you have the necessary permissions to query SharePoint Online (for example, using an Azure AD app registration with Files.Read.All or Sites.Read.All permissions).  
   - You can test queries via the [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) or use a PowerShell script with the Microsoft Graph module.

2. **Query SharePoint for File Information:**  
   - For example, to list files from a SharePoint document library on a specific site, use this HTTP GET request:
     
     ```
     GET https://graph.microsoft.com/v1.0/sites/{site-id}/drive/root/children
     ```
   
   - Replace `{site-id}` with the identifier for your target site.  
   - You can also use the search API—if you want to locate a file by name (extracted from your antivirus logs), you might use:
     
     ```
     GET https://graph.microsoft.com/v1.0/sites/{site-id}/drive/root/search(q='{fileName}')
     ```
   
   - **Example using PowerShell:**  
     If you prefer PowerShell, install the Microsoft Graph PowerShell SDK and run:
   
     ```powershell
     # Connect to Microsoft Graph
     Connect-MgGraph -Scopes "Sites.Read.All"
     
     # Get Site information (replace with your SharePoint site domain and path)
     $site = Get-MgSite -SiteId "contoso.sharepoint.com:/sites/YourSite"
     
     # Search for a file by name (replace 'suspicious.exe' with the file name from antivirus logs)
     Get-MgDriveItem -SiteId $site.Id -DriveId $site.Drive.Id -Search "suspicious.exe"
     ```
     
   - This command will return details about the file found on SharePoint, such as file path, modified date, and metadata.

---

## **Step 5. Correlate Antivirus Data with SharePoint Information**

1. **Establish a Correlation:**  
   - If your investigation scenario requires you to verify whether a suspicious file detected by antivirus exists in SharePoint (for example, if a malicious file was inadvertently uploaded), use the file name or hash extracted from your antivirus logs as the search key.
   
2. **Execute a Cross-Reference:**  
   - With the file name from Step 3, run the search API (or the PowerShell command shown above) to determine if the file is present in the SharePoint library.
   - Verify file metadata and access details to determine if the file is out-of-place or potentially compromised.

3. **Document Your Findings:**  
   - Record the results from both your Advanced Hunting queries and your SharePoint search.  
   - This correlation helps build the evidence chain required for further incident response and remediation.

---

## **Step 6. Automate and Integrate for Incident Response**

1. **Automated Workflow (Optional):**  
   - To streamline incident investigations further, consider integrating these steps into an automated workflow via Power Automate. For example, when an antivirus detection event is logged that meets a specific criterion, the workflow could automatically trigger:
     - A call to the Microsoft Graph API to search for the file in SharePoint.
     - An update to an incident record in a central database or notify the incident response team.

2. **Document the Process:**  
   - Create a runbook or playbook that outlines how to use your saved Advanced Hunting queries and your SharePoint search scripts.  
   - Ensure that your incident response team understands how to correlate data across the antivirus logs and SharePoint to accelerate investigations.

---

## **Step 7. Final Testing and Review**

1. **Simulate an Incident:**  
   - For testing, simulate an antivirus detection event by manually creating a test record (or use a known safe file tagged appropriately) that appears in your Advanced Hunting query results.
   - Use the file name from the event to manually verify a matching file’s metadata in your SharePoint library via Graph Explorer or your PowerShell script.
   
2. **Review and Refine:**  
   - Adjust queries, thresholds, or search parameters as necessary based on your environment and the behavior of your systems.
   - Store your final queries and scripts in a central repository for future use and incident response playbooks.

---

## **Conclusion**

By following this lab, you have learned how to:

- Review antivirus logs using KQL in Microsoft Defender XDR’s Advanced Hunting.
- Extract essential details from antivirus detections for forensic analysis.
- Use Microsoft Graph API and/or PowerShell to retrieve associated information from SharePoint.
- Correlate data across multiple sources to form a clear incident picture for effective response.

This integrated approach not only enhances your threat detection capabilities but also streamlines incident response by quickly linking suspicious antivirus events to file artifacts stored in SharePoint. Happy hunting and incident responding!