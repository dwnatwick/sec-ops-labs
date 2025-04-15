Below is a detailed, step‐by‐step guide to configuring the necessary permissions in Microsoft Defender XDR so you can gain visibility into Exchange Online email:

---

### 1. Verify Prerequisites and Licensing

- **Check Licensing:**  
  Ensure your organization holds the correct licenses. Visibility into email data through Defender XDR typically requires an appropriate Microsoft Defender for Office 365 plan (usually Plan 2) along with other Defender XDR components. This step is critical to enable the integration of email telemetry with Defender XDR.

- **Review Environment Setup:**  
  Confirm that your tenant is set up to use Microsoft Defender XDR and that you have a recent configuration—especially as Microsoft is transitioning new tenants to the Unified RBAC model. This ensures you’re working with supported capabilities and features.

---

### 2. Understand the RBAC Model in Use

- **Unified RBAC Model vs. Classic Permissions:**  
  Microsoft Defender XDR now uses a unified role-based access control (RBAC) model for centralized permissions management across several security solutions.  
  - **If you’re a new tenant or planning to enable Unified RBAC:** Configure necessary roles and permissions *before* activating Defender XDR Unified RBAC. Once activated, the legacy “Email & collaboration permissions” page (via https://security.microsoft.com/emailandcollabpermissions) becomes unavailable.  
  - **If you’re still using the classic permissions model:** You may continue to use the email & collaboration roles available in the Defender portal until you transition.

This initial understanding helps in deciding whether to assign global roles or custom roles for granting visibility into Exchange Online email.

---

### 3. Assign Global Microsoft Entra Roles (if applicable)

- **Use Built-in Global Roles:**  
  Microsoft Entra (formerly Azure AD) roles control access to Defender XDR capabilities. In many cases, you might assign roles such as:
  - **Global Administrator:** Full control (use with caution).
  - **Security Administrator or Security Operator:** For managing and monitoring security-related capabilities.
  - **Global Reader or Security Reader:** For view-only permissions.

- **Apply the Principle of Least Privilege:**  
  Always assign the fewest permissions necessary to maintain security hygiene. Only use highly privileged roles (like Global Administrator) in emergency scenarios or for critical operations.

---

### 4. Configure Microsoft Defender for Office 365 Specific Permissions

Since email data comes via Defender for Office 365, you need to set permissions specifically for the email & collaboration area:

- **Access the Permissions Page:**  
  Navigate to the Microsoft Defender portal and go to **Permissions > Email & collaboration roles > Roles**.  
  Alternatively, use the direct URL: [https://security.microsoft.com/emailandcollabpermissions](https://security.microsoft.com/emailandcollabpermissions).

- **Assign Roles for Email Management:**  
  - **Role Management:** Ensure key personnel have the Role Management permission, which allows viewing, creating, and modifying Defender for Office 365 role groups. By default, this role is part of the Organization Management role group (and usually assigned to global admins).
  - **Custom Roles:** If your organization needs fine-tuned access, consider creating custom roles with only the necessary permissions to view and manage email-related data.

This step is important for granting users the precise control needed to perform investigations and manage email alerts, policies, and investigations.

---

### 5. Integrate Exchange Online Role Permissions

Visibility into Exchange Online email within Defender XDR requires that certain Exchange Online-specific permissions are in place:

- **Assign Exchange Online Roles Separately:**  
  Exchange Online’s PowerShell and Security & Compliance PowerShell continue to use their dedicated role structures. Make sure that the accounts needing email visibility are also granted the correct Exchange Online roles. For example:
  - **Compliance Management** or **View-Only Configuration** roles can be used as a baseline to allow monitoring of email infrastructure.
  - Use the Exchange Online admin center or PowerShell to assign and verify these roles.

- **Maintain Consistency Between Platforms:**  
  Keep in mind that while Defender XDR centralizes many permissions, the email telemetry from Exchange Online is still governed by Exchange-specific settings. Double-check your configurations to ensure these roles aren’t overlooked.

---

### 6. Test Permissions and Validate Visibility

- **Perform a Test Login:**  
  Use an account with the newly assigned permissions to log in to the Defender portal. Ensure the dashboard correctly displays Exchange Online email data and alerts.

- **Validate Data Scope:**  
  If your Defender for Endpoint or Office 365 roles are scoped (for instance, limited to a set of devices or mailboxes), verify that the scope correctly reflects in the data shown for email events.

- **Make Adjustments as Needed:**  
  Fine-tune the role assignments or scope settings if you notice any discrepancies in email visibility.

---

### 7. Audit, Document, and Monitor

- **Document Role Assignments:**  
  Keep comprehensive records of who has been assigned which roles, along with notes on why those permissions are needed.

- **Monitor Logs and Alerts:**  
  Regularly review audit logs within the Defender portal and Exchange Online admin center. This not only verifies that permissions are active but also helps detect any unintended permission escalations.

- **Plan Regular Reviews:**  
  Security and compliance best practices recommend periodic reviews of all role assignments to ensure that only the minimum necessary permissions remain in effect.

---

### 8. Stay Informed on Updates

Microsoft’s security solutions evolve frequently. To stay current:
  
- Regularly review the official Microsoft documentation on [Microsoft Defender XDR RBAC](https://learn.microsoft.com/en-us/defender-xdr/manage-rbac) and [Defender for Office 365 permissions](https://learn.microsoft.com/en-us/defender-office-365/mdo-portal-permissions).  
- Keep an eye on update notices, especially around RBAC changes, to ensure ongoing compliance and optimal configuration.

---

By following these steps, you’ll enable the necessary configuration within Microsoft Defender XDR to ensure visibility into Exchange Online email, allowing your security team to monitor, investigate, and act on email-based threats effectively. This approach balances the need for granular access while maintaining robust security controls.

**Additional Considerations:**  
If you’re interested in further refining access beyond what built-in roles offer, consider exploring custom role creation within the unified RBAC framework. This might include setting specific scopes or combining permissions from multiple solutions to tailor the security posture uniquely for your organization. Moreover, integrate your role management with periodic security reviews to adapt to evolving threats and compliance requirements.


Let's dive deeper into the configuration details and best practices for managing custom roles in Microsoft Defender XDR. Custom roles allow you to tailor access so that users only see the data and features they require—minimizing risk while maximizing security insight. Here’s an in-depth look:

---

### 1. **Understanding the Role and Scope in Unified RBAC**

Defender XDR now enjoys a Unified RBAC experience that centralizes permissions while permitting granular control. Each custom role you create is distinct from any global roles in Microsoft Entra ID—even if names overlap. This separation lets you have fine-tuned control, ensuring that, for instance, a custom “Security Admin” in Defender for Endpoint doesn’t automatically inherit permissions in another product like Defender for Office 365. Each role can be scoped to specific data sources (or even include future data sources), so you’re not left in the dark when new capabilities roll out.

---

### 2. **Steps to Create and Configure a Custom Role**

Here’s a more detailed walk-through:

1. **Sign In and Navigate:**
   - Log in to the Defender portal at [security.microsoft.com](https://security.microsoft.com).
   - Navigate to **System > Permissions** and choose **Roles**.
   
2. **Start the Role Creation:**
   - Select **Create custom role**. You’ll be prompted to provide a name and a brief description. Use clear, descriptive names that immediately communicate the role’s purpose.

3. **Select Permissions:**
   - You’ll face three levels of permissions for each category:
     - **All Read-Only:** Use this when you want users to monitor data without being able to modify settings.
     - **All Read and Manage:** This grants comprehensive permissions in that category.
     - **Custom Permissions:** Handpick specific rights that the role should include.
   - Consider grouping by categories like “Security Operations” or “Incident Management” so that each custom role is specialized. This modular approach allows you to mix and match based on responsibilities.

4. **Define Data Source Scope:**
   - During setup, specify the “data sources” where these permissions apply. You can either assign the role across all available Defender XDR services or restrict it to specific ones (e.g., only Microsoft Defender for Endpoint or Office 365).
   - For scalability, consider checking the option to “Include future data sources automatically” if you want the role’s permissions to extend to newly integrated modules.

5. **Assigning the Custom Role:**
   - Once permissions are chosen, move to assignment. Add the assignment name and select the appropriate users or groups. Keep in mind that assignment history and changes should be documented for audit purposes.

---

### 3. **Best Practices for Managing Custom Roles**

**a. Follow the Principle of Least Privilege**  
   - Always start with the narrowest set of permissions. Provide only the essential capabilities users need to perform their tasks. This reduces the potential attack surface and limits the accidental or malicious misuse of privileges.

**b. Test Thoroughly**  
   - Before rolling out custom roles to production, deploy them in a controlled test environment. Use test accounts to validate that the assigned permissions yield the expected outcomes. Ensure that sensitive controls aren’t accessible unintentionally.

**c. Document and Track Role Configurations**  
   - Maintain a comprehensive inventory of all custom roles along with details on the permissions assigned, the data source scope, and which users or groups hold them. This aids in future reviews, troubleshooting, or any necessary audits.

**d. Regular Reviews and Updates**  
   - Schedule periodic role assessments. Microsoft Defender XDR is evolving rapidly; new permissions may be added over time. If you assign custom permissions individually, you might need to update roles to incorporate newly supported actions that affect security posture.

**e. Separate Duties Clearly**  
   - Avoid role overlap by distinctly segmenting responsibilities. For instance, one custom role might be solely for investigating alerts in Defender for Endpoint, while another might handle policy configuration in Defender for Office 365. This separation not only improves security but also helps in pinpointing issues when they arise.

**f. Use “Include Future Data Sources” When Appropriate**  
   - If your organization prefers to minimize administrative overhead, and you have a robust testing mechanism, you might opt for including future data sources automatically. However, remain cautious: new data sources could bring additional permissions that need careful review.

**g. Align with Global and Service-Specific Roles**  
   - Even though custom roles are independent, they should align with broader organizational security policies. Where possible, supplement them with corresponding global or service-specific roles in Microsoft Entra to ensure a cohesive security strategy.

---

### 4. **Troubleshooting and Maintaining Custom Role Configurations**

- **Audit and Compliance:**  
  Regularly monitor access logs and role assignments. Use audit reports provided in the Defender portal to immediately detect discrepancies or unauthorized escalations in permissions.

- **Change Management:**  
  Establish a change control process for role modifications. Whenever a role is updated—especially when new permissions are appended—document the change and notify affected stakeholders.

- **User Feedback:**  
  Involve end-users and administrators in periodic reviews. They can identify potential gaps or unnecessary restrictions that might hinder critical operations.

- **Training and Communication:**  
  Provide ongoing training for your security team about changes to Defender XDR roles and the platform’s evolving capabilities. Awareness is a key aspect of good role management.

---

By approaching custom role management with these best practices and detailed configuration steps, you not only enhance security but ensure that your organization is agile enough to adapt to Microsoft Defender XDR updates—a key aspect in today’s ever-evolving threat landscape. If you’d like, we can further explore scenarios for transitioning existing roles to custom configurations or dive into tool-assisted audits for these roles. 