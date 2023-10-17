![image](https://github.com/itnatepena/install-ad/assets/147539410/5b97511d-7550-48c8-aca5-949d6097c8e2)


<h1>Deploying Active Directory in Azure</h1>
This guide outlines the process I followed to set up Active Directory in an Azure environment, serving as the groundwork for future projects. I'm utilizing two Azure Virtual Machines, both residing on the same virtual network (vnet). This specific project focuses on configuring the first VM as the domain controller, while the second VM will join this domain in a future lab.

## Project Summary

This guide outlines the process of setting up Active Directory in an Azure environment, laying the groundwork for future projects. The key components of this project include the following:


- **Environments Used:** Azure (Virtual Machines/Compute).
- **Technology/Applications/Services Used:** Microsoft Azure, Remote Desktop, Active Directory Domain Services.

## Technologies Used

- **Operating Systems:** Windows Server 2022, Windows 10 Pro.

<h2>Installation Steps</h2>

<p>
Before proceeding, it's crucial to configure a static IP address for the domain controller VM. By default, VMs with dynamic IPs may encounter communication issues, which can impact future domain-joining operations. To set a static IP, follow these steps on the Azure portal:
</p>

<p>
1. Click on the Networking tab of the domain controller VM.
2. Access the Network Interface and open the IP configurations tab.
3. Toggle the Assignment switch to "Static" and save your changes.
  
![image](https://github.com/itnatepena/install-ad/assets/147539410/36508d6a-446a-4d14-a08a-ce4bfef43040)
</p>

<p>
Setting a static IP ensures a stable reference for future configurations.
</p>

<p>
With the static IP configured, let's verify connectivity between the VMs. Log in to the client VM and check if it can communicate with the domain controller. Use the "ping -t (domain controller private IP address)" command to test connectivity. If the ping times out, proceed to enable ICMPv4 on the domain controller's Windows Firewall:
  
![image](https://github.com/itnatepena/install-ad/assets/147539410/57843d02-0519-4dcd-aff7-a0d8a5f230d1)

</p>

<p>
1. Type "wf.msc" in the search bar to open Windows Defender Firewall.
2. Go to Inbound Rules and enable the "Core Networking Diagnostics - ICMP Echo Request" rules.
  
![image](https://github.com/itnatepena/install-ad/assets/147539410/344ab8bf-278f-475e-a73d-66dca11d5c69)

</p>

<p>
After this adjustment, the ping from the client VM should begin to work without issues.
  
![image](https://github.com/itnatepena/install-ad/assets/147539410/97159bc5-c5c8-4452-a16a-7e5af7940381)

</p>

<p>
Now, it's time to install Active Directory on the domain controller VM:
</p>

<p>
1. Open Server Manager and click on "Add Roles and Features."
  
![image](https://github.com/itnatepena/install-ad/assets/147539410/c2e8dc6b-4623-414a-bd68-a11668eca7a6)

2. Confirm the private IP address of the domain controller VM.
3. In the Server Roles tab, select "Active Directory Domain Services" and click "Add Features."
![image](https://github.com/itnatepena/install-ad/assets/147539410/8450a43a-4f3c-48a1-a384-ae6b3b2c4566)
4. Follow the on-screen instructions, then click "Install."
![image](https://github.com/itnatepena/install-ad/assets/147539410/068484bc-3a9e-4802-93df-d5ba0efa361d)

</p>

<p>
After installation, promote the server to a domain controller by clicking on the warning sign in the top right corner of Server Manager and selecting "Promote this server to a domain controller." Configure a new forest with a domain name (e.g., "natepena.com"), set a domain password, and complete the setup.
  
![image](https://github.com/itnatepena/install-ad/assets/147539410/42feb3e0-3cf4-40c0-966e-8f2ae6578d1f)

![image](https://github.com/itnatepena/install-ad/assets/147539410/39edc380-c79d-40e7-b3f5-4f016249f7d4)

Congrats, at this point, you should have Active Directory installed. 
You can <a href="https://github.com/itnatepena/configure-ad">click here</a> to continue on to my Active Directory Configurations project.

</p>

<h2>Important Note</h2>

When logging into the domain controller VM via Remote Desktop Connection, remember to log in within the context of the domain. For example, use "natepena.com\someuser" or, you can also try something like "someuser@natepena.com". Now that Active Directory is in place, it sets the stage for future configurations, and the client VM can be seamlessly joined to the created domain.
