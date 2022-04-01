# Ransomware Basics

Introduction to Ransomware 

Ransomware is a type of malware that infects systems and files, rendering them inaccessible until a ransom is paid. When this occurs in the healthcare industry, critical processes are slowed or become completely inoperable. Ransomware has become such a pervasive issue it is now part of the defense posture adopted by the National Health Information Sharing and Analysis Center (NH-ISAC) and Financial Services Information Sharing and Analysis Center (FS-ISAC), both of whom have begun trainings around the United States to defend against it.  
Ransomware typically infects the victim hosts in one of three ways:  

 * through phishing emails containing a malicious attachment  
 * via a user clicking on a malicious link  
 * by viewing an advertisement containing malware (malvertising)  

Ever-evolving variants and tactics, techniques, and procedures (TTPs) make it hard for security experts to keep up. Additionally, platforms such as ransomware as a service[i] (RaaS) make it easy for anyone with little to no technical skill to launch ransomware attacks against victims of their choosing.  
Open-source reporting has indicated that Common Threat Actors (CTAs) are targeting managed service providers (MSPs) to push ransomware out to multiple entities and the MSP becomes compromised and its existing infrastructure is used to disseminate the ransomware. The resulting impact is broken trust in the relationship between the customer and the MSP. 
  
## Mitigating Risk and Protection

Mitigating the risks of ransomware follow existing best practices.  The most important item for proactive ransomware protection is advanced, zero-trust networking and IAM patterns.
  
### Ephemeral Access
  
Ensure no human has long-lived privileged access to environments.  **Privileged access would be just-in-time, temporary live access.**  
Using the just-in-time (JIT) access methodology, organizations can give elevate human and non-human users in real-time to provide elevated and granular elevated privileged access to an application or system in order to perform a necessary task.  

#### Types of Just-In-Time Access  
**Broker and remove access.** This approach enables the creation of policies that require users to provide a justification for connecting to a specific target for a defined period of time. Typically, these users have a standing, privileged shared account and credentials for that account are managed, secured and rotated in a central vault.  
**Ephemeral accounts.** These are one-time-use accounts, which are created on the fly and immediately deprovisioned or deleted after use.  
**Temporary elevation.** This approach allows the temporary elevation of privileges, enabling users to access privileged accounts or run privileged commands on a by-request, timed basis. Access is removed when time is up.  
  
### Network Boundaries
  
Ensure resources, especially resources that store data such as databases, are firewalled to only allow private internet connectivity.  **No public internet access to data resources.**
    
This impacts development cycles in that connectivity to a database must originate within the environment's private network, this includes both manual (human) and automated (ci/cd) access.  In public internet this could take shape of a jump host with proper IAM to then connect to a database or having a self-hosted ci/cd runner within the private network.  This is very visible and benefical layer of protection from any malicious connevitity to sensitive resources.

### Disaster Rescovery

Post incident mitigation.  Retro-active mitigation can happen from having proper disaster recovery in place at the data layer.  For example, if the active database is compromised and held for ransom, this database can be ignored and swapped for a replica instance with different credentials.

### CIS Security Guidelines

Additionally, the CIS Security guidelines suggest the following:   
  * Update or create an incident response plan that includes what to do during a ransomware event.  
  * If not already being done, perform regular system backups. As ransomware is known to delete Volume Shadow Copies, ensure that backups are created and stored off-site or out-of-band. Also, use a backup strategy that allows multiple iterations of the backups to be saved and stored, in case the backups include encrypted or infected files. Routinely test backups for data integrity and to ensure you can recover from them.  
  * For any publicly-exposed services, such as Remote Desktop Protocol (RDP), Virtual Network Computing (VNC), and File Transfer Protocol (FTP), assess the need for exposure to the Internet. Consider applying additional controls, such as IP whitelisting or multi-factor authentication, where possible.  
  * Assess the need to have Remote Desktop Protocol (port 3389) and Server Message Block (SMB) (port 445) open on systems and, if required, consider limiting allowed connections to only specific, trusted hosts.  
  * Enable heightened monitoring for SMB activity throughout the network. Make sure to disable SMBv1 on all systems and utilize SMBv2 or SMBv3 after appropriate testing.  
  * Consider filtering inbound and outbound traffic based on IP addresses (and ports), leveraging geographic blocking and threat-based blocking.
Implement filters at the email gateway to filter out emails with known malspam indicators, such as known malicious subject lines, and block suspicious IP addresses at the firewall.  
  * Provide end-user training to help users identify suspicious emails or links and ensure that users are aware of the potential dangers of opening unsolicited emails. Also ensure users are aware of any support policies and procedures in place for assistance.  
  * If you do not have a policy regarding suspicious emails, consider creating one and specify that all suspicious emails should be reported to the security and/or IT departments.  
  * Ensure all user accounts fall under (and are not exempt from) acceptable policies associated with password aging, password complexity, and account lockout.  
  * Perform network segmentation according to organizational functionality and apply access controls between trust zones.  
  * Adhere to the principal of least privilege, ensuring that users have the minimum level of access required to accomplish their duties. Limit administrative credentials to designated administrators and never reuse the same credentials across multiple accounts or systems.  
  * Review any vendor accounts and their associated passwords to ensure they have been changed from their default settings.  
  * If remote access for the user account is required by a third-party vendor, consider developing a process that keeps the user account disabled until access is needed.  
  * Implement robust Windows Event logging, including an increase in the maximum file size for the Event Logs. Centralize and protect these logs out-of-bound and consider a backup solution, to help prevent counter-forensic log during an active compromise.  
  * Utilize application whitelisting on all assets to ensure that only authorized software executes and all unauthorized software is blocked from executing. (CIS Subcontrol 2.7). 
  * Restrict PowerShell execution to signed scripts and trusted scripts used for administration.  
  * Ensure that the organization's anti-malware software updates its scanning engine and signature database on a regular basis. (CIS Subcontrol 8.2)
Strongly consider utilizing behavioral-based detection methods, as they can help identify the malicious use of open source penetration testing suites and legitimate network administration tools.  
  * Implement a centrally-managed, up-to-date anti-malware solution. In addition to valuable preventive and corrective capabilities, detective controls provided by anti-malware software are beneficial in providing awareness of any threats which may become active within the environment.  
  * Keep all operating systems, applications, and essential software patched and remove unsupported legacy systems to mitigate easy exploitation. This may include purchasing extended support for legacy systems that are critical to operations.  
  * If not already being done, consider implementing an Intrusion Detection System (IDS) to detect command and control (C2) activity and other potentially malicious network activity, such as the MS-ISAC’s Albert system.  
  * Ensure that systems are hardened with industry-accepted guidelines, such as those provided by the CIS Benchmarks.  
  * Review and consider implementation of the 20 CIS Controls, where appropriate, as a means of bolstering your organization’s security posture.  
