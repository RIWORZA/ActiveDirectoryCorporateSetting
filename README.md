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
- <b>Windows Server 2019</b>

<h2>Project Walkthrough</h2>

<p align="center">
Step 1: First we will want to create VM through Microsoft Azure.  Here is the VM I created and the public IP used.  Note that my VM used I made it to be as discoverable as possible which is not recommended.  This VM will act as a Honeypot and demonstrate RDP request from potential attackers.<br/>
<img src="https://i.imgur.com/z3DhzbK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 2 Once the VM is created we will want to turn off Windows Firewall.  Once again this is not recommended but is applicable for the purpose of this project.<br/>
<br/>
<img src="https://i.imgur.com/cXR5w19.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br/>
Step: 3 Next we will want to create the powershell script to run.  The purpose of this script to run with a Geolocation API and then log all this data to a file.  This file we will use later for Log Analytics in Azure to run queries on.  Note  this script is from an open source resource.<br/>
<img src="https://i.imgur.com/nrnoEp3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 4 As stated before in order for this script to work we will link it with a Geolocation API.  I created an account with ipgeolocation and used free version of the API they provide.  Note that this API is open source and can consume up to 1,000 API requests per day.
<img src="https://i.imgur.com/GtDIhQM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 5 After successfully using correct API key this is how the script will run.  We will leave this script running in the background as logs are gathered and written to file.<br/>
<img src="https://i.imgur.com/15Qn9YG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 6 While this script runs the logs are being saved to C:ProgramData\failed_rdp.log file.  We will eventually link this file to Log Analytics in Azure so we can extract the raw data which we will eventually use with Sentinel.<br/>
<img src="https://i.imgur.com/Z8d6NxG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br/>
Step: 7 Here we see the raw data that is being output to the file.  Eventually, we will need to create custom fields  with Log Analytics to extract n order for Sentinel to utilize the data in a readable format.<br/>
<img src="https://i.imgur.com/RlGiRe1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Step: 8 Here we are creating a custom log in Azure to pull the file that was created in step: 6 <br/>
<img src="https://i.imgur.com/WIAjQHf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br/>
Step: 9 Next we will run Log Analytics Query with custom log and added fields so we train Log Analytics to separate fields for Sentinel.  Bascially we are training the Log Analytics to read the raw data being colletected in the log file and outputting it in a format that Sentinel can use.<br/>
<img src="https://i.imgur.com/ed29dlI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
Next we will setup Sentinel so it uses the query we just created.  As you can see the heatmap takes all the seperate fields we have trained Log Analytics to parse from the failed RDP logs and makes a visual representation using Latitude and Longitude data.  For this project I let it run for several hours and as you can see there are different attackers from the entire globe that are trying to access this VM.  Note this is only pooling data from RDP requests, and there are probably many more attacks that are occuring.<br/>
<br/>
<img src="https://i.imgur.com/5kaLnlU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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

