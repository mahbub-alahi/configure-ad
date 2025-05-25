<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://youtu.be/nwUHPELRXdM)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://imgur.com/kWoFIKf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Resource Group called "Active-Directory-Lab".
</p>
<br />

<p>
<img src="https://imgur.com/EwdRDkn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/jamyOKY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
As your resource group is being deployed, create the virutal network we will be using for the virtual machines to connect to. Make sure to add the resource group to this Virtual Network.
</p>
<br />

<p>
<img src="https://imgur.com/JbWP3dM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/NxBqyiQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a virtual machine called "DC-1", make sure to use a size of at least 2 vcpus and 8 GiB memory, MAKE SURE to use the "Image" Windows Server 2022 Datacenter: Azure Edition - x64 Gen2, because this will be our "Domain Controller".
</p>
<br />

<p>
<img src="https://imgur.com/OWd03AW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/32CaW9Q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
MAKE SURE you have the VM on the resource group we created, same with the Virtual Network, we will do the same for "client-1"s VM.
</p>
<br />

<p>
<img src="https://imgur.com/PV3Ycvb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/yGyU56h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/MzicBV5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a VM called "client-1", we will be using the "image" Windows 10 Pro for client 1, Adding it to the "Active-Directory-Lab" Resource Group, as well as the "Active-Directory-VNet" Virtual Network we created.
</p>
<br />

<p>
<img src="https://imgur.com/jtANPQ0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/v9XJNC1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After VM is created, Set Client-1's DNS settings to DC-1's Private IP Address, First we have to make sure the PRivate IP address of DC-1 wont change by setting the IP address from "Dynamic" to "Static". by clicking on DC-1's VM -> Network settings (left side) -> Click on the "Network Interface/IP Configuration" and set the IP configuration from Dynamic to Static, Press save.
</p>
<br />

<p>
<img src="https://imgur.com/GcfJQNA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/nt1VkDB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/u14vXaA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/lQxZ5g3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/B2VDboJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
Now that we set DC-1's Private IP address to static, we will copy the IP Address of DC-1 and go to client-1's->Network Settings->Click on "Network Interface/IP Configuration"-> On the left side youll see "DNS Servers" setting->Click Custom and Paste DC-1s Private IP Address from before. Click Save and you will see that Client-1 Inherited DC-1's Private IP address. Restart Client-1 for the effects to take place.
</p>
<br />

<p>
<img src="https://imgur.com/MOMVGyo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/C4dTWyv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/UNyoaLg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will now copy the Public IP address for client-1 and paste it into RDP so we can confirm the changes through the command line by running "ping (DC-1's Private IP Address)", After confirming all packets got sent through, Run ipconfig /all and you can see at the bottom next to "DNS servers" the Private IP address of DC-1.
</p>
<br />

___

<h4>Part 1 of Deploying Active Directory</h4>

<p>
<img src="https://imgur.com/cAiOdnZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the right corner where you see the caution sign, click on it and click "Promote this server to a domain controller"
</p>
<br />

<p>
<img src="https://imgur.com/5DvSrDG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/HynR4mH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/IYnRKlp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/7BpwFG2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/ndCMywm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click "Add roles and Features"-> "Add a new forest" -> and specify the Root domain name: "mydomain.com" -> For the domain Controller Options for the (DSRM) password, type in "Password1" -> Uncheck "Create DNS Delegation" -> For Server Roles Check "Active Domain Directory Servers" Then click next Until you can Install.
</p>
<br />

<p>
<img src="https://imgur.com/fyJVGoC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logout of DC-1 and log back in as "mydomain.com\labuser"
</p>
<br />

<p>
<img src="https://imgur.com/LyzloNY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/66vKM7D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/E36IGJu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/baln2cV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open up Active Directory Users and Computers -> Under mydomain.com -> Create a new Organizational Unit called "_EMPLOYEES" -> Create a new Organizational Unit called "_ADMINS" -> Create a new user named "Jane Doe" under mydomain.com/_ADMINS -> for the user logon name, use "jane_admin"-> Create Password and only check "Password never expires".
</p>
<br />

<p>
<img src="https://imgur.com/PQTwQGo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/bB879s3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Under Jane Doe Properties, go to "Member Of" -> Add -> Type in "Domain Users" -> "Check Names" -> and click "OK" -> Apply
</p>
<br />

<p>
<img src="https://imgur.com/ppt6vgx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logout on DC-1 and log back in as "mydomain.com\jane_admin". Use jane_admin as your admin account from now on.
</p>
<br />

<p>
<img src="https://imgur.com/wCz2b2C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/9W3k1Xl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/qEzPPqr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/hW6NloF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in as client 1 -> Right click start menu -> System -> Rename this PC (advanced) -> Click domain -> Type in "mydomain.com" -> Press OK -> a Prompt will show up asking for DC credentials -> Type in the credentials we just used to log into DC-1. You will get a prompt welcoming you to the domain and to restart your PC.
</p>
<br />

<p>
<img src="https://imgur.com/NNRJSWp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/rZDyZeb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into DC-1 and verify that Client-1 is on Active Directory Users and Computers by going to ADUC -> mydomain.com -> Computers and you will see client-1. Create a new Organizational Unit called "_CLIENTS" and click and drag client-1 into the new _CLIENTS OU. 
</p>
<br />

___

<h4>Part 2 of Deploying Active Directory (Creating Users with Powershell)</h4>


<p>
<img src="https://imgur.com/qgsSoj3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/gEOnpKx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/5Gm1Myf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/R01frzb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/YzlFevx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setup Remote Desktop for non-administrative users on client-1. Log into Client-1 as mydomain.com\jane_admin -> Right Click start menu -> System -> Remote Desktop -> "Select users that can remotely access this PC" -> Add -> type "Domain Users" into the text box and press OK and OK.
</p>
<br />

<p>
<img src="https://imgur.com/BtIVc7F.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open up WindowsPowerShellISE as an administrator so we can run this script.
</p>
<br />

<p>
<img src="https://imgur.com/YCaramP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
  
Open up this [Script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) -> Copy Raw File

</p>
<br />

<p>
<img src="https://imgur.com/TRDV8nA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/rMFdukn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/PHjWJ4C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
In PowerShellISE create a new script and save it onto your desktop as "create.users". Paste the script into powershell and press the green start button at the top to let it run.
</p>
<br />

<p>
<img src="https://imgur.com/zk1ARGc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/IyUjZT6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
As this script is running we can confirm its going into the "_EMPLOYEES" OU by going to ADUC in DC-1 and Clicking on _EMPLOYEES. Log in as one of the users we created, the password of all the users is "Password1".
</p>
<br />

___

<h4>Part 3 Group Policy and Managing Accounts</h4>

<p>
<img src="https://imgur.com/oVUllQC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/Egn9p5W.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/eLFYIeY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/anPnM6e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Get logged into DC-1 and run gp.msc -> In the drop down go to mydomain.com and right click on "Default Domain Policy"-> Edit -> Computer Configuration -> Software Settings -> Windows Settings -> Name Resolution Policy -> Security Settings -> Account Policies -> Click Password Policy.
</p>
<br />

<p>
<img src="https://imgur.com/82Yw3VH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/J1Tjuk8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/F51IQTh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/MS5WPVl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set the account lockout threshold to 5 invalid logon attempts, the account lockout duration to 30 minutes, and the account lockout counter to 10 minutes.
</p>
<br />

<p>
<img src="https://imgur.com/6XzpJ0B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/HisG6d3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/ce0QYH5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to client-1 as mydomain.com\jane_admin and use CMD as an administrator and run "gpupdate /force", Logout client-1.
</p>
<br />

<p>
<img src="https://imgur.com/Vp0gZAJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/A7V9rIg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login Client-1 as "mydomain.com\bat.novo", use the incorrect password for 5 times until you see the security prompt.
</p>
<br />

<p>
<img src="https://imgur.com/TLENRrn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/7qx6EMG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go into DC-1's ADUC, click mydomain.com -> _EMPLOYEES -> Locate the users account we locked out and you can see that theres a setting with a checkbox to unlock users, Unlock the user and apply. 
</p>
<br />

<p>
<img src="https://imgur.com/AxnCrSA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will be able to login as "bat.novo" to double check we logged in as the user we can go to CMD-> and run "whoami". This confirms that we are on the bat.novo account.
</p>
<br />

<p>
<img src="https://imgur.com/eVp5k0j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/mFN8gP9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To reset passwords within ADUC we can go back to our _EMPLOYEES and right click on a user. You can see reset password and that will be the way to configure your users password and password settings.
</p>
<br />

<p>
<img src="https://imgur.com/bPfoPAL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/0wCfad6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Disabling an account goes through almost the same process by right clicking on a user and clicking "Disable Account". You can tell its disabled from the little icon to the left showing a down arrow. 
</p>
<br />

<p>
<img src="https://imgur.com/WVrOJP8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/bsoc3ly.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/GsxNbRp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/SEZYNNn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will open up client-1, Run eventvwr.msc as administrator, a prompt screen will show up asking for the credentials for a domain admin, use mydomain.com\jane_admin as  -> Custom Views -> Windows Logs -> Security -> Click the Find Action on the right side -> search "bat.novo" and look for the 5 Audit failures, these are the failures from when we put into the password wrong 5 times.
</p>
<br />
