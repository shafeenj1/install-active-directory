<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Setting up Active Directory in Azure</h1>
This lab walks through how I set up Active Directory in Microsoft Azure. It serves as the baseline for future labs. I’ll be working with two virtual machines on the same virtual network (vNet). In this lab, the focus is on one VM, which will host and be configured as the domain controller. The second VM will act as a client machine and will be joined to the domain in a later lab. <br />

<h2>Platforms and Technologies Used</h2>

- Microsoft Azure
- Active Directory
- Remote Desktop


<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro 

<h2>Setup Instructions</h2>

<p>
<img width="3494" height="1628" alt="image" src="https://github.com/user-attachments/assets/2f8934d4-5606-4317-aabb-cb07b0024d1c" />

</p>
<p>
Before working with the virtual machines, you need to assign a static IP address to the domain controller VM. By default, both VMs use dynamic IPs, which can prevent them from properly communicating with each other even if they’re on the same virtual network. If this isn’t changed, the client VM won’t be able to join the domain you’ll set up later. To fix this in the Azure portal, go to the domain controller VM and open the Networking section. From there, select the network interface, navigate to IP configurations, and change the assignment from dynamic to static. Save the update once you’re done. Setting a static IP ensures the domain controller has a consistent address that other machines can reliably use during configuration.
</p>
<br />

<p>
<img width="1544" height="765" alt="image" src="https://github.com/user-attachments/assets/9824b09b-ac29-48aa-8b41-34385e8092be" />
<img width="3492" height="1784" alt="image" src="https://github.com/user-attachments/assets/fd281f30-de72-41af-94cb-1fd4c595d567" />

<img src="https://i.imgur.com/IbIJWiC.png" height="80%" width="80%" alt="Installation Steps"/>
</p>
<p>
After setting the static IP, it is time to log in to the client VM and see if there is connectivity to the domain controller. Using ping -t (domain controller private ip address), will show that the connection is being timed out. On the domain controller VM, we need to enable ICMPv4 on the local Windows Firewall. Within the search bar, type wf.msc to open Windows Defender Firewall. Click on Inbound Rules and enable the Core Networking Diagnostics - ICMP Echo Request rules. Returning to the client VM will show that the ping is now resolving without errors.
</p>
<br />

<p>
<img src="https://i.imgur.com/ktSoNmA.png" height="80%" width="80%" alt="Installation Steps"/>
</p>
<p>
It is now time to install Active Directory on the domain controller VM. With Server Manager open, click on Add Roles and Features and click Next. Confirm the private IP address of the domain controller VM. In the Server Roles tab, click on Active Directory Domain Services. Click Add Features, click Next, then Install. Next we have to promote the server into a domain controller. In Server Manager, there is a warning sign in the top right corner under a flag. Click on that flag and click Promote this server to a domain controller. Click on Add a new forest and specify a domain name. In my case, I will use ernestotest.com. Specify a password for the domain and click on Next on each screen and Install.
</p>
<br />

<h2>An Important Note </h2>

When logging back in to the domain controller VM through Remote Desktop Connection, it is important to log in with the context of the domain. Type out the domain path and then the name of the user. For example: mydomain.com\labuser. In my case, it is ernestotest.com\labuser. Now that Active Directory is installed, configurations can be implemented in future labs and the client VM will be able to join the domain that was created.
