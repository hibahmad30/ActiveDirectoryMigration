<h1>On-Premise AD Migration to Microsoft Entra ID</h1>

<h2>Description</h2>
Migrating to Microsoft Entra ID offers several advantages, including enhanced security, improved scalability, and centralized management of identities. This cloud-based IAM solution enables organizations to manage user access to resources more efficiently, enforce robust security policies, and ensure compliance with industry standards. By transitioning from an on-premise Active Directory to Microsoft Entra ID, organizations can leverage advanced features such as multi-factor authentication, conditional access policies, and seamless integration with other Microsoft cloud services.
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
The migration from on-premise Active Directory to Microsoft Entra ID represents a significant step towards modernizing IT infrastructure. This project is crucial because it aligns with the growing trend of cloud adoption, which offers numerous benefits over traditional on-premise setups. By moving to Microsoft Entra ID, organizations can achieve greater flexibility and scalability, allowing them to respond more quickly to changing business needs. The cloud-based nature of Entra ID ensures that identity management services are available anytime and anywhere, supporting a remote and mobile workforce.
<br/>
<br/>
In this project, I successfully migrated an on-premise Active Directory (AD) environment to Microsoft Entra ID (formerly known as Azure AD). The primary goal was to leverage cloud-based identity and access management (IAM) solutions to enhance security, scalability, and manageability of user identities and resources. This migration involved setting up an Active Directory server, promoting it to a Domain Controller, creating an organizational structure, and finally migrating this setup to Microsoft Entra ID.
<br/>
<br/>
The project commenced with the creation of a virtual machine in Azure using Windows Server 2019. This virtual machine served as the foundation for our Active Directory server. By navigating the Azure portal, I provisioned resources within a Resource Group, named the virtual machine instance, and set administrative login credentials. The setup included adding essential roles like Active Directory Domain Services and DNS server, which are crucial for promoting the server to a Domain Controller. The root domain 'activeshiftentra.com' was established using the Ionos service and configured as the new forest in AD.
<br/>
<br/>
Post configuration of the AD, I structured the organizational units (OUs) to simulate a typical corporate environment. This involved creating users, groups, and provisioning various devices such as computers, laptops, and servers. Additionally, I implemented Group Policy Objects (GPO) to enforce security settings across all OUs. For instance, the Default Domain Controller password policy was adjusted to enhance password security, which was crucial for maintaining a secure and compliant IT environment.
<br/>
<br/>
The final and critical phase of the project was migrating the on-premise AD to Microsoft Entra ID. This was facilitated by installing Microsoft Entra Connect on the server instance, which linked the on-premise AD to the cloud-based Entra ID. The domain 'activeshiftentra.com' was registered and verified in Azure. This step required configuring DNS settings in Azure DNS Zones and updating name servers through Ionos to ensure proper domain verification. Once the configuration was complete, all users from the on-premise AD were successfully migrated to Microsoft Entra ID, thus enabling centralized cloud-based identity management.

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
