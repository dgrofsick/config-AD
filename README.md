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


<h2>Setting up Resources in Azure</h2>

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

<h2>Setting Up Active Directory</h2>

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

<br />

<h3>Creating an Admin and Normal User Account in Active Directory</h3>

- In <b>Active Directory Users and Computers (ADUC)</b>, create an <b>Organizational Units (OU)</b> called <b>_EMPLOYEES</b> and <b>_ADMINS</b>
  - Mouse over to <b>Tools</b> and select <b>Active Directory Users and Computers</b>
  - Right-click <b>mydomain.com</b> on the left, hover over <b>New</b>, select <b>Orginizational Unit</b>
 
![image](https://github.com/dgrofsick/config-AD/assets/148154704/72f38f0c-7e51-41e0-a7d4-e3ba79b4fc60)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/6d0d1a4f-9d19-4e8e-8513-6ab9f9cd6ee5)

<br />

- Create a new employee named <b>Jane Doe</b> with the username of <b>jane_admin</b> with same password used earlier <b>(Password1)</b>
  -  Right-click <b>_ADMINS</b>, hover over <b>New</b>, select <b>User</b>
  -  Leave <b>[ ] User must change password at next logon</b> unchecked

![image](https://github.com/dgrofsick/config-AD/assets/148154704/3a3cf9fe-3008-4b71-91f1-9c9a0e91a120)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/bf222a02-16eb-45bc-aabf-433bfa49b25f)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/466752d2-ace3-4d3d-97ea-b042d9118137)

<br />

- Add <b>jane_admin</b> to the <b>Domain Admins Security Group</b>
  - Right-click <b>Jane Doe</b> and select <b>Properties</b>
  - Within the Properties menu, click the <b>Member Of</b> tab and click <b>Add</b>
  - Check names for "domain", select <b>Domain Admins</b> click <b>OK->Apply->OK</b>

<b>Note: Once a user is created, they must be assigned this way to offcially be added to an intended group.</b> 

![image](https://github.com/dgrofsick/config-AD/assets/148154704/70875bb9-78e7-4902-bc85-1d5fab5c156c)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/f2027610-6bbd-478a-921e-b6acf78565ac)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/91188ac7-3936-41b7-8467-d884ed732d58)

<br />

- Log out of <b>DC-1</b> and log back in as <b>mydomain.com\jane_admin</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/579ccf4e-bcf5-4830-88d3-2b1be4626a00)

<b>Note: User jane_admin will be used as your admin account from now on within this exercise.</b>  

<br />

<h3>Joining Client-1 to Your Domain</h3>

- From the Azure Portal, set <b>Client-1’s</b> DNS settings to the <b>Domain Controller’s Private IP address</b>
  -  Go to Azure and copy <b>DC-1's private IP</b>
  -  Go to <b>Client-1</b>, click on <b>Networking</b> on the left, select the network interface link <b>client-1771_z1<b> (odds are your interface link will be different but use the below image to properly locate it)
  -  Click <b>DNS servers</b> on the left, select the <b>Custom</b> option and enter/paste <b>DC-1's private IP</b>, and select <b>Save</b>

<b>Note: by doing the above steps, Client-1 will be recognized within DC-1's network</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/2ccdf32f-77ff-4aa0-abd4-ade9485da87c)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/23a73fdd-4493-4b26-ab59-64ea1ac1f03e)

<br />

- From the Azure Portal, restart <b>Client-1</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/ab1c3d16-b82b-46fb-adf2-cb3439fb7dea)

<br />

- Login to <b>Client-1</b> via Remote Desktop as the original local admin (mydomain.com\ADuser) and join it to the domain
  -  Right-click the Windows icon and select <b>System</b>
  -  Click <b>Rename this pc (advanced)</b> on the right side
  -  Select <b>Change</b>
  -  Click the <b>Domain</b> bubble, enter <b>mydomain.com</b> and click <b>OK</b>
  -  Use the log in info created in the beginning (mydomain.com\jane_admin - Password1.) to allow access to this admin user. 

![image](https://github.com/dgrofsick/config-AD/assets/148154704/9e558e6f-d559-4ef1-808d-693d639c061e)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/abdccd09-b77a-491f-98ef-d33b664d3edb)

<br />

- Log in to the <b>Domain Controller</b> via Remote Desktop and verify <b>Client-1</b> shows up in Active Directory Users and Computers (ADUC) inside the Computers folder

![image](https://github.com/dgrofsick/config-AD/assets/148154704/09b727ef-2220-4a81-8222-7c0a69ce0b82)

<br />

- For organnization purposes, you can create a new organizational unit called <b>_CLIENTS</b> and drag <b>Client-1</b> to it from the Computers folder


![image](https://github.com/dgrofsick/config-AD/assets/148154704/5fe6c86c-14e4-4674-ae3a-641c36d3627a)

<br />

<h3>Setting up Remote Desktop for Non-Admin Users on Client-1</h3>

- Log into <b>Client-1</b> as <b>mydomain.com\jane_admin</b> and open system properties

![image](https://github.com/dgrofsick/config-AD/assets/148154704/0ab6fbd9-ba94-4011-a8fc-db625928ba8b)

<br />

- Click <b>Remote desktop</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/6f7940f2-a81a-4843-978c-5438277cc54c)

<br />

- Allow “domain users” access to remote desktop
  -  From within the remote desktop menu, click <b>Select users that can remotely access this PC</b>
  -  Click <b>Add</b>, check names for "domain users", and click <b>OK</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/c6900d8c-1431-4456-a73b-0c6393e6a8bd)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/3994ce1b-3700-47b7-9e4a-f49faa222f99)

<b>Note: You can log into Client-1 as a normal, non-administrative user now.  Normally you’d want to do this with a Group Policy that allows you to change MANY systems at once, but for demonstration purposes, we are bypassing those methods for now.</b>

<br />

<h3>Creating Additional Users to Log into Client-1</h3>

- Login to <b>DC-1 as jane_admin</b>
- Open <b>PowerShell_ise as an administrator</b>
  -  In the Windows search bar, type "powershell"
  -  Right click <b>Windows Powershell ISE</b> and select <b>Run as adminstrator</b>

![image](https://github.com/dgrofsick/config-AD/assets/148154704/7162d1a5-addf-4faa-b3b5-43ec06bd7409)

<br />

<b>Note: for convenience, open the following link https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1, that was graciously provided</b>

- Create a new file and paste the contents of the script below into it (be sure to use the link above)
  -  Within Powershell, first click the <b>New File</b> button in the top left
  -  Click the above link, locate the <b>Raw</b> button and click it (for raw code contents of the script provided on the page)
  -  Highlight the entire script and copy

![image](https://github.com/dgrofsick/config-AD/assets/148154704/ce17d304-6066-4910-9a92-c7d05002dbe2)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/3ad5ac55-5725-4e4b-84c4-16d7d3003ea7)

<br />

-  Paste the copied code into the blank white space within Powershell
    - Notice within the top of the code:
    - -  $PASSWORD_FOR_USERS   = "Password1"
    - -  $NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
    - Note the password for any and all users created with this script will be <b>Password1</b>
    - In addition the current amount of users created by this script is defaulted to 10,000.  For time's sake, we will edit that number to 100
- Run the code using the 'green play' button highlighted in the image below and notice the user accounts being created
![image](https://github.com/dgrofsick/config-AD/assets/148154704/ff8eae24-86d0-4714-9771-898c748578cc)

<br />

- When finished, open ADUC and observe the accounts in the appropriate OU

![image](https://github.com/dgrofsick/config-AD/assets/148154704/f7216fff-e055-4a1a-809b-f6e968369b84)

<br />

- Attempt to log into Client-1 with one of the accounts (remember the password in the script: <b>Password1</b>)
  - The user names created from the provided code will be completely random, so during the time of doing this demonstration on my own end, I opted to choose <b>"bobo.tutek"</b>.  Feel free to select any of the provided names you end up with for this step.
 
![image](https://github.com/dgrofsick/config-AD/assets/148154704/4c0de094-ad38-4c22-b3ac-2376050c249a)
![image](https://github.com/dgrofsick/config-AD/assets/148154704/8cbbd46f-b6e4-40b1-8d95-86e2fc6a668a)

- Congratulations!  We just completed setting up an example of Active Directory with a pool of random users to log in with.  From here on while this system is still running, you can log in as whoever you choose.  If any errors occur, such as failed log in attempts, the jane_admin account can be used to address that issue by right-clicking that account from within ADUC's users folder and changing any required settings/permissions (i.e. resetting the user's password or unlocking the account from too many failed login attempts.)
