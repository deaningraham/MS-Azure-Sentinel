This module guides you through Analytics Rules in Microsoft Sentinel, and shows you how to create different types of rules for security detections.

## Enable an Azure Activity rule

- Head back over to ***Microsoft Sentinel,*** click on ***Analytics*** located under ***Configuration***

![Step 47-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/10ab46b7-3445-43ca-ad86-8d4db780ab22)

- Click the ***Rule Templates*** tab if not selected. Rule templates are installed here by content hub solutions. We can create active rules based on the templates supplied here, or create them from scratch.
- Click the ***Add Filter*** button, and select ***Source name*** as the filter, then ***Azure Activity*** and click ***Apply.***
- Find and click the template ***"Suspicious Resource deployment"*** and look at the short summary description in the panel on the right:
  - The default Severity is specified as Low
  - The Description describes the intent of the rule
  - It uses the Azure Activity data source, and there's a green "connected" icon showing that the AzureActivity table is available
  - The template specifies the tactic of Impact for threat categorization and reporting
  - A preview of the KQL query which runs the detection is shown in the Rule query area for quick inspection
  - We're going to create this rule with the default parameters in place. Click Create rule.
 
![Step 48-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/023fab9e-8cd6-45a7-994d-4b6769be7327)

After clicking the ***Create rule*** button, you should arrive at the ***Analytics rule wizard"*** page:

![Step 49-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/16dfe2dd-605f-4837-b8bc-109b3898b61d)

- Make sure that the ***Status*** is enabled and then click on the ***Next: Set rule logic>*** button.

On the Set rule logic screen, you have the ability to create or modify the KQL query, control entity mapping, enable and adjust alert grouping, and define the scheduling and lookback time range.

Some highlights: (From Microsoft)

- Expand the query panel using the arrows above the query (top right)
- Pop the query open, review the output and edit the KQL using View query results
- Simulate alert volume from the rule by using the Results simulation area's Test with current data button
  
![Step 50-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/13822812-aebf-44e4-b812-93f4cde45454)

Click ***Next: Incident settings*** at the bottom (or the Incident settings tab at the top) to continue the wizard.

![Step 51-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/14783e53-e357-4f03-8b36-2ac24a8cd8b6)

On the Incident settings tab, you’ll see that Incident creation is enabled, while Alert grouping is disabled. Not all alerts detected by Sentinel need to be promoted to incidents, especially if they are particularly noisy. These settings can be adjusted later if needed.
   
- This rule looks good as it is, so we’ll proceed to rule creation. Click on the ***Review and Create*** tab to continue.
- Review and then click ***save*** at the bottom of the page.

## Enable a Microsoft incident creation rule for Microsoft Defender for Cloud

Microsoft Sentinel, a cloud-native SIEM, acts as a unified interface for alert and event correlation. To ingest and display alerts from Microsoft Security products, we can create a Microsoft incident creation rule. In this section, we will explore this feature and create a sample rule that uses a filtering option to promote only Medium and High severity Defender for Cloud alerts to Sentinel incidents. This helps reduce alert fatigue for analysts.

Note: In Module 1, we enabled the Microsoft Defender for Cloud connector to sync security alerts with Sentinel. While you can create the rule as described below without enabling the connector, alerts will not be ingested or promoted without it.

- From the Microsoft Sentinel main page, click Analytics.
- Click on Create and then Microsoft incident creation rule.

![Step 52-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/70e92a2f-c84e-4ab6-9d7d-687710f79714)

- For the rule name, enter "Defender for Cloud - only Medium and High Alerts"
- From the Microsoft security service dropdown, select Microsoft Defender for Cloud
- In the Filter by severity select custom and mark High and Medium

![Step 53-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/f0fe8fd1-2107-4603-8ea7-75a4b848ca07)

- Click the ***Next: Automated response*** button.
  - On the Automated response page you can attach automation rules which can assist SOCs with repetitive tasks, incident enrichment, and security remediation. 

![Step 54-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/dc325a98-776f-4d59-8f41-c9baadd86a4d)

- Click ***Next: Review and create*** and then ***Save*** to enable the rule.

<!--- ## Review Fusion Rule (Advanced Multistage Attack Detection)

[From Microsoft] The Fusion rule is a unique kind of detection rule: Using the Fusion rule, Microsoft Sentinel can automatically detect multistage attacks by identifying combinations of anomalous behaviors and suspicious activities/alerts that are observed at various stages across a kill-chain.

In this section we will review the Fusion rule in Microsoft Sentinel.


- Under the ***"Rules templates"*** tab on the Microsoft Sentinal Analytics page, look for the filter ***Source name: Azure Activity*** and click on the "x" to remove the filter.
- Now click ***Add filter*** then ***Rule type***
- Select ***Fusion*** and then click ***Apply***
 
![Step 55-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/622278e7-f558-4776-85f1-99d9677ee554)

You should now see this. Double check that you have removed the old Azure Activity filter.

![Step 55-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/bf23064b-fb97-42e4-9053-45aad499d9ff)

Notice the tag IN USE, as the Fusion rule is enabled by default.--->

## LAB EXERCISE: Create a Microsoft Sentinel Custom Analytics Rule
Lab scenario comes from Sentinal Training Guide, but screenshots and work is my own.

#### Scenario
A security consultant notifies you about some intelligence they came across, around malicious inbox rule manipulation, based on some threads they read online (like https://www.reddit.com/r/sysadmin/comments/7kyp0a/recent_phishing_attempts_my_experience_and_what/). The attack vector they've described seems to be:
- Office activity
- Creation of new inbox rules
- Certain keywords being included in the rules being created

Based on the attack vector and your organizational risk, he recommends you create a detection rule for this malicious activity.
In this exercise you will use the Microsoft Sentinel analytics rule wizard to create a new detection rule.

**Important note:** in this lab we will use the **custom logs** we onboarded as part of the training lab installation, and we'll replace the usual table names and data types with our custom data sources. The Sentinel Training Lab content package also adds a custom rule as described below for reference.

#### Set up the detection

1. Review the article at the above link and consider what data source(s) would need to be part of a detection.
   
2. Check whether the operations you're interested in are already captured as part of your collection strategy:
   - Open the Sentinel **Logs** area and navigate to a query tab
   - Run the search query below to see a list of activities Sentinel ingested
     - Note: you may need to adjust the *Time range* to more than the last 24h if the training lab content was installed longer ago
	
    ```KQL
	OfficeActivity_CL
	| distinct Operation_s
    ```

   - As you can see, a **New-InboxRule** operation is indeed captured in your logs:
     
![Step 57-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/3b75771c-956f-40e2-8b8b-72fff7883531)

3. Click into **Analytics**, click the **Create** button in the action bar at the top, and pick **Scheduled query rule**

![Step 58-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/028d98de-5ab9-4e0f-b887-7e924e3d28a4)

5. For the **Name**, type *Malicious Inbox Rule - student* 
   
   Tip: You can use your name if you're sharing a workspace.
   
6. In the rule **Description**, state the intent of the rule: *This rule detects creation of inbox rules which attempt to Delete or Junk warnings about compromised emails sent to user mailboxes*.
   
7. Under **Tactics** tick *Persistence* and *Defense Evasion*.
   
![Step 60-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/a83cb5f5-46e8-43d5-a753-1b70dbc3043b)
   
9.  For rule **severity** select *Medium*.

![Step 59-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/18c8f1b0-804e-491f-958c-67106ea03155)

11.  Press **Next: SET rule logic**.
    
12.  In the **Rule logic** page, review and copy the query below:

	```KQL
	let Keywords = dynamic(["helpdesk", " alert", " suspicious", "fake", "malicious", "phishing", "spam", "do not click", "do not open", "hijacked", "Fatal"]);
	OfficeActivity_CL
	| where Operation_s =~ "New-InboxRule"
	| where Parameters_s has "Deleted Items" or Parameters_s has "Junk Email" 
	| extend Events=todynamic(Parameters_s)
	| parse Events with * "SubjectContainsWords" SubjectContainsWords '}'*
	| parse Events with * "BodyContainsWords" BodyContainsWords '}'*
	| parse Events with * "SubjectOrBodyContainsWords" SubjectOrBodyContainsWords '}'*
	| where SubjectContainsWords has_any (Keywords)
	or BodyContainsWords has_any (Keywords)
	or SubjectOrBodyContainsWords has_any (Keywords)
	| extend ClientIPAddress = case( ClientIP_s has ".", tostring(split(ClientIP_s,":")[0]), ClientIP_s has "[", tostring(trim_start(@'[[]',tostring(split(ClientIP_s,"]")[0]))), ClientIP_s )
	| extend Keyword = iff(isnotempty(SubjectContainsWords), SubjectContainsWords, (iff(isnotempty(BodyContainsWords),BodyContainsWords,SubjectOrBodyContainsWords )))
	| extend RuleDetail = case(OfficeObjectId_s contains '/' , tostring(split(OfficeObjectId_s, '/')[-1]) , tostring(split(OfficeObjectId_s, '\\')[-1]))
	| summarize count(), StartTimeUtc = min(TimeGenerated), EndTimeUtc = max(TimeGenerated) by  Operation_s, UserId__s, ClientIPAddress, ResultStatus_s, Keyword, OriginatingServer_s, OfficeObjectId_s, RuleDetail
	```
![Step 61-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/4f88effc-e9f8-4d03-a670-994a891310dc)

11.  We can check how many hits the query finds using the **Test with current data** button on the right side, which helps with Alert modelling and tuning.

![Step 62-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/a0c62e37-1a98-45de-aba9-d7aa77d44615)

13.   Under **Alert enhancement**, expand the **Entity mapping** section, which allows us to map fields to well-known categories (entity types):

![Step 63-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/5f6d41e1-52f2-4e1e-bd30-9951f1c8611f)

      - Click **Add new entity**, and then pick **Account** as our first type. Fill out the identifiers as follows:
        - In the left-hand selector, pick **FullName** as the type, and set the right-hand column identifier to **UserId__s**
          - This tells Sentinel that we have some sort of *Account*, specified by its *full name*, in the column *UserId_s*
      - Press **Add new entity** and this time select **Host** 
        - In the left-hand selector, select **FullName**, and map it to **OriginatingServer_s** in the right column
      - Press **Add new entity**, and pick the **IP** entity type
        - In the left-hand selector, pick **Address**, and map it to the **ClientIPAddress** value
	
  - Your mapping should look like the below:

![Step 64-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/c103b56b-cebf-426d-a190-91d134091c63)

#### Add a customized Alert title

Off to a great start! Now, to make your SOC more productive, save analyst time, and help quickly and effectively triage newly-created incidents, a SOC analyst asks you to provide the affected user from the search results as part of the alert title.

1. To achieve this, we'll use the **Alert details** feature and use a custom Alert Name Format.

![Step 65-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/54bafd3b-11dc-4180-8160-ba67bb64077d)

   - As we know the username is available in the UserId_s column, under the **Alert Details** heading, in the **Alert Name Format** section, provide the dynamic title **"Malicious Inbox Rule - {{UserId__s}}"**

![Step 66 -Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/fee26955-2cae-4042-9bfe-74d9c9b655dd)

3. Under **Query scheduling**, set the **run query every** to **5 minutes** and the **Lookup data to last 12 Hours**. 
   - If you deployed the lab more than 12 hours ago, you will need to change the lookback period in order to cover that period.
   - (This scheduling might not be ideal for production environment and should be tuned appropriately)

![Step 67-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/6127f919-bf5a-48be-8d55-840d03f42192)

4. Leave the other values at defaults.

5. Click the **Incident settings** button or the tab at the top.

6. As your SOC is under stress, we want to *reduce the number of alerts* and be sure that when analyst handle a specific incident, he/she will see all related events or other incidents related to the same attack story. For that we will implement the **Alert grouping** feature. To do so, follow the steps below: 

- On the **Incident settings** tab under **Alert grouping**: 
  - Set *Group related alerts into incidents* to **Enabled**.
  - Set *Limit the group to alerts created within the selected time frame* to **12 hours**.
  - Set the *Group alerts triggered by*... setting to *Grouping alerts into a single incident if the selected entity types and details matches*, then pick only the Account.

![Step 68-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/b9bf487e-669c-48ab-aa35-92ffd473e86f)

6. Skip to **Review and create** and **Save** your new Analytics rule.

The next time the rule is scheduled - *as long as the training data was onboarded within its lookback period* - it should find the logs from the OfficeActivity_CL custom log table and raise grouped Alerts as Incidents. 

**Tip:** You should already be able to see the sample Incident with a similar outcome from the rule installed when the Training Lab content was onboarded (if it was onboarded recently).
 	
![Step 69-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/5b3402d7-6cec-4872-9921-66c2003b0297)

## Review resulting security incident

Now we've created a custom Analytics rule to detect malicious inbox rule creation, let's quickly review an Incident produced by a similar Analytics rule which was installed and run when this training lab content was onboarded. 

**Note:** The next module provides more detail on incident investigation.

#### Find the Incident

1. From your Microsoft Sentinel instance, select **Incidents** and review the incident page.
   
2. Find the incident with the title **"Malicious Inbox Rule, affected user AdeleV@contoso.OnMicrosoft.com"** or similar - notice that the title adapted to the user affected by the detection.

3. In the right pane we can review the incident preview, this view will gave us high level overview on the incident and the entity that related to it.

	There's a lot of information in this quick view panel!
	
	- At the top: Title, ID, assignment to a user, status (New/Active/Closed) and Severity
	- The Description provided by the rule (providing good descriptions helps analyst understand detections)
	- Any Alert products involved (here, just Sentinel)
	- A count and quick links to Events, Alerts and any Bookmarks added to the incident
	- Incident update and creation times
	- Quick links to entity pages for Accounts (here Adele's account - but also potentially Hosts, IPs, and more)
	- Tactics noted by the rule definition
	- A link to the Incident Overview workbook, which provides a live report on the Incident state
	- A direct link to edit the rule producing the detection, in case you need to edit the rule/detection logic quickly
	- Quick tagging and tag display
	- A direct link to the incident
	- The most recent comment, if present

![Step 70-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/ffed29dc-a77f-4704-a760-5ebfe15948f8)

So you can you can see a snapshot of the most critical information and perform basic incident management directly from the summary panel.

4. Click **View full details** at the bottom

![Step 71-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/c7241145-bf0b-4518-9027-2feaecf46023)

5. On the full **Incident page** (Preview), you can find:
   - The same quick panel (now on the left)
   - The **Incident Actions** button at the top right
   - The **Overview tab** which includes:
     - An **Incident timeline panel**, which shows any Alerts and Bookmarks associated with that incident in a timeline view
     - An **Entities panel** allowing a quick pivot to an entity on the Entities tab
   - The **Entities tab** allowing exploration of Entity insights and timelines directly
   - A list of **Similar incidents** at the bottom of the page for added context, if available
   - A set of **Incident insights** in the right-hand panel, if available
  
  
6. After looking over the Incident, click the **Entities** tab to list all the mapped entities related to this incident.

![Step 72-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/e1a2c595-f9e2-4028-9089-d1c764308e43)

8. Click the entity **"AdeleV@contoso.OnMicrosoft.com"** from the incident Entities list
   - This action will navigate to the **Entities tab**, a list of entities with 3 sub-tabs on the right.
   - Clicking each tab will show different information about the Entity and its state
     - Note: State information is pulled from various sources: Some may be pulled directly from Azure services (like Microsoft Entra ID or Azure Virtual Machine host information), some from onboarded data sources, and some from UEBA (if enabled). Note that for *AdeleV*, our fake user, there's no information available in your user directory, but for production environments much more information is typically available!
    
![Step 73-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/1f3f0027-bd31-437b-aefa-6fdd262232bc)

![Step 74-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/ac255e01-2b8d-49f9-b7c5-63f3670f5c19)
   
- Clicking the **View Full Details** button at the bottom will take you to the full Entity page

![Step 75-Create Sentinel Lab](https://github.com/deaningraham/MS-Azure-Sentinel/assets/173529885/3700b9f5-dac8-4cfc-927e-7a68b78709c6)

