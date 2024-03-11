<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Making the 2 virtual machines
- Changing IP address setting
- Fix so the ping gets a reply
- Making important organizational units
- Chaning the DNS server
- Generating employees as an example and logging in

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/bB5W7RM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this step I created 2 virtual machines and I named them "Client-1" and "DC-1"(domain controller). It's very important to make sure these 2 virtual machines have the same virtual network.
</p>
<br />

<p>
<img src="https://i.imgur.com/ZXpzGqd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Small step but an important step will be changing DC-1's private IP setting by going to DC-1-> networking setting-> network interface-> IP configuration then change it to static. To ensure the client and domain controller connectivity by enabling ICMPV4 on domain controller then ping it from client 1. Use command "ping -t ____private ip goes here. 
</p>
<br />

<p>
<img src="https://i.imgur.com/l2JnF7P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Since it is not getting a reply open domain controller ->windows firewall with advanced security-> inbound rules-> protocol-> enable rule for ICMP echo request and echo request. If this doesn't work just enable the another one which is right under the second one that needs to be enabled. Now it should reply. Install Active Directory on domain controller->server manager->add roles and features->get active directory domain services. Server manager->flag->"promote this server to a domain controller"->add new forest->rootname "mydomain.com"->NetBios name MYDOMAIN. After completing steps it will restart.
</p>
<br />

<p>
<img src="https://i.imgur.com/BNVPmRz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next step we're just kinda making a few important folders. DC-1->tools->active directory users and computers->mydomain.com to create an organizational unit called "_EMPLOYEES" and "_ADMINS". Make an admin account called "Jane Doe" in the "_ADMINS" folder. After that make whatever username and password you'd like but make sure to check off password never expires. Assign Jane Doe to be a domain admin. Jane Doe->proterties->member of->add->domain admin->check name. Members of "domain admin" can make changes to the domain now login as Jane Doe with the login credentials set.
</p>
<br />

<p>
<img src="https://i.imgur.com/7ralSia.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On azure set client-1's DNS settings to DC1's private IP. Client-1->network interface->DNS server->custom->enter->private IP in DNS server. Then go to client 1->start->system->rename this PC(advanced)->change->Domain:"mydomain.com"login as jane doe then restart computer. Now login as Jane admin ->system->remote->desktop->select users that can remotely access this this PC->enter "Domain users" then login as Jane Doe and then restart the computer. On DC-1 start->users and computers->mydomain.com->users->enter "Domain Users" then check names. Now everyone can access client-1. Next is going to be a test to see if it's true.
</p>
<br />

<p>
<img src="https://i.imgur.com/1KuVE5F.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To start testing to see if this works we are going to use a code to create users and then login with 1. Open powershelLise as an administrator after that you can see I used the code in the picture which only for practice purposes. Open new script then place the code in there and click run. Active directory users and computers->mydomain.com->_EMPLOYEES now you can see all the employees generated for this example now just pick one and save there login so you can login with them.
</p>
<br />
