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

- Set up Resources in Azure
- Ensure Connectivity Between the Client and Domain Controller
- Install Active Directory
- Join Client-1 to Your Domain (mydomain.com)
- Set up Remote Desktop for Non-Administrative Users on Client-1
- Create Additional Users and Attempt to Log into Client-1 with One of the Users

<h2>Deployment and Configuration Steps</h2>

<h3>Setting up Resources in Azure</h3>

- Create the Domain Controller VM (Windows Server 2022) named <b>DC-1</b> using the information below.

<b>Note: Keep the Resource Group and Virtual Network (Vnet) that get created at this time in mind.</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/0c3a7e80-c66b-4edd-82ed-4ef134f1b283)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/f4762546-1ad2-44a4-ba2f-62f565c42663)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/a9d69d77-640f-4697-bcdc-ea4a68e84717)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/1f5853b2-504b-493d-aac4-173ba03c5b86)

<p>

- Set Domain Controller’s NIC Private IP address to be static
  -  Go to <b>DC-1</b>, click <b>Networking</b> in the lefthand menu 
  -  Under <b>Network Interface</b> click 'dc- ____'
  -  Click <b>IP configurations</b> on the lefthand menu again
  -  Click <b>ipconfig1</b> and finally select the <b>static</b> bubble

</p>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/32033cc9-2cc0-449b-9706-ea4c61f5f807)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/db9b4ccd-5e4d-49ad-98f4-57fd6e192de6)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/5b1cc7df-4941-48cd-8fb8-2a84af59472a)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/4f6bbc7a-e137-46fc-a1fe-30a8b54f2b4a)

<br />

- Create the Client VM using Windows 10 (21H2) and name it <b>Client-1</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/22d620f0-6bd9-4ea5-86f1-7876f2bb89bc)

<br />

- Use the same Resource Group and Vnet that was used the in steps to create <b>DC-1</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/4bb65d31-7b95-41fd-8591-8ac8f0d66a0d)

<br />

- Ensure that both VMs are in the same Vnet

![image](https://github.com/dgrofsick/config-AD/assets/148154704/5974e849-c189-403d-9817-45d70e44b95f)

![image](https://github.com/dgrofsick/config-AD/assets/148154704/d6488ef8-d831-4f17-b51b-5fea14019cee)

<br />

<p>

<h3>Ensuring Connectivity</h3>

</p>

<p>

- Login to <b>Client-1</b> with <b>Remote Desktop</b> and ping <b>DC-1’s</b> private IP address with either <b>ping (ip address)</b> or <b>ping -t (ip address)</b> (perpetual ping)
  - Remember, when connecting to <b>Client-1</b>, the public ip address can be found by selecting it from either the <b>Resouce Groups</b> menu or the <b>Virtual Machines</b> menu within Azure.
  - Copy the the public ip address and type <b>Remote Desktop</b> into the Windows Search bar to locate and open the app.
  - Paste <b>Client-1's</b> ip address into the app
  - Select <b>Connect</b>, <b>Use a different account</b>, and enter the proper login credentials associated with <b>Client-1</b>

</p>

- Type <b>cmd</b> within search bar belonging to the <b>Client-1 vm</b> and open <b>Command Prompt</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/ca7d602c-f794-4221-bfb1-665a3bc96cbb)

<br />

<b>Note:</b> The private ip should be 10.0.0.4 assuming DC-1 was the first vm created.  If not, it may be 10.0.0.5
- As mentioned earlier, use either <b>ping (ip address)</b> or <b>ping -t (ip address)</b> for a perpetual ping request from <b>DC-1</b>
  - Notice upon entering the ping command, the request has been timed out (denied) each time.  The following steps will remedy that.

![image](https://github.com/dgrofsick/config-AD/assets/148154704/8c34afcc-5e96-4adf-a755-d6f877a828fe)

<br />

<p>

- Log in to the <b>Domain Controller</b> <b>(DC-1)</b> and enable <b>ICMPv4</b> on the local windows firewall (refer to the steps used to log into <b>Client-1</b>, but instead using <b>DC-1's</b> menus)
  -  In the seach bar, type <b>Windows Firewall</b> or <b>Firewall</b> and select <b>Windows Defender Firewall with Advanced Security</b>

<b>Note:</b> ICMPv4 allows for pinging to register with correspoinding ip addresses

![image](https://github.com/dgrofsick/config-AD/assets/148154704/f006a056-dbfb-4db3-8d91-b3b937f65876)

<br />

-  Select <b>Inbound Rules</b> from the lefthand menu, sort by Protocol to easily find ICMPv4 and enable pinging options from <b>Core Networking Diag. - ICMP Echo</b> (should be 2 options)
  - Right-click your selections and click <b>Enable Rule</b> 

![image](https://github.com/dgrofsick/config-AD/assets/148154704/aa29d19c-dbd4-4fd6-b8bc-3765ff5c1db5)

</p>

<p>

- Check back at <b>Client-1</b> to see the ping succeed

![image](https://github.com/dgrofsick/config-AD/assets/148154704/42896ac0-826c-4dbe-8c62-629c8f5c1606)

<br />

</p>

<h3>Installing Active Directory</h3>

- Login to DC-1 and install Active Directory Domain Services
  - Enter the <b>Server Manager</b> on <b>DC-1</b> (the program should already be open upon logging into <b>DC-1</b>)
  - Go to <b>Manage</b> in the top right and select <b>Add Roles and Features</b>
 
![image](https://github.com/dgrofsick/config-AD/assets/148154704/3616c29c-b6b0-4b26-aef6-e56258da02e4)

<br />

- Continue to click <b>Next</b> until the roles show up and find <b>Active Directory Domain Services</b> and intall

![image](https://github.com/dgrofsick/config-AD/assets/148154704/4aa3fb01-a963-434e-882b-94f2bf990687)

<br />

- Be sure to check <b>[ ] Restart the destination server automatically if required</b> as this step will be required as we continue along.

![image](https://github.com/dgrofsick/config-AD/assets/148154704/ca23ca49-ab99-4d82-b4cf-8fe613722c2f)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/6d81efda-1247-444b-bba5-e10fdfd23320)

<br />

- Promote <b>DC-1</b> as a domain controller
  - Click the <b>flag</b> in the top right and select <b>Promote this server to a domain controller</b>
  - Select <b>Add a new forest</b>, use the domain name <b>mydomain.com</b> (or whatever you choose) and hit <b>Next</b>
  - Use password: <b>Password1</b> (or whatever you choose) and click <b>Next</b> until installation is complete.

![image](https://github.com/dgrofsick/config-AD/assets/148154704/c911a5c8-f2f9-442c-920d-d0f2c0f9f063)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/5082c8a3-2ce3-407b-ac67-05a576f0d983)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/97b3c9b1-cd7c-4720-9335-14d60b51dcc9)

 
 <br />

- Restart <b>DC-1</b> and then log back in as user: <b>mydomain.com\ADuser</b>

<b>Note: Remember to use BACK SLASH '\' in your username NOT forward slash '/' </b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/d11801ba-34e8-47d8-948d-cc6e0e448262)
