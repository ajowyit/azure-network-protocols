<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this project, we explore network traffic between two Azure Virtual Machines using Wireshark and PowerShell. We analyze communication patterns to better understand how data flows within a virtual network. Additionally, we demonstrate how to configure Network Security Groups (NSGs) to control inbound and outbound traffic, showcasing how they enhance the security and management of cloud-based resources. <br />

---

<h2>⚠️ Prerequisites</h2>

- [Creating Virtual Machines in Azure](https://github.com/ajowyit/creating-virtual-machines/)


<h2>💻 Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>👨‍💻 Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>🪜 High-Level Steps</h2>

- Step 1: Connect to the virtual machine using Remote Desktop Protocol (RDP) and Install Wireshark
- Step 2: Observe the ICMP Traffic
- Step 3: Use a Network Security Group (NSG) firewall to deny ping
- Step 4: Observe the SSH and DHCP traffic
- Step 5: Observe the DNS and RDP traffic

<h2>Actions and Observations</h2>

<h3>Step 1: Connect to virtual machine using RDP & Install Wireshark</h3>
<p>

<img width="1500" alt="Screenshot 2025-05-26 at 11 16 53 PM" src="https://github.com/user-attachments/assets/6008b209-9a30-4b66-a941-7959a1e2d425" />

<img width="1500" alt="Screenshot 2025-05-26 at 11 32 17 PM" src="https://github.com/user-attachments/assets/984cc7f0-1126-4ae8-899d-04ab8b0eba08" />

</p>
<p>
- Log into your Azure account. Navigate to Virtual Machines. Start the virtual machines if they are not already on/running. Select both virtual machines by checking the box and then click the "Start" button. Azure will ask, "Do you want to start all the selected VMs?" Click "Yes". Make sure to take note of the Public IP for the windows-vm.
</p>

<p>
- You'll need to wait a bit for the virtual machines to be up and running. Make sure that the virtual machines are be up and running before moving on by checking Status.
</p>
<br />

<p>
 <img width="1017" alt="Screenshot 2025-05-26 at 11 37 43 PM" src="https://github.com/user-attachments/assets/2781674d-ef24-4871-89e2-17071e05cdff" />

</p>

<p>
- Open Remote Desktop (Windows) or the Windows App (MacOS). For this walkthrough, I'm using the Windows App because I have a Mac. The Windows App can be found in the App Store and downloaded from there. Once you open the Windows App, click the "+" button at the top right. Then, select "Add PC" from the dropdown options.
</p>
<br />
<p>
 <img width="521" alt="Screenshot 2025-05-26 at 11 40 06 PM" src="https://github.com/user-attachments/assets/33bb040e-a18c-4d64-b3f2-bf033b6f067d" />

 </p>
<p>
- Enter the Public IP address for the windows virtual machine that we took note of earlier in "PC name".  If you forgot to take note of it, go back to Azure and locate it on the main Virtual Machines page. For Friendly name, I used "windows-vm" for clarity and simplicity. Click "Add".   
</p>
<br />

<p>
 <img width="1000" alt="Screenshot 2025-05-26 at 11 50 12 PM" src="https://github.com/user-attachments/assets/9f591580-8189-404c-9645-655d5a06db45" />
</p>

<p>
- Enter the Username and Password for the windows-vm that we created in Azure. Click "Continue".
</p>
<br />


<p>
- Now we're connected to the windows-vm in Azure from our Home Lab using RDP! Open the virtual machine's browser, "Microsoft Edge". Go to www.wireshark.org to dowload Wireshark on the virtual machine using default settings. We will use this Wireshark to observe network traffic with various protocols.
</p>
<br />

<p>
<img width="1700" alt="Screenshot 2025-05-27 at 12 17 24 AM" src="https://github.com/user-attachments/assets/1fad2c2d-e7af-4740-819c-6f3c349a1dc0" />

<img width="1500" alt="Screenshot 2025-05-27 at 12 23 20 AM" src="https://github.com/user-attachments/assets/9280aa79-53d0-4711-a228-2febd0b08995" />

</p>


<p>
- Once on Wireshark's website, select "Windows x64 Installer" to download the installer .exe file. Then go to your downloads, open the file and finish the Wireshark setup. Keep clicking "Next" and "Finish" until you complete the setup.
</p>
<br />

<h3>Step 2: Observe ICMP Traffic</h3>
<p>
 <img width="748" alt="Screenshot 2025-05-27 at 12 51 32 AM" src="https://github.com/user-attachments/assets/0eb44072-f0cb-4b90-a0bd-2127481970a8" />
</p>

<p>
- Open Wireshark. 
- Make sure "Ethernet" is highlighted
- Click on the shark fin in the top left of the screen to start capturing network traffic.
</p>
<br />

<p>
<img width="751" alt="Screenshot 2025-05-27 at 1 20 11 AM" src="https://github.com/user-attachments/assets/ba9ce63a-1d85-4ff3-b888-0743e1c38fbb" />
<img width="750" alt="Screenshot 2025-05-27 at 12 54 52 AM" src="https://github.com/user-attachments/assets/db9412a1-8ee4-4ca9-ac15-3b06298df572" />
</p>
  
<p>
- We can see all the traffic running over the network in the purple shaded area.
</p>

<p>
- At the Filter Bar at the top of the srceen, type "icmp" and press enter or click the arrow icon to apply the filter. 
</p>

<p>
- We will use the icmp filter to isolate ICMP traffic.
</p>
<br />

<p>
 <img width="1500" alt="Screenshot 2025-05-27 at 1 23 53 AM" src="https://github.com/user-attachments/assets/e6767870-7e44-4ea1-b80d-5409a4e13a38" />
</p>

<p>
- In Azure, select the linux-vm and click Networking to find the Private IP address. It should be 10.0.0.5. Go back to the windows-vm.
</p>
<br />

<p>
 <img width="796" alt="Screenshot 2025-05-27 at 1 29 27 AM" src="https://github.com/user-attachments/assets/c7c893a2-1392-4182-8953-fe82293379ba" />
</p>

<p>
- On the windows virtual machine, click the Windows button at the bottom left of you screen. Search for PowerShell and open it. We are going to use commands in PowerShell to ping the linux-vm from the windows-vm.
</p>
<br />

<p>
<img width="1583" alt="Screenshot 2025-05-27 at 1 50 04 AM" src="https://github.com/user-attachments/assets/0cf8cc2d-3a44-48d6-add3-53fb4587b405" />

 <img width="1567" alt="Screenshot 2025-05-27 at 1 55 15 AM" src="https://github.com/user-attachments/assets/9c6ea52c-f098-4433-85a2-12e8af4cff04" />

</p>

<p>
- In PowerShell, enter the command ping 10.0.0.5 to ping the linux-vm. Since the icmp filter is active in Wireshark, we will only seeing icmp traffic over the network. This lets us see our windows virtual machine (10.0.0.4) send the request to the linux virtual machine (10.0.0.5) and the linux virtual machine send a reply back to the winows-vm. We have just successfully tested the connection between the two virtual machines.  
</p>

<p>
- Next, in PowerShell, initiate a perpetual ping to the linux-vm by entering the command ping 10.0.0.5 -t. This tells the windows virtual machine to ping the linux virtual machine non-stop. Observe all of that traffic
</p>
<br />

<h3>Step 2: Use NSG (Firewall) to Deny Ping</h3>
<p>
<img width="1500" alt="Screenshot 2025-05-27 at 2 45 55 PM" src="https://github.com/user-attachments/assets/a5c3cab7-69b1-464a-8c29-8dbf995b9545" />
</p>

<p>
- Now we will explore Network Securiy Group functions and set a new security rule for the linux-vm. In Vitural Machines, click the linux-vm, select Network settings, and then click "linux-vm-nsg" under Network security group.
</p>
<br />

<p>
<img width="1500" alt="Screenshot 2025-05-27 at 2 49 26 PM" src="https://github.com/user-attachments/assets/52473a99-38e2-4d1f-a27b-51b9154f631f" />
</p>

<p>
- Next, select Inbound security rules. Click the "+Add" button to create a new rule. We are going to tell the Firewall to Deny inbound traffic to the linux-vm from ping. Change Source to "Any". Make sure there's an asterisk in both port ranges (asterisk = all). Select "ICMPv4" for the Protocol since ping uses ICMPv4. For Action, select "Deny". Change the Priority to "290" which will make our new rule first priority. Finally, add the rule to the NSG by clicking the "Add" button.
</p>
<br />

<p>
<img width="1500" alt="Screenshot 2025-05-27 at 3 01 48 PM" src="https://github.com/user-attachments/assets/64155970-dd23-4287-a632-79f929bebf13" />
</p>

<p>
- Now our new Security Rule has been created. Go back to the windows-vm.
</p>
<br />

<p>
 <img width="1498" alt="Screenshot 2025-05-27 at 3 28 58 PM" src="https://github.com/user-attachments/assets/c56c64e5-94c4-4c9d-abee-c1cd08e65432" />
</p>

<p>
- Observe how the ping request(s) time out in PowerShell because of the new rule we created in the Network Security Group. 
</p>
<br />

<p>
<img width="1500" alt="Screenshot 2025-05-27 at 3 40 38 PM" src="https://github.com/user-attachments/assets/a8e85b7f-b238-4525-8483-96a6df877a8c" />

<img width="1587" alt="Screenshot 2025-05-27 at 3 42 45 PM" src="https://github.com/user-attachments/assets/c0d1d35c-9920-4032-ab15-b4895edae0ac" />
</p>

<p>
- Go back and delete the security rule we created. Click the trash can to the right of the rule. Click "Yes" to confirm. Pings should no longer time out, go back to the windows-vm and observe PowerShell and the network traffic in Wireshark. Notice how Wireshark shows requests and replies again and PowerShell is showing replies from 10.0.0.5 again!
</p>

<br />

<h3>Step 3: Observe SSH and DHCP Traffic</h3>
<p>
 <img width="1624" alt="Screenshot 2025-05-27 at 4 05 38 PM" src="https://github.com/user-attachments/assets/5d197381-36e8-4e71-8b23-07bc10a97643" />
<img width="1620" alt="Screenshot 2025-05-27 at 4 11 49 PM" src="https://github.com/user-attachments/assets/934bf7e1-d655-4378-bc82-b17431188561" />
</p>

<p>
- Now let's observe SSH Traffic. In Wireshark, type ssh into the filter bar and start a new network traffic capture. 
</p>
<p>
- In PowerShell, we will create a secure connection to the linux-vm using the command ssh username@PrivateIP.(username and Private IP of the linux-vm) Example = ssh ajowyit@10.0.0.5 and press enter. 
</p>
<p>
- It will ask if are you sure. Type yes and press enter again. Then enter the password of the linux-vm. As you type in the password, it will remain blank. Type in the password and press enter.
</p>
<p>
- We now have a secure and encrypted connection between the windows-vm and linux-vm, notice the encrypted packets in Wireshark!
</p>
<p>
- Observe the outputs in PowerShell with the id, hostname, and uname -a commands.
</p>


<br />

<p>
 <img width="1621" alt="Screenshot 2025-05-27 at 4 30 04 PM" src="https://github.com/user-attachments/assets/f731f6e6-1760-4572-96a9-0a4c7efb0b41" />

</p>

<p>
- We are done observing SSH traffic. Type the command exit and press enter in PowerShell to close the ssh connection to the linux-vm. Next, let's observe DHCP traffic. 
</p>
<br />

<p>
<img width="850" alt="DHCP1" src="https://github.com/user-attachments/assets/93a7227e-1e04-405f-b917-50a6e20c8efa" />

<img width="850" alt="DHCP2" src="https://github.com/user-attachments/assets/f29464d7-627b-4950-a884-708625174fe0" />
</p>

<p>
- Dynamic Host Configuration Protocol (DHCP) is a protocol used to assign dynamic IP addresses to devices. In Microsoft Azure, virtual machines are assigned reserved dynamic IPs via DHCP. 
</p>

<p>
- When we release an IP address manually with the command ipconfig /release, it can disconnect our RDP session. In order to circumvent this, we will create a batch script that will automate release and renew operations.
</p>

<p>
- Type dhcp into the filter bar of Wireshark and start a new capture. Open Notepad in the windows-vm and write:

ipconfig /release
ipconfig /renew
</p>

<p>
- So, Figure 25. What is this all about? Well, we can't use the command ipconfig /release in PowerShell because we will loose our connection to the windows-vm. We came here to see some DHCP Traffic, so lets create some. Open the Notepad on the windows-vm. Type in, ipconfig /release and ipconfig /renew like you see in Figure 25. Now, save that in C:\ProgramData as dhcp.bat and click save. Now lets hop back over to PowerShell.
</p>
<br />
<p>
<img width="850" alt="DHCP3" src="https://github.com/user-attachments/assets/2f816a07-96d7-48d7-ab6f-7721220eb7e9" />

<img width="850" alt="DHCP4" src="https://github.com/user-attachments/assets/5a2880b1-eb24-4b33-a4f7-7c3eb7634b43" />
</p>

<p>
- In Powershell, we are going to change directories with the command cd c:\programdata and press enter. Now, list what files are in this directory with the command ls and press enter. Next, run our bat file, dhcp.bat, that we just created with the command .\dhcp.bat and press enter. See Figure 26.
</p>

<p>
- You will see the ipconfig /release and the windows-vm will disconnect for a second, but it will fire back up pretty quick once the windows-vm renews the IP. Now take a look at Wireshark and PowerShell in Figure 27. We can see the actual proccess in action! DHCP traffic as promised.🤯 Big shout out to Josh Madakor for this little trick. 🫡
</p>
<br />

<h3>Step 5: Observe DNS and RDP Traffic</h3>
<p>
<img width="850" alt="DNS1" src="https://github.com/user-attachments/assets/0ad0edf0-a43d-46e9-b477-b33e00639b76" />

<img width="850" alt="DNS2" src="https://github.com/user-attachments/assets/5aa050be-bde6-4f25-852c-1df42b6894bd" />
</p>

<p>
- Time for some DNS traffic. In the Wireshark filter bar, type in dns and start a new capture. Over in PowerShell, type in the command nslookup disney.com and press enter. PowerShell shows us the an IP for Disney. Lets copy that IP, take it the browser, and see what we get when we run a search. See Figure 28. 
</p>

<p>
- What is this? 🤨 Even Wreck-it-Ralph looks confused. We can see that the IP, 130.211.198.204, is clearly related to Disney somehow. Notice the Disney logo by the IP address? Here are a couple things to consider. 1) We probably are not authorized to see what this IP really is. 2) DNS protocol translates human-readable domain names into machine-readable IP addresses. Very few websites can be accessed by typing the IP address directly into the address bar of a browser. Still, pretty cool stuff. 
</p>
<br />

<p>
<img width="850" alt="RDP1" src="https://github.com/user-attachments/assets/7a4c3d8b-2ce0-441c-89ce-e45520c51cad" />

<img width="850" alt="RDP2" src="https://github.com/user-attachments/assets/e2370e30-1b5e-492b-bbf1-cb2b80173138" />
</p>

<p>
- Last but certainly not least. Lets look at some RDP traffic and switch it up a bit. Filter the traffic by the Port RDP uses. In the filter bar, type tcp.port == 3389 and start a new capture. Notice all the packets just zooming through? We saw this same thing at the beginning of this project. We have been running a virtual machine using RDP (Remote Desktop Protocol) this entire time! There is traffic constantly flowing over the network while we are connected to the windows-vm. Also, you can filter in Wireshark with rdp in the filter bar. See figure 31. 
</p>

<h2>✅ Conclusion</h2>

<p>
This concludes our project. We sucessfully connected to our windows-vm using RDP and observed a ton of network traffic with various protocols using Wireshark and PowerShell. I highly recommed hitting the YouTube and checking out more about both. The more tools you are familiar with, the better. Hey, we even dipped our pinky toe into the vast ocean of Cybersecurity by creating a new security rule to deny inbound traffic. Maybe that was a stretch, but still fun stuff.</p> 
<p>Hopefully, you learned something that you didn't know before and possibily had a little fun at the same time. I know Wreck-it-Ralph did. Don't forget to Stop (turn off) the VMs in Azure. As always, Thank You for your time and viewing this Project. We'll see you on the next one!😎      
</p>
<br />





