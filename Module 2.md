In this module we want to install and enable Data Connectors in Microsoft Sentinel to bring in alerts and/or telemetry from different sources. However, before we can do that, we need to make sure that we have the apropriate roles and permissions set up on our workspace.

Here is a brief description of each set of Roles and Permissions for MS Sentinel taken from this Microsoft [article](https://learn.microsoft.com/en-us/azure/sentinel/roles#microsoft-sentinel-specific-roles)

## Microsoft Sentinel-specific roles
All Microsoft Sentinel built-in roles grant read access to the data in your Microsoft Sentinel workspace.

- ***Microsoft Sentinel Reader*** can view data, incidents, workbooks, and other Microsoft Sentinel resources.
- ***Microsoft Sentinel Responder*** can, in addition to the permissions for Microsoft Sentinel Reader, manage incidents like assign, dismiss, and change incidents.
- ***Microsoft Sentinel Contributor*** can, in addition to the permissions for Microsoft Sentinel Responder, install and update solutions from content hub, and create and edit Microsoft Sentinel resources like workbooks, analytics rules, and more.
- ***Microsoft Sentinel Playbook Operator*** can list, view, and manually run playbooks.
- ***Microsoft Sentinel Automation Contributor*** allows Microsoft Sentinel to add playbooks to automation rules. It isn't meant for user accounts.

***[Note]*** For best results, assign these roles to the resource group that contains the Microsoft Sentinel workspace. This way, the roles apply to all the resources that support Microsoft Sentinel, as those resources should also be placed in the same resource group.

As another option, assign the roles directly to the Microsoft Sentinel workspace itself. If you do that, you must assign the same roles to the SecurityInsights solution resource in that workspace. You might also need to assign them to other resources, and continually manage role assignments to the resources.

If you continue reading the article, Microsoft also gives us information on other roles and permissions for special use cases.

## Setting up the Roles and Permissoins for Sentinel

- From the menu at the top left corner of the screen, we want to again select ***"Resource groups"***
- Select the resource group you created for this lab. In my case, it is ***"Sentinel-RG"***
- On the left hand side, you will select ***"Access control (IAM)"***

And now you should be on this page:

![Step 17-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/bce3370a-87cd-434d-9e0d-2dcb7715d3ee)

Click on ***"Role assignments"*** and if all went well, you should see your name listed below ***"Owner"*** with inherited ***"Owner"*** permissions, which essentially gives you full control. 

The page should look something like this, though you may only see your name listed once.

![Step 18-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/3651c56c-013e-47f8-b2a7-acdab331bbf9)

- If would like to add additional users and roles, you can do this by clicking on the ***+ Add*** button near the top of the screen.
- Select ***"Add roles assignment"*** and you should see a large list of roles to choose from.

![Step 19-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/061bb65b-6d1e-47d1-914a-fa66035fab50)

- We want to filter out the roles by typing in ***"Sentinel"*** into the search box above the list.
- From here, you can assign the appropriate role for any additional users you may want to add.

![Step 20-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/b180ae68-bc89-4722-a93e-840f87f0042b)

- Click to highlight the role you want to assign. I am going to select "Microsoft Sentinel Contributor" role for demonstration purposes.
- Click on the ***"Members"*** tab near the top of the screen.
- Click on ***+ Select member*** and you should see the following pop up on the right side of the screen.

![Step 21-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/6c2bebc0-1f28-45e0-a4a9-fd57459b78b1)

Select your user and click on the ***"Select"*** button at the bottom and now you should see the user and role added to ***"Access control (IAM) page"*** under the ***"Role assignments"*** tab.

## Enable the Azure Activity Connector

Now we will be enabling the Azure Activity connector for Sentinel. This connector imports logs from Azure management plane activities, which lets you track Azure administrative activity within the subscription.

- From the home screen, click on ***"Microsoft Sentinel"*** under Azure service or search for using the search bar.
- Select the workspace you created for this lab.
- Then select ***"Content hub"*** under ***"Content management"*** from the money on the left side of the screen.

![Step 22-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/e2be0ef6-0d47-4cd4-83b5-3a0bbb766ced)

- Select ***"Azure Activity"*** from the list.
- Click on the ***"Install/Update"*** button near the top of the screen.
- Once it has been installed, you will see a green check mark next to ***"Azure Activity"*** like this
  
![Step 25-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/a4dc32f3-e71a-4ade-9d4b-90a39e1c3353)

- Once the deployment completes, click on ***"Data connectors"*** under ***"Configuration."*** You may have to scroll down the menu a bit, but it will be right below the ***"Content management"*** section.
- In the data connectors screen, click the Azure Activity connector.
- Once you have ***"Azure Activity"*** selected, you can click on the ***"Open connector page"*** button on the right hand side of the page.

You should now be at this page:

![Step 26-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/828dda04-d0bd-4fa0-8de5-9025f660df83)

- On the Azure Activity connector page, scroll down through the Configuration instructions until you get to number 2, Connect your subscriptions through diagnostic settings new pipeline.

![Step 27-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/cff37d78-83a5-4213-9cf2-254054c976f2)

- Click on Launch Azure Policy Assignment wizard, which will redirect you to the policy creation page.

![Step 28-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/92e71088-e303-4139-a216-325d4856efad)

- Next to the ***"Scope"*** field, click on the blue button with the three dots "..."
- Select your subscription and then click on the ***"Select"*** button at the bottom of the screen.
- Next go to the Parameters tab. On the ***"Primary Log Analytics workspace"*** select the workspace you created for this lab.

![Step 29-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/05e0d033-d795-4e36-b151-b1e61258c548)

We'll use the deployIfNotExists (DINE) feature of Azure Policy to deploy the setting directly to any Activity logs in scope. To do this, tick Create a remediation task. Leave the Managed Identity set to a System Managed Identity, and pick a region proximate to your subscription.

![Step 30-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/1ab865e9-329f-43f5-a827-2a40d8ebc4da)

Press Review and Create, and then Create to save the policy.

- Now we want to go back to the ***"Content"*** Hub and click on ***"Azure Activity"*** 
- Now click the ***"Manage"*** button at the bottom of the panel on the right.

![Step 31-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/8ea0ba96-ff2a-425b-b983-d6e1c16d7763)

In the Manage view, you can see the contents of the solution, including connectors, analytics rules, workbooks, hunting queries, and other included content. Once installed, these components will be visible in their respective sections within the Sentinel interface, such as Analytics rule templates and Workbook templates.

![Step 32-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/bbdc50f6-598e-42c6-98ce-11062d0679eb)

## Enable the Microsoft Defender for Cloud Data Connector

In this section, we will enable the Microsoft Defender for Cloud data connector. This will allow you to stream security alerts from Microsoft Defender for Cloud into Microsoft Sentinel. This integration helps you incorporate Defender alerts, view Defender data in workbooks, and more effectively investigate and respond to incidents.

- Like before, we want to open up ***Microsoft Sentinel.***
- From there, click on the ***Content hub*** and then in the search bar, type "defender" and then select ***Microsoft Defender for Cloud***
- Click the ***"Install/Update"*** button near the top or the ***"install"*** button at the bottom right of the screen.

![Step 33-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/4991026d-b1a0-4bd5-9ecb-7ec745ee0a49)

#MAY HAVE TO ADDRESS ISS WITH MISSING GALLARY OPTIONS AND INSTALL OOBE. IMAGES SAVED IN FOLDER.

- Now navigate back to the ***"Data connectors"*** section under the ***"Microsoft Sentinel"*** page.
- Select ***"Tenant-based Microsoft Defender for Cloud (Preview).
- Click on the ***"Open connector page"*** button at the bottom right corner.

![Step 38-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/b6f4e687-1924-443e-8fad-2681be745faa)

You should now be on this page:

![Step 39-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/6cebf697-2c43-41a5-942a-9f00a75245a9)

- Scroll down the section of the page under ***"Instructions"*** until you see the ***"Connect"*** button, then click on it.

You've now connected alerts from Microsoft Defender for Cloud. This connector populates the 'SecurityAlerts' table when a Defender for Cloud alert is raised, but at this stage, that alert won't be promoted into an Incident. We'll cover a rule to promote the alerts in the next module.

## Enable Microsoft Defender Threat Intelligence connector

Now we will add the ***"Microsoft Defender Threat Intelligence"*** (MDTI) connector to your Sentinel workspace, which ingests Microsoft Threat Intelligence indicators automatically into the `ThreatIntelligenceIndicator` table.

The *Threat Intelligence* content solution includes the data connectors for all supported forms of Threat Intelligence.

**NOTE:** Sentinel also supports importing Threat Intelligence indicators via the TAXII protocol using the **Threat Intelligence - TAXII** data connector, so if you have your own preferred TI source, you can add that to your workspace instead.

- To install the MDTI search for ***"Threat Intellegence"*** and follow the same steps you peformed for the Microsoft Defender for Cloud.
- Once you have ***"Threat Defender"*** installed, click on the ***"Manage"*** button.
- Select ***"Microsoft Defender Threat Intelligence (Preview)"*** and then click on the ***"Open connector page"*** button.

You may get the following error at the top right hand corner of your screen

![Step 40-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/2d8bb196-ce4f-4743-9a38-3a9dee93628f)

- If you get this error, simply click on the ***"Microsoft Defender Threat Intelligence"*** title to take you to the ***"Threat Intelligence"*** page.
- Select the ***"Microsoft Defender Threat Intelligence (Preview)"*** solution, then click the ***"Open connector page"***button.

![Step 41-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/8a2aedf6-f22b-4be7-a41e-8851088d79d7)

You should now be on this page:

![Step 42-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/fc396415-54f8-45de-a828-aab87b920d0b)

- Leave the ***"Import indicators"*** at ***"All available"*** and then click on the ***"Connect"*** button.
- Threat Intelligence indicators will start being ingested into your ThreatIntelligenceIndicator table.

## Enable Threat Intelligence TAXII data connector

In this section, we will enable the Threat Intelligence - TAXII data connector. This connector allows you to import threat indicators from TAXII servers into Microsoft Sentinel. These indicators can include IP addresses, domains, URLs, and file hashes.

Before we go any further, we are going to set up a free account at plusedive.com

![Step 44-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/6ccaa44f-9977-4409-a7b7-8219cd831d22)

-  Head back to the ***"Microsoft Sentinel"*** page and select ***"Threat Intelligence"*** from the ***"Content hub."***
-  Click on the ***"Manage"*** button.
-  Select ***"Threat Intelligence - TAXII"*** and click on the ***"Open connector page"***
  - Note: If you get another error, simply click on the title as you did before, select the ***"Threat Intelligence - TAXII"*** solution and then click on the ***"Open connector page"*** button.

You should now be on this page:

![Step 43-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/da2feeab-c05e-4ed3-a2b4-cc255f848f92)

- ***Friendly name (for server):*** Put whatever you like, I will simply name mine "pulsefeed."
- ***API root URL:*** https://pulsedive.com/taxii2/api/
  - ***NOTE:*** This information can be found at this link https://pulsedive.com/api/taxii
- ***Collection ID:*** 981c4916-ebb2-4567-aece-54ae970c4230 (Test collection)
  - ***NOTE:*** This information can be found at this link https://pulsedive.com/api/taxii
- ***Username:*** taxii2
  - ***NOTE:*** This will NOT be the username that you created when you registered with the site.
- ***Password:*** This will be YOUR API Key, which can be found here https://pulsedive.com/api/taxii at the top of the page.
- ***Import indicators:*** We can leave this as default; "All available."
- ***Polling frequency:*** We can leave this as default; "Once an hour."

Once you have this all filled out, your configuration should look something like this:

![Step 45-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/96149ec5-f877-4d43-89f2-a4f072ae930a)

***REMEMBER:*** Your password (your API key) will be different. I have deleted half of my key, so yours will be about twice as long.

- Click the ***"Add"*** button

You should get a notification stating that your TAXII connector has been added successfully at the top right hand corner of the screen and you will see it listed below the form.

![Step 46-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/ff497730-b66f-4261-b30f-f7d7d0f3af43)







