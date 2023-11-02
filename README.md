# <h1>Active Directory in a Corporate Setting</h1>


<h2>Description</h2>
Here I setup a lab enviroment replicating a corporate environment with  Domain controller and client connecting to DC.  I created two VMs, one acting as the Domain controller and the other acting as a client connecting to access the internet.I will be using Windows Server 2019 and Windows 10 desktop iso files in VirtualBox.  On the server we will have two seperate NICs one acting as a NAT and the other as the internal network that the client will be connecting to.  The client will be accessing the sever through remote access and route through the server using DHCP to access the internet.   Finally we will also be running a Powershell script to add 1000 generic users in Active Directory that the client can utilize to sign into under.
<br />


<h2>Languages and Utilities Used</h2>

- <b>VirtualBox</b> 
- <b>PowerShell</b> 
- <b>Active Directory</b>
- <b>Remote Access</b>
- <b>DHCP</b>


<h2>Environments Used </h2>
- <b>Windows 10</b>
<br/>
- <b>Windows Server 2019</b>

<h2>Project Walkthrough</h2>

<p align="center">
Step 1: First we will download the two iso files we will be using they can be found at https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019 and https://www.microsoft.com/en-us/software-download/windows10<br/>
Step 2: We will begin with setting up Windows Server 2019 on VirtualBox.  Here are the settings that I have for mine.  Please note that I did need to add two different adapters in the network setting in VirtualBox.  One will be for the server to access the internet and the other for clients joining the server.
 <br/>
<img src="https://i.imgur.com/e7xB1yx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 2 On boot we will run the installer and choose Windows Server 2019 Standard.  If you choose Datacenter we will only have a command line to work with and no GUI. <br/>
<br/>
<img src="https://i.imgur.com/0Bwv0Jo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br/>
Step: 3 Last thing before starting the installation we will choose custom installation and run all defaults after that.<br/>
<img src="https://i.imgur.com/OZf361v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 4 After the server is booted I then make changes on the internal adapter.  For this step I named both adapters so there is no confusion moving forward.  I will be using the internal adapter with DHCP and Remote Access Management so clients can join the Domain.  I also made changes to the IPv4 settings for the internal adapters settings. This will be used later when clients join the domain and are assigned IP address by DHCP.<br/>
<img src="https://i.imgur.com/bQ4y5af.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 5 Next we will want to name the server so we can identify it.  The reason for this is that there can be a number of servers in an organization.  Things like file servers, application servers etc. it is also good to label and document your setting.  Since this server is going to be our Domain Controller I will be naming it DC for simplicity.<br/>
<img src="https://i.imgur.com/jd21fcT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 6 At this point we will begin adding the necessary features so that this server can act as a Domain Controller and manage our users through Active Directory.  The first thing we will want to do is add the feature Active Directory Domain Services.<br/>
<img src="https://i.imgur.com/oneyHqs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br/>
Step: 7 During the configuring of this feature we will want to configure the settings for the DC including the naming convention and if we want to create a new forest, or add a domain to an existing forest, in this case we are creating a new forest.<br/>
<img src="https://i.imgur.com/2IDtzzH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 8 Once I completed this I then rebooted the VM and as you can see by the default sign in it is now registering as a domain. <br/>
<img src="https://i.imgur.com/wwuvZNR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br/>
Step: 9 As a best practice for security we will want to create a separate Admin account for ourselves and any other users that require admin access.  The reason we do this step is for non-repudiation.  When changes are made on the server we will want to be able to track who did that change.  If we only have one admin account and multiple people access the server through it, how can you tell who did what in the server.  This gives accountablity to all users that sign into the server.  Here I created an OU as Admins and added a new user to that OU.<br/>
<img src="https://i.imgur.com/MAx6MsY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step 10 In order for clients to communicate with this server I will also add the Remote Access feature.<br/>
<br/>
<img src="https://i.imgur.com/G9L1KIX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 Step: 11 Next we will need to make changes to the DHCP settings.  Basically in order for clients to connect to the internet we want them going through the Domain Controller.  DHCP will then assign IPs and lease them to Clients based on the designed scope (IP range).<br/>
<img src="https://i.imgur.com/Iu8wEcc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step 12 In order for clients to communicate with this server I will also add the Remote Access feature.<br/>
<br/>
<img src="https://i.imgur.com/G9L1KIX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
 Step: 13 As shown here there are currently no Address leases as there is no client connection to the server.<br/>
<img src="https://i.imgur.com/oYA4Uxj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step 14 Next I will add about a 1000 users to Active Directory through a Powershell script.  This script will run and generate all the user based on a .txt document.<br/>
<br/>
<img src="https://i.imgur.com/CMFHibI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 15 After running the script we can now check Active Directory Users and Computers and see all the users that were added.<br/>
<img src="https://i.imgur.com/MAx6MsY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step 16 Next we will setup the Windows 10 VM.  We will do the standard setup and boot the VM.  Note when choosing which Windows I selected Windows 10 Pro as Windows 10 Home is unable to join Domains. After boot we will want to join it to the Domain we created on the server.<br/>
<br/>
<img src="https://i.imgur.com/am0XnsJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
Step 17 Finally after the Client is joined to the domain any user we setup on Active Directory will be able to sign in and access their desktop with the correct credentials.  Note that if we take a look back at the address leases in DHCP on the DC we can see that an address has been assigned to the client that was just joined to the DC.<br/>
<br/>
<img src="https://i.imgur.com/XFdcYUa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
That concludes this project.  We created to VMs one using Windows Server 2019 and Windows 10 Pro.  We configured the Server to act as a Domain Controller and setup two adapters so the device can communicate with the internet and clients on the network.  We also setup DHCP to lease out IP address to clients that are joined.  Finally we created 1000 users in Active Directory and connected a client to the DC.  Anyone using that client can connect to their desktop given they have their credentials.
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

