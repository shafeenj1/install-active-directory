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

<img width="3472" height="1792" alt="image" src="https://github.com/user-attachments/assets/0cf98b26-536f-4605-8f13-5e3ae76e8250" />

</p>
<p>
After assigning the static IP, the next step is to log into the client VM and check connectivity to the domain controller. If you run ping -t using the domain controller’s private IP address, you’ll notice the requests are timing out. To fix this, go to the domain controller VM and allow ICMPv4 traffic through the Windows Firewall. Open the search bar, type wf.msc, and launch Windows Defender Firewall. From there, navigate to Inbound Rules and enable the rules labeled Core Networking Diagnostics - ICMP Echo Request. Once that’s done, go back to the client VM and run the ping command again, you should now see successful responses instead of timeouts.</p>
<br />

<p>
<img src="https://i.imgur.com/ktSoNmA.png" height="80%" width="80%" alt="Installation Steps"/>
</p>
<p>

  
Now it’s time to set up Active Directory on the domain controller VM. Open Server Manager, select Add Roles and Features, and proceed through the initial prompts by clicking Next. Make sure the VM’s private IP address is correct.
When you reach the Server Roles section, choose Active Directory Domain Services, then click Add Features, followed by Next, and finally Install.
After the installation finishes, you’ll need to promote the server to a domain controller. In Server Manager, look for the notification flag with a warning icon in the top-right corner. Click it and select Promote this server to a domain controller. Choose Add a new forest, enter your desired domain name (for example, ernestotest.com), and set a Directory Services Restore Mode (DSRM) password. Continue clicking Next through the remaining steps, then click Install to complete the process.
</p>
<br />

<h2>A Key Point </h2>

When reconnecting to the domain controller VM using Remote Desktop, make sure you sign in using the domain account format. Enter the domain followed by the username, like: mydomain.com\labuser. In this case, it would be ernestotest.com\labuser. With Active Directory now set up, you’ll be able to apply configurations in upcoming labs, and the client VM can be joined to the newly created domain.
