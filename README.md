<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Begin by deploying two Azure virtual machines—one for the Active Directory Domain Controller (Windows Server 2022) and another for a client system (Windows 10). Configure each VM with a static private IP address. Using Remote Desktop, log into the Windows Server VM and install the role for the Active Directory Domain Services (AD DS) through Server Manager. Promote the server to a domain controller, specifying a new root domain name (e.g., ad.yourdomain.com) during the wizard process. After the server reboots, the Active Directory will be ready for initial configuration.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Then, configure the DNS services on the domain controller so that clients can resolve internal names. Set the Windows 10 DNS settings to point to the domain controller's IP address. Join the Windows 10 machine to the domain by navigating to System > About > Rename this PC (Advanced) and entering the domain name you created earlier. Upon successful domain join, reboot the client and verify connectivity and authentication with Active Directory by logging in using domain credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
At last, use the console for Active Directory Users and Computers on the domain controller to establish not only user accounts but also organizational units. Use PowerShell or Server Manager where necessary to configure group policies that determine such things as password requirements, desktop backgrounds, and the installation of software. Then, verify that all accounts and groups work as they should by using a new domain user to log into the Windows 10 VM.
<br />
