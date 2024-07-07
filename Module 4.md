# Module 4 - Incident Management

This module guides you through the SOC Analyst experience using Microsoft Sentinel's incident management capabilities.

### Exercise 1: Reviewing Microsoft Sentinel incident tools and capabilities

As a SOC Analyst, your entry point to work on Security incidents (i.e. tickets/jobs/cases) in Sentinel is the **Incidents** page.

1. From the Sentinel workspace, click **Incidents** to open the incidents page. This page will show all the open incidents in the last 24 hours by default (configurable).

2. We can **filter** our view of incidents. When we want to:
   - change the time window
   - see incidents of a specific severity
   - see *Closed* incidents as well as *New* and *Active* incidents
  
    we can use the **filters bar**:

3. **Find** and **click** the incident titled *Sign-ins from IPs that attempt sign-ins to disabled accounts*. In the right-hand pane you can see the incident preview with high-level information shown.
   
4. As you are the SME SOC analyst investigating incidents, you need to *take ownership* of this incident. In the right pane, change the **Owner** from *Unassigned* to *Assign to me*, and click **Apply**.
   
5. Also change the **Status** from *New* to *Active* while you're working on the incident and click **Apply**.
 
    ![Assign indicent to yourself](../Images/m5-assigen_ticket.gif?raw=true)

6. Another way to review incidents - and also to get a high-level view on general SOC health - is through the *Security efficiency workbook*. We have 2 options to open this workbook:

- Through the **link in the top action bar** in the Incidents page - this will open the workbook's general view, where we see overall statistics for incidents:
- ![Security Operations Efficiency General view](../Images/m5-SecurityOperationsEfficiency.gif?raw=true)

- Through the **link in the incident**, which opens the same workbook to a different tab, presenting the information and lifecycle for the selected incident.
- ![Security Operations Efficiency Incident view](../Images/m5-SecurityOperationsEfficiency_incident.gif?raw=true)

7. Review the workbook views.
