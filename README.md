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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
