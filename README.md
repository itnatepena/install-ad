<h1>Deploying Active Directory in Azure</h1>
This guide outlines the process I followed to set up Active Directory in an Azure environment, serving as the groundwork for future projects. I'm utilizing two Azure Virtual Machines, both residing on the same virtual network (vnet). This specific project focuses on configuring the first VM as the domain controller, while the second VM will join this domain in a future lab.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>Installation Steps</h2>

<p>
Before proceeding, it's crucial to configure a static IP address for the domain controller VM. By default, VMs with dynamic IPs may encounter communication issues, which can impact future domain-joining operations. To set a static IP, follow these steps on the Azure portal:
</p>

<p>
1. Click on the Networking tab of the domain controller VM.
2. Access the Network Interface and open the IP configurations tab.
3. Toggle the Assignment switch to "Static" and save your changes.
</p>

<p>
Setting a static IP ensures a stable reference for future configurations.
</p>

<p>
With the static IP configured, let's verify connectivity between the VMs. Log in to the client VM and check if it can communicate with the domain controller. Use the "ping -t (domain controller private IP address)" command to test connectivity. If the ping times out, proceed to enable ICMPv4 on the domain controller's Windows Firewall:
</p>

<p>
1. Type "wf.msc" in the search bar to open Windows Defender Firewall.
2. Go to Inbound Rules and enable the "Core Networking Diagnostics - ICMP Echo Request" rules.
</p>

<p>
After this adjustment, the ping from the client VM should work without issues.
</p>

<p>
Now, it's time to install Active Directory on the domain controller VM:
</p>

<p>
1. Open Server Manager and click on "Add Roles and Features."
2. Confirm the private IP address of the domain controller VM.
3. In the Server Roles tab, select "Active Directory Domain Services" and click "Add Features."
4. Follow the on-screen instructions, then click "Install."
</p>

<p>
After installation, promote the server to a domain controller by clicking on the warning sign in the top right corner of Server Manager and selecting "Promote this server to a domain controller." Configure a new forest with a domain name (e.g., "natepena.com"), set a domain password, and complete the setup.
</p>

<h2>Important Note</h2>

When logging into the domain controller VM via Remote Desktop Connection, remember to log in within the context of the domain. For example, use "natepena.com\someuser" or, in my case, "natepena.com\labuser." Now that Active Directory is in place, it sets the stage for future configurations, and the client VM can be seamlessly joined to the created domain.
