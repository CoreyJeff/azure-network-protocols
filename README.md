<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1 - Create Resource Groups
- Step 2 - Create Virtual Machines
- Step 3 - Download Wireshark In VM
- Step 4 - Add Rule in Network Security Groups tab in Azure
- Step 5 - Connect To VM2 from VM1 Powershell

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 1: From the home page click on Resouce Groups. You should be in the resource groups tab. Now you need to click (Create new resource group). Under Subscription, there should be a blank space to name your group. Name it (RG-Lab). And in the Region tab under it put (US West 3) 
because for some reason when you put another region it doesn't show up in your resource groups. After that, you should be set for the resource group now click (Review + Create).
</p>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 2: Go back to the home page and head to the Virtual Machines tab. Once you are there click Create in the upper left corner. Then click Azure Virtual Machine. Under subscription you want to make the resource group the one you just made which is
(RG-Lab). There should be an option. Your Virtual Machine name will be (VM1). Region will stay (US West 3) Security Type change to Standard. Image (Windows 10 Pro 21H2) For size minimum should be (2 vcpu 16 GiB memory). Username is Labuser then create your own password. When you're finished check the "I confirm" box at the bottom and then (Review + Create).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 3: You are going to make another virtual machine so go back to the Virtual Machines Tab and create new.
Under resource group change it to your (RG-Lab) group. Under Virtual Machine Name you can call it (VM2). 
Leave the Region as (US West 3). Now you will change your Image to (Ubuntu Server 20.04 LTS). For size minimum should be (2 vcpu 16 GiB memory).
Change your Authentication Type from SSH Key to Password. Use (labuser) as your Username and don't forget your password. Now (Review + Create) the VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4: Now that you are done with the prerequisites go to the Virtual Machines tab and open (VM1) using remote desktop. So copy your Public IP in (VM1) and search
Remote Desktop in your start menu paste it and connect. Enter your username and password you made at the beginning of the lab and connect and accept the warning.
</p>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 5: In the VM download Wireshark and open it. Click the blue logo in the upper right corner to start scanning. Under the blue logo in the search bar type icmp and enter. Minimize your VM go to VM2 and copy the private IP. Now go back into your Remote Desktop Connection
and open Powershell from the start menu. Use the (ping) command to get the Reply ex(labuser> ping 10.0.0.5). You should be able to see the Replies on your Wireshark. Now we will constantly ping the IP by using the same command but adding (-t) at the end ex(labuser> ping 10.0.0.5 -t)
and we will manually change the firewall on VM2 to not allow ICMP traffic.
</p>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 6: Minimize VM1 and in Azure go to Network Security Groups. Go to the (VM2-nsg) and click Inbound Security Rules on the left side. Click Add at the top of the page to add a new rule. Change the Protocol to ICMP and the Action to Deny. Set the Priority to 200 and Name it (DENY_ICMP_FROM_ANYWHERE) and click add.
Go back to VM1 in your Remote Desktop and open Wireshark and it should say *Request timed out*. Minimize the VM and on your actual desktop go back to the Network Security Groups tab and allow ICMP traffic again by clicking on the rule you made and switching it back to allow and refresh. Open your VM1 again and your replies should start coming through on Wireshark again. To stop the replies click on the Powershell window and press Ctrl+C.	
</p>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 7: Lastly we will explore SSH traffic. First, in Wireshark go to the search bar change it from ICMP to SSH, and click the green logo above the bar to refresh. In Powershell we are going to use the (ssh) command and VM2s IP address to connect to VM2 ex(labuser> ssh labuser@10.0.0.5)
If you did it right you will get a finger-printing message and you want to type (yes) and enter the password to VM2. Keep in mind you won't be able to see the password so make sure you get it right. If you completed the steps correctly you should have a connection to VM2.
So whenever you type a command in Powershell you will see the traffic being sent between the two VMs on Wireshark. example use the id command (labuser@VM2:~$ id). To exit the connection simply type (exit) and enter and the session will be closed.
</p>
