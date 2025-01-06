<p align="center">
<img src="https://i.imgur.com/aMMGyHQ.jpeg" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />

<h1>Preparing Active Directory Infrastructure in Azure</h1>



<h2>Description</h2>
In this project, I created two VMs (Virtual machines), one running a Windows Server, to act as a domain controller. The other VM will act as a client, running Windows 10, and join the domain. In later projects, I will deploy AD, run a script that will create users in the domain, which I can log into from the client VM, then manage the accounts and update the group policies, all to simulate a real-life IT environment!  <br/>
<br />


<h2>Environments and Utilities Used</h2>

- <b>Microsoft Azure</b>
- <b>Virtual Machines</b>
- <b>Remote Desktop Connection</b>


<h2>Operating Systems Used </h2>

- <b>Windows Server </b>
- <b>Windows 10</b>

<h2>Project Walk-through:</h2>

<p align="center">
Navigate to Microsoft Azure and create a resource group: <br/>
<img src="https://i.imgur.com/BcQ414M.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Next, create a virtual network like so: <br/>
<img src="https://i.imgur.com/QvTkERb.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Once my resource group and network are created, I'll create and set up the virtual machine acting as our Domain Controller. For the image, make sure you use Windows Server:  <br/>
<img src="https://i.imgur.com/Xk2DtU0.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/JiVXyJQ.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
In the Networking tab of this VM, I'll make sure it will create itself on the virtual network I just created. I'll leave all other settings default and create this VM: <br/>
<img src="https://i.imgur.com/ghqn7TI.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Now, I'll create another VM that will serve as the client. The image for this machine should be Windows 10, NOT Windows Server like I did for the previous machine:  <br/>
<img src="https://i.imgur.com/9hKyxBp.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/EBRXTaM.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
In the Networking tab of this VM, I'll make sure it will create itself on the same virtual network as the previous machine created. I'll leave all other settings default and create this VM:  <br/>
<img src="https://i.imgur.com/RYUn5oi.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
I now need to set our DC (Domain Controller) private IP address to "static," as by default, it is set to "dynamic." I want this to be static because this DC will double as a DNS (Domain Name System) server, which I will tell our client to use as a DNS server later. The IP address could change if the IP allocation setting were dynamic, leaving our client's DNS configuration invalid. So, I'll go to the network settings of the DC and switch the IP allocation to static:  <br/>
<img src="https://i.imgur.com/esZllT8.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/l0PmApR.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Next, I'll use Remote Desktop Connection to connect to the DC using its public IP and the login credentials I created when setting up this machine:  <br/>
<img src="https://i.imgur.com/OHCVLqt.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Once logged in, the following screen will appear with the Server Manager open. (If this isn't what you're seeing and instead it is a regular Windows desktop, you may have connected to the client VM instead or chose the wrong image when creating the DC):  <br/>
<img src="https://i.imgur.com/yVeXBtz.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Next, I will disable the firewall (you probably wouldn't do this in real life, but for the sake of this lab where nothing is at stake, I'll go ahead and do it). So, to disable the firewall, I'll right-click the "Start" button and select "Run." Then type "wf.msc":  <br/>
<img src="https://i.imgur.com/0ztwOZI.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Click on "Windows Defender Firewall Properties" then on the "Domain Profile," "Private Profile, and the "Public Profile" tabs, and turn the firewall state off:  <br/>
<img src="https://i.imgur.com/5luuIqk.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/uZJpkGP.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
<img src="https://i.imgur.com/3wiNDiy.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
You should see that all the firewall settings are disabled:  <br/>
<img src="https://i.imgur.com/wnHbVDn.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Next, I must configure our client's DNS settings to the DC. To start, back in Azure, I'll grab the DCs private IP address:  <br/>
<img src="https://i.imgur.com/OIzMsfO.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Then, I'll go to the network settings on the client machine. Click on the NIC (Network Interface Card), go to settings, then DNS servers, and switch from "Inherit from the virtual network" to "Custom." Input the DCs private IP here and save:  <br/>
<img src="https://i.imgur.com/ddrkcIQ.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
After that's saved, I'll restart the client machine:  <br/>
<img src="https://i.imgur.com/2C3Qi1q.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Once the machine has restarted, I'll use a Remote Desktop connection to connect to the client machine using its public IP and the login credentials I created while setting up this machine:  <br/>
<img src="https://i.imgur.com/ZgpSqPf.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
Now that I'm logged in, I will open Powershell and attempt to ping the DC using the ping command and its private IP address. In my case, it'll look like this. (If there is an error and the connection timed out, double-check in Azure to ensure both machines are on the same virtual network. If they aren't, this is likely causing the error, and you'll need to set up the machine again on the same network):  <br/>
<img src="https://i.imgur.com/5bEWedc.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />
While here, I can double-check that the DNS server settings are pointing to the DC. I'll run "ipconfig /all" and look for the "DNS Servers," and it should point to our DC if everything is set up properly:  <br/>
<img src="https://i.imgur.com/dgShrBB.png" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />
<br />

<h2>Active Directory Infrastructure is Now Prepared! </h2>

<b>We've successfully created two VMs (Virtual Machines), one running Windows Server, to act as a Domain Controller. The other VM is a client running Windows 10. Don't forget: In later projects, I will deploy AD, run a script that will create users in the domain, which I can log into from the client VM, then manage the accounts and update the group policies, all to simulate a real-life environment!  </b>
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
