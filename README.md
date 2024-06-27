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

### 2. Create Honeypot VM 

Once you have created your free Azure account, you should be brought to a screen like this. Search for virtual machine and click on virtual machine on the drop down menu.

![Step 1-Create Virtual Machines](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/746cf46e-ca76-4d31-b588-53793b6fb504)

This virtual machine will be our exposed host machine (honey pot) that will be attacked.

Once you click on “Virtual Machines” you will come to this screen. Click on create, then select the top option “Azure virtual machine.”

![Step 2-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/e8531d9a-c84d-491e-87ea-dbeb3c206480)

From here, you will come to this page where we will actually create our honey pot.

![Step 3-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/f40d804d-89ea-42db-8ed5-421905288f1b)

* ***Create a new Resource Group***
    * This will create a container that will hold all of our lab resources.
    * I will naming mine Azure-Sentinel-Honeypot-Lab
* ***Virtual Machine name:***
    * This will be the name of our honeypot
    * I will be naming mine Honeypot-VM
* ***Region:***
    * Choose whatever region is closest to you. For me, I will be choosing “(US) West US 2” since that is the only zone near me that is available with this subscription type.
* ***Zone options:***
    * Assuming that the region you chose supports availability zones, you may have the following options
        * <ins>**No infrastructure redundancy required**</ins>
            * For this lab, we really do not need any redundancy, so this is what we will choose.
        * <ins>**Availability zone**</ins>
            * This option would separate our VMs between three different data centers within the same region, providing redundancy and some resilience in the event of a natural disaster, electrical failure, network failure, etc. at one of the data centers.
            * Each of these data centers are separated by about 300 miles, with no greater than a 2 millisecond round trip latency between zones.
        * <ins>**Virtual machine scale set**</ins>
            * This particular option provides elasticity allowing the number of VM instances to scale up or down based on resource need and usage. 
        * <ins>**Availability set**</ins>
            * Finally, this will distribute our VMs across different fault domains, but within the same data center. This will provide redundancy, but no resilience in the case of a natural disaster.
    * As you can see, since this is just a honey pot, none of these options really pertain to this lab, but would be useful in enterprise environments where availability is crucial.
* ***Availability zone:***
    * You will need to choose between zone 1, zone 2, and zone 3 depending on subscription availability. See Size option below. Notes below the available options will tell you what zone is available for your subscription.
* ***Security Type:***
    * Since this is our honeypot, we do not want to beef up our security, but rather we want to keep our security at a minimum, so we will select “standard.”
* ***Image:***
    * For this honeypot, we will select Windows 10 Pro.
* ***Size:***
    * Default selection should be fine, unless size is unavailable in your zone. In which case, you may have to choose between zone 1, zone 2, or zone 3.
* ***Username and Password:***
    * Nothing special here, just make sure you make a username and password that you will remember.
* ***Ports:***
    * We will leave the default settings.
* ***Licencing:***
    * Check the box
* Click on the ***Next: Disks >*** button.


**This now brings us to our next page:**

![Step 4-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/1db5745e-1e72-4129-a679-e7c2053bf108)

 * Here we will leave the defualt settings for disk size and type.
 * There will be no encryption for our subscription type and we will be keeping the default option for key management.
 * We will be deleting the disk when we delete the VM.

Click on the ***Next: Networking >*** button and you will come to this screen:

![Step 5-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/6eeacfd9-93b6-4ef1-bdf1-35cb220e2e15)

* Under ***"NIC network security group"*** we will be selecting ***"Advanced"*** and setting up our own network rules.
  * This will act as a kind of firewall, but rather than hardening our VM as we normally would with good firewall rules, we will be opening up all the ports and making the system vulnerable to attacks.
* Click on ***"Create new"*** to create a new security group.

This will open up the following page:

![Step 6-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/a0d2b9d7-2068-4c07-9448-e48b5b32c8be)

* Under ***"Inbound rules"*** we want to delete the existing rules by clicking on the trash icon.
* Then we want to click on ***+ Add an inbound rule***
  * This will allow us to create our own "inbound rules" that will make our VM completely open and exposed to the internet.
  * The only things we want to change here is ***"Destination port ranges,"*** which we will set to "all" by typing in an asterisk "*"
  * You can change the ***"Name"*** if you like, but I will be leaving mine as default.

When you are done, it should look something like this:

![Step 7-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/1fcafc01-e1e7-4955-9cee-2fee84b23692)

Click the ***"Add"*** button and now your ***"inbound rules"*** section should look like this.

![Step 8-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/e0f7c88f-7371-4c82-8c47-0acae44b7839)

* Click the ***"OK"*** button at the bottom of the screen and you will be brought back to the ***"Networking"*** tab under ***"Create a virtual machine."***
* Now click the ***"Reivew + create"*** button.
  * ***Note:*** if you get an error message about your "Basics" settings, simply click on the "Basics" tab, double check your settings and then click on the ***"Review + create"*** button again. When I got this error, my settings were all correct and I simply had to click the ***"Review + create"**** button again.

You should now have a screen that looks like this: 

![Step 9-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/63380aa0-4f0d-4842-b6f0-dd1b6d73821d)

* Verify that everything looks good and then click the ***"Create"*** button.
* And now you will have a vunerable honeypot VM, ready to be discovered and attacked by hackers.

Once the VM has been created (may take a few minutes) you should come to a page that looks something like this:

![Step 10-Create Virtual Machine](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/ac444575-1c4b-45ca-82e8-57b40d3a0663)

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

## 4. Enable VM Log Gathering in Microsoft Defender for Cloud

Now we want to enable the ability to gather logs from the VM for the Log analytics workspace that we just created.

* In the search box, search for "defender for cloud" and select ***"Microsoft Defender for Cloud."****

You should now be brought to a screen that looks like this:

![Step 14-Enable Log Gathering in Defender](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/09f55158-1305-4e07-8971-bf2ed53eefa4)
