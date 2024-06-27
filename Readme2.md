# Microsoft Azure Sentinel Lab

## Objective
[Brief Objective - Remove this afterwards]

The Purpose of the ***"Microsoft Sentinel Training Lab Solution"*** and by extension this particular lab is to provide hands-on practical experience with the Sentinel product features, capabilities, and scenarios. Sentinel is both a SIEM (Security Information Event Management) and SOAR (Security Orchestration, Automation, and Response) all in one solution, providing both a way to aggregate and ingest logs from various sources and a way to automate and respond to threats and incidents. 

From the [overview page](https://portal.azure.com/#create/azuresentinel.azure-sentinel-solution-azuretraininglab) it says "This solution ingests pre-recorded data into your Microsoft Sentinel workspace and enables several artifacts to simulate scenarios that showcase various Microsoft Sentinel features." In short, this lab will provide cybersecurity analysts with hands on experience with both Sentinel and certian aspects of Azure.

The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned

- Advanced understanding of SIEM and SOAR concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Microsoft Azure for the purpose of deploying a virual environment for our lab.
- Utilizing Microsoft Sentinel SIEM and SOAR for log ingestion, analysis, and hands-on experience with automation and response.
- Employing network analysis tools, such as Wireshark, for capturing and examining network traffic.

## Steps

### 1. Create Free Trial Account With MS Azure

https://azure.microsoft.com/en-us/free/

### 2. Create Microsoft Sentinel

On your home page, under ***Azure services"*** you should see an icon called ***"Microsoft Sentinel."*** Click on that icon and then click on ***"Create."***

![Step 1-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/d601c291-1ea9-4c3d-ae9a-dd1577dc8698)

If you do not see that icon, you can simply search for ***"Sentinel"*** from the search bar, select ***"Microsoft Sentinel"*** and then click ***"+ Create"*** at the top right hand corner of the screen.

![Step 1b-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/670068a6-4469-436d-897b-b34da31bc47c)

From here, we will create a new ***"workspace."*** by clicking on ***"+ Create a new workspace"*** at the top right hand corner of the screen.

![Step 2-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/64f7e190-b584-4d32-af19-960f288f23dc)

You will then be brought to this screen here:

![Step 12-Create Log Analytics Workspace](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/c089971e-59cf-4468-82d6-4827ff819f5a)

* ***Subscription:*** if you are using a trial account, you will likely see "Azure subscription 1," otherwise, choose the subscription that you want to use.
* ***Resource Group:*** You can either select a resource group that you have created in the past or create a new resource group. In this tutorial, we will be creating a new resource group. Click ***"Create new"*** and name this workspace whatever you like. I will be naming mine "Sentinel-RG." Click the ***"OK"*** button.
* ***Name:*** Now we want to name our workspace. For this tutorial, I will be naming mine ***"Sentinel-Workspace."***
* ***Region:*** Select a region close to you.

When you are done, it should look something like this:

![Step 3-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/cac485d2-2170-4757-9243-9dfafe21e551)

* Click the ***"Review + Create"*** button.
* After the validation is complete and you get the "Validation passed" message at the top of the screen, click the ***"Create"*** button at the bottom of the screen.
* Once complete, you should get a notification that says your deployment succeeded and your screen should not look like this.

![Step 4-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/a90bcae8-8397-4f4f-b933-a9b3fae6ec31)

Note: If you do not see your new workspace listed below, refresh your page and it should pop up.

Select your new worspace and click on the ***"Add"*** button at the bottom of the page. This will take a few minutes.

Once the workspace has been added to Microsoft Sentinel, if you have not already activated your free trial of Microsoft Sentinel, you will see a message stating that your free trial has been activated like this:

![Step 5-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/d293c911-79ba-404d-9620-682e1d463987)

We can also see "selected workspace: 'sentinel-workspace' (or whatever you named your workspace) at the top under the ***"Microsoft Sentinel"*** header.

### 3. Deploy Microsoft Sentinel Training Lab Solution

We are now going to deply the Microsoft Sentinel Training Lab Solution in our new workspace. This will ingest pre-recorded data and create several other artifacts that we will be using in this lab. 

* From the search bar, search for "Sentinal training lab" and select ***"Microsoft Sentinel Training Lab Solution"*** under ***Marketplace.***
* You should now land on a page that looks like this:

![Step 6-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/6b6df232-fda6-4f10-9735-6da631a3ef66)

Click and the ***"Create"*** button and you should land on this page:

![Step 7-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/43074006-0096-444e-9081-38c6365c3f30)

- Make sure you select the ***"Resource grou"*** and ***"Workspace"*** that you just created for this lab, then click on the ***"Review + create"*** button at the bottom of the page.
- Once the validation process has completed, click the ***"Create"*** button at the bottom of the page. The Deployment process may take up to 15-20 minutes.

![Step 8-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/66c3611e-ffb7-4bf3-9586-b4c93aa72f35)

Once complete, go back to your Microsoft Sentinel. You can do this by typing ***"Sentinel"*** into the search bar and then selecting ***"Microsoft Sentinel"*** from the drop down.

You should have landed here.

![Step 10-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/907987f1-a854-4c69-9f6c-64be5e2ae9b4)

Select and click on your workspace and you will land on a page that looks like this:

![Step 11-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/ca7b80d9-57d7-4e79-8259-b90fa9c383a7)

You should now see a few incidents. If you do not, allow 10 more minutes for the incidents to populate.

### 4. Configure Microsoft Sentinel Playbook

Now we want to configure a Playbook that we will use later in the lab. 

- Click on the menu button at the top left hand corner of the screen.
- Click on ***"Resource groups"***
- Select the resource group we created for this lab

You should now be on this page:

![Step 13-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/341525b5-072c-4f81-87ae-c91ad8f315c5)

- At the bottom of the page, you will see a list. You should see an API Connection resource called azuresentinel-Get-GeoFromIpAndTagIncident, click on it.
- From this page, you want to select ***"Edit API connection"*** under ***General.***

![Step 14-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/7d2e930f-5a7e-4cac-ac64-a58f34c1a54a)

You should now be on this page:

![Step 15-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/9e05ca35-59d2-40c7-8f99-cdee20b8fc7e)

- Click on Authorized, and then when prompted, select your account.
- Click ***"Save"***



* From here, you can read up on the Microsoft Sentinel Training Lab Solution.
* Once you have done that, click the ***"Create"*** button.



