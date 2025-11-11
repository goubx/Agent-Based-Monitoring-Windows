# Agent-Based-Monitoring-Windows

I will demonstrate how to set up and run local agent-based scans on tenable.io for Windows devices.

<h2>Requirements for this Lab</h2>

- <b> Azure </b>
- <b> Windows 11 Pro VM </b> 
- <b> Tenable.IO </b>

<h2>Walkthrough</h2>

<p align="center">
After creating a Windows virtual machine, I will now show you how I go about creating a local-based agent to scan my Windows device. The purpose of this is that in work settings, employees are given devices to work on outside of the office. Instead of having to manually check all of those devices, I can set up an agent that does a self-assessment on the device it's installed on.
  
<h2> 1. While my VM is booting up, I will log onto Tenable. </h2>

<h3> Now I'm gonna create the Agent Group </h3>

Agent groups are used to organize and manage the agents linked to your account. Each agent can be added to any number of groups, and scans can be configured to use these groups as targets.

Go to:

* Settings
* Sensors
* Nessus Agents
* Agent Groups

<img src="https://i.imgur.com/N4agAsd.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

> As you can see, the Agent Group above was created.

<h2> 2. Next, I'm going to create a Basic Agent Scan </h2>

* Go to Scanner
* Press New Scan
* Press the Nessus agent
* Then select the Basic Agent Scan Template

This is what it looks like when you get there.

<img src="https://i.imgur.com/AIqjFK6.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

Configurations:
* I selected the group I created in the "agent groups" section.
* Then I named the scan
* For the Scan Type, I am selecting a triggered scan so that when a filename I select ends up in the directory, & the agent discovers it, it will know it's time to scan.

For this scenario, the key file will be 
> goub.txt

<h3> I will now leave the scan as is and save it. </h3>

<h3> Copy and paste this command from settings in Tenable. This is the code that will be used to install the agent on the VM. </h3>

* Settings
* Sensors
* Nessus Agents
* Linked Agents
* Look at the instructions for installing the Agent on Windows Platforms

It should look like this, on the right side of the screen.

<img src="https://i.imgur.com/HttwobU.png" height="80%" width="80%" alt="Agent Group created"/>
<br />

Copy and paste the command as seen:

> Invoke-WebRequest -Uri "https://sensor.cloud.tenable.com/install/agent/installer/ms-install-script.ps1" -OutFile "./ms-install-script.ps1"; & "./ms-install-script.ps1" -key "58aab372289ac80911e4c5ad40a07b23b5524319f9ff5c010aa50ec625ccf389" -type "agent" -name "<agent name>" -groups '<list of groups>'; Remove-Item -Path "./ms-install-script.ps1"

Keep this because you will have to edit in the file name & group names. 
<br />

<h2> 3. Now I will install the agent on the virtual machine. </h2>

<h3> I will have to run commands on PowerShell to install the agent. </h3>

First, I will open up the Notepad in my VM and paste that command. 

<img src="https://i.imgur.com/rQG5qjY.jpeg" height="80%" width="80%" alt="Agent Group created"/>
<br />

It is basically downloading the PowerShell Script from the Tenable Server. It will provide things like the key, agent, agent groups, etc.

I am going to put my agent group's name, which is "Agent-Goub-Win11," that I created, in the spot where it says list of groups.

And now it should say instead:

Invoke-WebRequest -Uri "https://sensor.cloud.tenable.com/install/agent/installer/ms-install-script.ps1" -OutFile "./ms-install-script.ps1"; & "./ms-install-script.ps1" -key "58aab372289ac80911e4c5ad40a07b23b5524319f9ff5c010aa50ec625ccf389" -type "agent"  -groups 'Agent-Goub-Win11'; Remove-Item -Path "./ms-install-script.ps1"

> I removed the agent from the code since it is not necessarily needed in this scenario.

<h3> Now I will launch the command into PowerShell. </h3>

<img src="https://i.imgur.com/vq1Eq03.png" height="80%" width="80%" alt="Launch Command"/>
<br />

> Here is the installation occurring. It will take a few minutes for the installation to finish.

<h3> Now my agent is installed on the VM. Below is a screenshot to confirm it is running in the background. </h3>

<img src="https://i.imgur.com/q3Q1tJX.png" height="80%" width="80%" alt="Launch Command"/>
<br />

<h3> Now in PowerShell, I am going to add the file I created as the trigger into the trigger directory </h3>

<img src="https://i.imgur.com/Wgh3Q1t.jpeg" height="80%" width="80%" alt="Launch Command"/>
<br />

The commands to create this in PowerShell are 

> cd \

> cd programdata

> cd Tenable

> cd '.\Nessus Agent\'

> cd nessus

> cd triggers

> New-Item -Name goub.xt <---- Trigger File Name

<img src="https://i.imgur.com/Wgh3Q1t.jpeg" height="80%" width="80%" alt="Trigger creation"/>
<br />

This is what it looks like before I add the file

<h3> After adding the file, the scan should start. </h3>

<img src="https://i.imgur.com/3apcJ6b.jpeg" height="80%" width="80%" alt="Trigger creation"/>
<br />

The scan has started since it was deleted from the trigger folder.

As you can see below, the scan has been activated since it's running in the task manager. This is also confirmed because in my Tenable dashboard, the files are showing as "triggered".

<img src="https://i.imgur.com/pNm67DZ.png" height="80%" width="80%" alt="Trigger creation"/>
<br />

<img src="https://i.imgur.com/pekkXyl.png" height="80%" width="80%" alt="Trigger creation"/>
<br />

Now I just have to wait for the results once the scan is complete.

<img src="https://i.imgur.com/bpBgL8m.png" height="80%" width="80%" alt="Trigger creation"/>
<br />

<h2> It should look like this while the scan is running in Tenable. </h2>
<br />

<img src="https://i.imgur.com/bpBgL8m.png" height="80%" width="80%" alt="Trigger creation"/>
<br />

<h2> With the scan now complete, I will upload the completed scan file. </h2>
