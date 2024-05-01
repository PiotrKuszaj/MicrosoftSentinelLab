<h1>🗺️ Microsoft Sentinel GeoMapping</h1>

<h2>📃 Description</h2>
In this Project, I set up a virtual machine using Windows 10 to act as a honey pot. The goal of this is to extract metadata from the Windows Event Viewer using a PowerShell script and forward the information to a third-party API so Microsoft Sentinel can map the locations of login attempts around the world on a dashboard like this: <br/><br>
<p align="center"><br>
  <img src="https://i.imgur.com/XEBpdoV.png" height="80%" width="80%" alt="" align="center"/>
<br />


<h2>👓Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Azure Log Analytics Workspace</b>
- <b>Microsoft Sentinel</b> 

<h2>💻 Environments Used</h2>

- <b>Windows 10 Pro Virtual Machine</b>

<h2>🔎Program walk-through:</h2>

<p align="center">
Create a Virtual Machine<br>
In the Networking tab, "NIC network security group" should be set to advanced and create a new group:<br/>
<img src="https://i.imgur.com/6BYgXik.png" height="80%" width="80%" alt=""/>
<br />
<br />
Get rid of the default rules<br>
Create new Inbound Rule as follows:  <br/>
<img src="https://i.imgur.com/LG5FmJ2.gif" height="80%" width="80%" alt=""/>
<br />
<br />
Save and create your virtual machine after saving the inbound rule<br>
Next Create a Log Analytics Workspace:  <br/>
<img src="https://i.imgur.com/WSyEao1.png" height="80%" width="80%" alt=""/>
<br />
<br />
While everything else is being created, let's setup our Microsoft Defender for Cloud:  <br/>
<img src="https://i.imgur.com/IOKy82Y.gif" height="80%" width="80%" alt=""/>
<br />
<br />
Back to Log Analytics Workspace, connect the LAW to the Virtual Machine:  <br/>
<img src="https://i.imgur.com/JSHpzgj.gif" height="80%" width="80%" alt=""/>
</p>

<p align="center">
Create Microsoft Sentinel and connect it to your workspace:<br/>
<img src="https://i.imgur.com/13tpMWn.png" height="80%" width="80%" alt=""/>
<br />
<br />
Get your VM IP and connect via remote desktop connection<br>
Use the credentials you used when you created the VM:  <br/>
<img src="https://i.imgur.com/QjNwoGP.gif" height="80%" width="80%" alt=""/>
<br />
<br />
Turn Off Firewall in the VM: <br/>
<img src="https://i.imgur.com/hnnEDRJ.gif" height="80%" width="80%" alt=""/>
<br />
<br />
Next, I downloaded a <a href="https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1">PowerShell Script</a> that uses a Third Party <a href="https://ipgeolocation.io">API</a><br>
Sign up and replace with your API at the top of the downloaded PowerShell script:
  <br/>
<img src="https://i.imgur.com/F1a9qqE.png" height="80%" width="80%" alt=""/>
<br />
<br />
In PowerShell ISE run the script, make sure to replace with your API at the top of the script:  <br/>
<img src="https://i.imgur.com/NQYu3qk.gif" height="80%" width="80%" alt=""/>
<br />
<br />
When the script is run, it will update with the failed login attemps as shown below<br>
Logs are stored in C:\ProgramData\failed_rdp by default<br>
Make sure you can see hidden files in the "view" tab: <br/>
<img src="https://i.imgur.com/r5yyCBh.gif" height="80%" width="80%" alt=""/>
<br />
<br />
Back in Azure Portal, we will create a New custom log<br>
Log Analytics Workspace ➜ Tables ➜ New custom log (MMA-based):<br/>
<img src="https://i.imgur.com/LJpOfVl.png" height="80%" width="80%" alt=""/>
</p>

<p align="center">
Download the <a href="https://github.com/PiotrKuszaj/MicrosoftSentinelLab/blob/main/failed_rdp.txt">Sample Log</a><br>
Upload the file to the custom log that was just created<br>
In Collection Paths tab, set Windows path to where the failed_rdp file is on your VM<br>
Default path will be C:\ProgramData\failed_rdp.log <br/>
<img src="https://i.imgur.com/hUHnzB6.gif" height="80%" width="80%" alt=""/>
<br />
<br />
In Microsoft Sentinel, we create a new workbook:  <br/>
<img src="https://i.imgur.com/CbS7i1r.gif" height="80%" width="80%" alt=""/>
<br />
<br />
Add a <a href="https://github.com/PiotrKuszaj/MicrosoftSentinelLab/blob/main/Query.txt">Query</a><br>
If we call the log table we created, it shows us our logs<br>
Parse to get the information we need such as state and country:  <br/>
<img src="https://i.imgur.com/tA8LCRX.gif" height="80%" width="80%" alt=""/>
<br />
<br />
We can visualize this information further by creating a Map<br>
Editing filters such as Time Range:  <br/>
<img src="https://i.imgur.com/zffhaYv.gif" height="80%" width="80%" alt=""/>
<br />
<br />
A map is made! It can be adjusted to auto refresh:<br>
It will update with the failed login attempts onto the virtual machine and display locations on the map:<br/>
<img src="https://i.imgur.com/SHo7hy5.png" height="80%" width="80%" alt=""/>
</p>
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray

