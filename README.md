# Creation-and-configuration-of-a-Wazuh-SIEM
<h2>Description</h2>
The first step is creating a Linode account, which you can do at https://cloud.linode.com/ once I created a linode account, I launched Wazuh as a virtual machine in Linode. I did this by clicking “create>Linode>marketplace” and searched and clicked on Wazuh in the search bar. Then from there, I entered my email, new credentials, name for the VM, etc. Next I selected the image, which I chose Ubuntu 22.04, and I chose the Linode 4GB plan because any plan cheaper than that would not work.
<img src="https://imgur.com/dcwghSV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br/>Because by default SSH is open to everybody, I created a firewall in Linode and added firewall rules to allow all TCP and UDP traffic from my IP address so that my virtual machine is only accessible to me
<img src="https://imgur.com/IVWZJH5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Launched Wazuh as a Virtual machine using Linode.
<img src="https://imgur.com/hu1IAZW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 


Then proceeded to install Wazuh by copying and pasting the SSH access code into the windows command prompt. I watched Wazuh install by entering “htop” as a command. I then put “ls -al” to list all files and then entered “cat .deployment-secrets.txt” to find my admin username and password to connect to Wazuh. 
<img src="https://imgur.com/mpWKc83.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Afterward, I found the reverse DNS address under Network>IP addresses section in Linode. Copied and pasted the reverse DNS address into my browser and connected using the admin username and password.
<img src="https://imgur.com/ViazzEj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 

Now I’m loaded into the SIEM, I have my Windows machine already connected as an agent, but below are the steps I took to connect my Kali Linux VM.

<img src="https://imgur.com/ARivdoW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
First I clicked “add agent” and then “deploy a new agent” then for the address that the agent uses to communicate with the Wazuh server, I copied and pasted the FQDN as shown below
<img src="https://imgur.com/c1U15l7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/HRccQrh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Then I entered the following command into my Kali Linux terminal and it installed Wazuh
<img src="https://imgur.com/J0CGCEt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Now my 2 agents are successfully connected to the SIEM, I can now run Security Configuration Assessments to get a benchmark for how secure my agents are, scan for vulnerabilities, monitor security events and setup real-time alerts when security events are detected, and so much more.

Vulnerability scanning in Wazuh is disabled by default, so I enabled the vulnerability detector by going into the configuration settings of Wazuh and scrolling down until I see “Vulnerability detector” and changing the configuration from “no” to yes”
<img src="https://imgur.com/uM7VJqc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


I also enabled real-time alerts for file integrity monitoring by editing the ossec configuration for my agent. C:\Program Files (x86)\ossec-agent. I did this in the ossec.conf file by 1. Specifying a directory 2. Listing realtime=”yes” 3. Listing report_changes=”yes” 4.check_all=”yes” and then added the directory that I am going to monitor with all of these options, which is C:\Users\Username\Desktop</directories> Now anytime a file is created or changed, the alert will show up under “integrity monitoring” in the SIEM
<img src="https://imgur.com/AwVRZTb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Now I am going to configure my SIEM  to send me alerts in Slack. The first step to take so is to go to my Slack api and click “create a new app”> ”from scratch” then click on “incoming webhooks and activate it. Then click “Add New Webhook to Workspace” and then choose whichever channel I want notifications to be sent too.
<img src="https://imgur.com/CGe0RTb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Next I am going to copy the Webhook URL and then go to Wazuh>management>configuration>edit configuration. Then underneath “global” on the 21st line, I input this text:
<integration>
<name>slack</name>
<hook_url>https://hooks.slack.com/services/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</hook_url>
 <alert_format>json</alert_format>
 </integration>
Then I replaced the hook url with the webhook url from my slack api


<img src="https://imgur.com/BREFpi9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/lETJz0M.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Now I am getting notifications from Wazuh sent to my Slack channel.
<img src="https://imgur.com/ZAHekDT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>






