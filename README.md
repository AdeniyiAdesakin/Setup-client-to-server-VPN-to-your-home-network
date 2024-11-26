<h1>Setup client to server VPN for Remote Access</h1>
<p>The objective of this task is to enable remote access to a Windows Server 2019 from a remote client machine using a VPN (Virtual Private Network).</p>
<br>

<h3>*Configure Windows Server 2019</h3>
<p><b>Install Remote Access and select Direct Access and VPN (RAS)</b> </p>
<P>1. From the server manager dashboard on Windows server 2019, click on Manage and on Add roles and Features</P>
<p align="center"><img src="https://i.imgur.com/BFw3fUz.png" height="50%" width="50%"/>

<p>2. On the before you begin screen, just click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/gaMZiFi.png" height="50%" width="50%"/>

<p>3. On the Installation Type screen, make sure the Role-based or feature-based installation is selected, then click <b>NEXT.</b>.</p>
<p align="center"><img src="https://i.imgur.com/ZvY3YRt.png" height="50%" width="50%"/>

<p>4. On the Select destination server screen, make sure the select a server from the server pool is selected and select the server you are trying to install Adds from the server pool, then click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/Xq8ggfi.png" height="50%" width="50%"/>

<p>5. On the Add Roles and Features Wizard > Select server role > role page,select Remote Access click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/U9DIq1A.png" height="50%" width="50%"/>

<p>6. On the Add Roles and Features Wizard > Select role services page > role services, select Remote Access and VPN(RAS) and click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/pso68N7.png" height="50%" width="50%"/>

<p>7. On the Add Roles and Features Wizard > Add features that are required for DirectAccess and VPN(RAS) page, click ADD FEATURES.</p>
<p align="center"><img src="https://i.imgur.com/zpz6YVw.png" height="50%" width="50%"/>

<p>8. On the Add Roles and Features Wizard > Confirm Installation Selections page, click INSTALL.</p>
<p align="center"><img src="https://i.imgur.com/doATHOv.png" height="50%" width="50%"/>

<p>9. On the Add Roles and Features Wizard > Installation Progress page, click CLOSE when installation is complete.</p>
<p align="center"><img src="https://i.imgur.com/cYaLX4w.png" height="50%" width="50%"/>

<p>10. Back to the server manager, click on the notifications and click on “Open the getting started wizard” OR go to Tools and scroll down to Routing and Remote Access and double click.</p>
<p align="center"><img src="https://i.imgur.com/cBwX85l.png" height="50%" width="50%"/>
<p align="center"><img src="https://i.imgur.com/ukeAAE4.png" height="50%" width="50%"/>
  
<p>11. After the Routing and Remote Access is opened, right-click on ADDS-SERVER(local) and click on “Configure and Enable Routing and Remote Access”</p>
<p align="center"><img src="https://i.imgur.com/kO6Hx39.png" height="50%" width="50%"/>

<p>12. On the Routing and Remote Access Server Setup Wizard first page, click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/UeKRLSk.png" height="50%" width="50%"/>

<p>13. On the Routing and Remote Access Server Setup Wizard > Configuration page, select Custom configuration click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/9TVhRtG.png" height="50%" width="50%"/>

<p>14. On the Routing and Remote Access Server Setup Wizard > Custom Configuration page, select VPN Access and click NEXT.</p>
<p align="center"><img src="https://i.imgur.com/eRvwsHc.png" height="50%" width="50%"/>

<p>15. On the Routing and Remote Access Server Setup Wizard > Completing the Routing and Remote Access Server Setup Wizard page, click FINISH.</p>
<p align="center"><img src="https://i.imgur.com/8YjAHDl.png" height="50%" width="50%"/>

<p>16. On the Routing and Remote Access  > Start the service page,  click on Start Service.</p>
<p align="center"><img src="https://i.imgur.com/i86dJ1Q.png" height="50%" width="50%"/>


<p>17. After clicking on Start Service, you will right-click on ADDS-SERVER(local) and go to properties. From properties, go to IPv4 and you can choose between Dynamic Host Configuration Protocol(DHCP) and Static Address Pool. But for this project, we are going to leave it at DHCP.</p>
<p align="center"><img src="https://i.imgur.com/uXndOoj.png" height="50%" width="50%"/>
<p align="center"><img src="https://i.imgur.com/ViMUYCt.png" height="50%" width="50%"/>

<br>
<br>

<h3>*Add VPN User in Active Directory</h3>
<p>1. I will be using one of the users in Active Directory. Right-click on the user and go to properties </p>
<p align="center"><img src="https://i.imgur.com/2wYtCRl.png" height="50%" width="50%"/>

<p>2. On the properties’ page, go to on Dial-In and click on Allow Access on the Network Access Permission tab</p>
<p align="center"><img src="https://i.imgur.com/uSLkiXP.png" height="50%" width="50%"/>

<br>
<br>

<h3>*Configure Windows Server Firewall</h3>
<p>1. From your computer, go to control panel and click on System and Security.</p>
<p align="center"><img src="https://i.imgur.com/1oNQ2tU.png" height="50%" width="50%"/>

<p>2. On the Control Panel > System and Security page, go to Windows Defender Firewall tab and click on Allow an app through Windows Firewall.</p>
<p align="center"><img src="https://i.imgur.com/KtaDxS9.png" height="50%" width="50%"/>

<p>3. On Windows Defender Firewall >Allowed Apps page, scroll down to Remote Access and make sure the Public and Private check boxes are selected.</p>
<p align="center"><img src="https://i.imgur.com/jKjHUjU.png" height="50%" width="50%"/>

<br>
<br>

<h3>*Configure Home Router</h3>
<p>1. To do this, login to your ISP  homepage and click on Network.</p>
<p align="center"><img src="https://i.imgur.com/BQCsaPb.png" height="50%" width="50%"/>

<p>2. From the Network’s page, go to Firewall and under Firewall, click on Port Forwarding.</p>
<p align="center"><img src="https://i.imgur.com/2oxHwxr.png" height="50%" width="50%"/>

<p>3. On the Network >Firewall > Port Forwarding page, click on Add to add to the Port Forwarding rule</p>
<p align="center"><img src="https://i.imgur.com/2XMPMFy.png" height="50%" width="50%"/>

<p>4. On the “Add” page, input your Host Id to complete the Private IP, leave the Protocol as TCP, Input PPTP port(1723) in the spaces provided for Private and Public Ports.</p>
<p align="center"><img src="https://i.imgur.com/WsSefj7.png" height="50%" width="50%"/>

<p>5. Back on the Network >Firewall > Port Forwarding page, you will see the rule you just added.</p>

<br>
<br>

<h3>*Find Public IP Address</h3>
<p>To find your public IP Address, you can search on google or just go to whatismyip.com and copy your public IPv4</p>
<p align="center"><img src="https://i.imgur.com/PzSVxMC.png" height="50%" width="50%"/>

<br>
<br>

<h3>*Verify by connecting to the Remote Network</h3>
<p>1. From your computer’s Typesearch, type VPN and click open</p>
<p align="center"><img src="https://i.imgur.com/LGnSqoR.png" height="50%" width="50%"/>

<p>2. On the VPN’s page, click on Add a VPN </p>
<p align="center"><img src="https://i.imgur.com/rR4uB4i.png" height="50%" width="50%"/>

<p>3. 3.On the Add a VPN connection’s page, select the Windows(built-in) option for VPN provider, Name the connection name as you desired, Input the public IP address of the server you are trying to connect to. For the VPN Type, select PPTP, Select User name and Password for the Type of sign in info. Then input the Username and the Password and click Save.</p>
<p align="center"><img src="https://i.imgur.com/swRQTOE.png" height="50%" width="50%"/>
<p align="center"><img src="https://i.imgur.com/6Xevspw.png" height="50%" width="50%"/>

<p><b>P.S - I couldn’t connect the VPN user because the PPTP port(1723) used was closed. I tried calling my ISP for a solution(maybe to open it for a few days for the purpose of this project) but couldn’t find any resolution. 

<p>Since, I couldn’t find a solution, I moved on to use Softether VPN to complete this project.</b></p>
<p align="center"><img src="https://i.imgur.com/IAYgdo3.png" height="50%" width="50%"/>

<p><I><b>NOTE: To check whether a port is closed or opened, use this link - https://www.yougetsignal.com/tools/open-ports/</b></I></p>
<p align="center"><img src="https://i.imgur.com/mlmR5Lj.png" height="50%" width="50%"/>
