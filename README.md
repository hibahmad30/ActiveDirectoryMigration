<h1>On-Premise AD Migration to Microsoft Entra ID</h1>

<h2>Description</h2>
Vulnerability management is essential for organizations to identify and mitigate security vulnerabilities in their systems and networks. This project aims to explore the various stages of the vulnerability management lifecycle using Nessus Essentials and an insecure Windows 10 system. Nessus is a powerful tool that provides extensive scanning and remediation capabilities for vulnerability management. The following link provides an overview of how to create a virtual machine in Azure: https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal. 
<br />

<h2>Environments Used </h2>

- <b>Windows Server 2019</b>
- <b>Azure Virtual Machines</b>
- <b>Windows Active Directory</b>
- <b>Microsoft Entra ID (formerly Azure AD)</b>
- <b>Domain Name System (DNS)</b>
- <b>Microsoft Entra Connect</b>

<h2>Create the Active Directory Server:</h2> 

<p align="center">
We will begin by navigating to the Azure portal and spinning-up a virtual machine to host the AD server on. This involves creating a Resource Group to manage the resources we will be provisioning, naming the virtual machine instance, selecting the Windows Server 2019 image. and defining the login credentials for the Windows Server administrative account. The following link provides an overview of how to create a virtual machine in Azure:<br /><br />https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal<br />
 <br/>
 <br/>
<img src="https://i.imgur.com/wOCjWl1.png" alt="Provision Windows Server VM"/>
<img src="https://i.imgur.com/IlZX1vP.png" alt="Provision Windows Server VM"/>
<!<img src="https://i.imgur.com/oU2Y1eD.png" alt="Provision Windows Server VM"/>>
<!<img src="https://i.imgur.com/TTC5jyK.png" alt="Provision Windows Server VM"/>>
<img src="https://i.imgur.com/jY6qVAD.png" alt="Deploying Windows Server VM"/>

<h2>Configure AD Server and Promote as Domain Controller:</h2> 
<p align="center">
Once the VM deployment is complete, we will navigate to the resource and copy the public IP address of the instance. Then, we will use RDP into the instance using the administrator credentials defined earlier. 
<br />
<br />
<img src="https://i.imgur.com/YYT0VMp.png" alt="RDP"/> 
<br />
<br />
Once we are signed in and the VM has initialized, we will add the following roles to the server: Active Directory Domain Services, DNS server. This will promote the server as the Domain Controller. Once the installation is complete, we will need to create the root domain. I used the Ionos service to create a unique domain (activeshiftentra.com), and entered it under 'Add a new forest'. 
<br />
<br />
<img src="https://i.imgur.com/n0ASKZO.png" alt="Server Manager"/> 
<img src="https://i.imgur.com/rPTPMaH.png" alt="Select Server Roles"/> 
<img src="https://i.imgur.com/5PbNvdL.png" alt="Configure Root Domain Name"/> 
<br/>
<br/>
Once the configuration is complete, we will verify the deployment: 
<br/>
<br/>
<img src="https://i.imgur.com/ByKi3Mt.png" alt="RDP verification"/> 
<img src="https://i.imgur.com/TmKoZMO.png" alt="AD verification"/>
<br/>
<br/>
<h2>Create Organizational Structure (Users, Groups, Workstations):</h2> 
 <p align="center">
Now that we have successfully configured AD, we will now create organizational units (OUs), users, and provision some computers, laptops, and servers. I have created the following sample organzational structure to replicate and provision in AD:  
<br/>
<br/>
<img src="https://i.imgur.com/R47I8uC.png" alt="Organizational Chart"/>
 <img src="https://i.imgur.com/OGMTuwh.png" alt="Create Users"/>
 <img src="https://i.imgur.com/4A8fSEY.png" alt="Provision Workstations and Servers"/>
 <br/>
 <br/>
For practice, we will edit  the Group Policy Object (GPO) for the Default Domain Controller password policy which will apply to all Organizational Units (OUs) by navigating to following directories in the Group Policy Management Editor: 
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy. 
<br/>
<br/>
<img src="https://i.imgur.com/gyYk45n.png" alt="Password Policy GPO"/>
<br/>
<br/>
<h2>Migrating AD to Entra ID:</h2> 
<p align="center">
We will now begin migrating our Active Directory setup to Microsoft Entra ID! On our server instance, we will install Microsoft Entra Connect by navigating to the following link: https://www.microsoft.com/en-us/download/details.aspx?id=47594. 
<br/>
<br/>
<img src="https://i.imgur.com/XOFJRy0.png" alt="Microsoft Entra Connect Download"/>
<img src="https://i.imgur.com/vDNukck.png" alt="Microsoft Entra Connect Setup"/>
<br/>
<br/>
We will then connect our AD by entering the domain name. The next step is to register the domain in Azure, and ensure that it matches the on-prem domain. As shown below, the Azure AD domain is not currently added: 
<br/>
<br/>
<img src="https://i.imgur.com/4hGLGb4.png" alt="Connect Domain"/>
<img src="https://i.imgur.com/3IIfjIj.png" alt="Azure Domain Needed"/>
<br/>
<br/>
To add the domain in Azure, navigate to Microsoft Entra ID, and to the following path: Manage > Custom domain names > Add custom domain > Enter same domain name > Verify. Then, save the value of 'Destination or points to address'. The verification will fail at this point, as we will need to configure our DNS Zone in Azure: DNS Zones > Create DNS Zone > Enter domain as Name. Once the DNS Zone has been deployed, add a record set, and enter the value recorded for 'Destination or points to address' of the custom domain configuration. Additionally, I navigated to Ionos and added the remaining name servers listed in Azure, which is neccessary for the Azure AD domain to verify. 
<br/>
<br/>
<img src="https://i.imgur.com/GM6epIi.png" alt="Azure Custom Domain Name"/>
<img src="https://i.imgur.com/aB3gDqp.png" alt="Create DNS Zone"/> 
<img src="https://i.imgur.com/jnFbk4G.png" alt="Deployment Complete"/>
<img src="https://i.imgur.com/9TAVXBj.png" alt="Add Record Set"/>
<img src="https://i.imgur.com/WOOni8r.png" alt="Configure Name Servers"/>
<br/>
<br/>
Once the configuration is complete, navigate to 'All Users' in Microsoft Entra ID. Your Active Directory users should now be migrated! 
<br/>
<br/>
<img src="https://i.imgur.com/LL3FVoM.png" alt="Configuration Complete"/>
<img src="https://i.imgur.com/kM9BMKA.png" alt="Migration Complete"/>
<br/>
<br/>
<h2>Key takeaways:</h2>
Credentialed scans are essential for identifying vulnerabilities in a system, as they allow Nessus to access and scan all parts of the system, including system files, registry entries, and application settings. This results in a more comprehensive and accurate assessment of the system's vulnerabilities.
<br/>
<br/>
Deprecated software is a major source of vulnerabilities, making it crucial to keep all software up to date, including your operating system, browsers, and applications. Deprecated software often contains known security vulnerabilities that have been addressed in newer versions. While vulnerability remediation does often involve applying patches and updating software, it may also require changing system configurations. Nessus scans can be used to identify these vulnerabilities and remediate them. 
<br/>
<br/>
Regularly scanning your systems on an ongoing basis is also crucial to address any new vulnerabilities that may appear. The vulnerability management lifecycle is a powerful tool used to ensure that vulnerabilities are addressed continuously: 
<br/>
<br/>
In the planning stage of vulnerability management (Stage 0), any stakeholders and/or resources that will be involved should be identified. Guidelines and metrics outlining any goals or expectations should also be provided. In this project, the target system was a Windows 10 virtual machine, and the goal was to gain an overview of how vulnerability management works. 
<br/>
<br/>
In this project, the target system was a Windows 10 virtual machine, and the goal was to understand how various configuration changes to both the Windows 10 machine and Nessus can result in varying scan results. Another goal was to explore different vulnerability remediation techniques.
<br/>
<br/>
In the asset discovery phase (Stage 1), an inventory of all hardware and software assets should be created and/or updated regularly. It is common to use specialized solutions for asset management. This stage also involves scanning the assets for vulnerabilities. In this project, the primary asset was the Windows 10 virtual machine, and Nessus Essentials was used as the vulnerability scanner. 
<br/>
<br/>
Stage 2 of the vulnerability management lifecycle is vulnerability prioritization, where the vulnerabilities discovered in Stage 1 are prioritized based on their severity level. This stage focuses on maximizing efficiency. The prioritization is often based on threat intelligence sources such as CVE or CVSS, and may also be based upon how critical the particular asset is, and how likely it is to be exploited. In this project, the prioritization was primarily based on the severity levels provided by Nessus Essentials. 
<br/>
<br/>
Stage 3 involves vulnerability resolution, which involves remediation, mitigation, or acceptance. Remediation may involve patching software or making changes to system configurations, and essentially removing the vulnerability from the system entirely. Mitigation either lessens the severity of the vulnerability or makes it more challenging to exploit, but does not remove it from the system. For vulnerabilities that are less severe or not as high of a priority, they may just be accepted rather than being remediated or mitigated. This project focused on remediation primarily, however it is still crucial to understand mitigation and acceptance when working on larger scale systems in an organization. 
<br/>
<br/>
Stage 4, or the verification and monitoring phase, involves scanning the systems again to ensure that their remediation and mitigation efforts were effective. This is also to ensure that there were no new vulnerabilities created while making any changes to the system. 
<br/>
<br/>
The final stage, reporting and improvement (Stage 5), involves documenting the vulnerabilities that were found, as well as their outcomes. This stage focuses on the most recent scans and actions taken in the life cycle, as vulnerability management is an ongoing process. It is very important to have clear, up-to-date, and comprehensive documentation, particularly when working with stakeholders.

<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
