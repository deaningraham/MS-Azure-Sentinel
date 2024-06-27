# Microsoft Azure Sentinel Lab

## Objective
[Brief Objective - Remove this afterwards]

The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### 1. Create Free Trial Account With MS Azure

https://azure.microsoft.com/en-us/free/


### 3. Create Log Analytics Workspace

Next we want to create a "log analytics" to store our logs from the Honepot-VM for our SIEM to aggregate and use. To do this, type into the search bar, "log analytics workspace" and select the option ***"log analytics workspaces"*** and you should be brought to a screen the looks like this

![Step 11-Create Log Analytics Workspace](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/2178df17-5eeb-4350-8285-ea3e95a5eca9)

Click on the ***"create log analytics workspace"*** button in the middle of the page.

You will then be brough to a page that looks like this:

![Step 12-Create Log Analytics Workspace](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/c089971e-59cf-4468-82d6-4827ff819f5a)

* ***Resource Group:*** go ahead and select the resource group that you created. Mine is called "Azure-Sentinel-Honeypot-Lab."
* ***Name:*** Name this work space whatever you like. I will be naming mine "Honeypot-logs"
* Double check that the "Subscription" and "Region" are the same as the ***"Basics"*** settings for the "Honeypot-VM."
* Click the ***"Review + Create"*** button.
* After the validation is complete (passes), click the ***"Create"*** button at the bottom of the screen.
