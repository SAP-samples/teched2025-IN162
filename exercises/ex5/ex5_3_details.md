# Exercise 5.3 - Create a new support case  in SAP Sales and Service Cloud

In this exercise we will create a support case in SAP Sales and Service Cloud that will eventually trigger the IFlow we built in [Exercise 4](../ex4/README.md)

## Step 1 - Log into SAP Sales and Service Cloud

1.  Paste the URL : `https://my1001903.de1.demo.crm.cloud.sap/` in your browser window where you would have previous logged into the AEM URL (`https://eu10.console.pubsub.em.services.cloud.sap/login?tenant-id=8b4a1697-2b58-4571-a986-1377cc070073`) as part of [Exercise 1](../ex1/README.md#exercise-11---log-into-advanced-event-mesh-and-explore-it)
> [!IMPORTANT]
> The reason why you are expected to use the same windows is beacuse the login session to SAP Service Cloud utilizes SAML browser cookies to extend SSO from AEM.
> 
> If you have closed all browser windows, make sure to log into AEM and then open Service Cloud in a separate tab of the same window.
> 
Click on 'Sign in using SSO'.
<br>![](../ex5/images/image6.png)

2.  The system will log you in.
    <br>![](../ex5/images/image7.png)

3.  Click on 'Create Case'.
    <br>![](../ex5/images/image1.png)

4.  In the 'new case' window, use the pre-configured Case Type. Use the table to enter the values:
    | Field | Value |
    | ----- | ----- |
    | Case Type | Standard Service for TechEd 2025 IN162 Hands-on Session |
    | Subject | Damaged goods received in Order#16200 |
    | Support Case By Customer | IN162-`000` (replace `000` with your assigned parcipant ID) |
    | Priority | Urgent |
    | Escalation Status | Escalated |
    | Description | Dear Support Team, I am writing to report that some items in my recent order, #16200, arrived damaged. The order was received on 20th of September 2025. I have attached photos showing the damage for your review. Please advise on the next steps to resolve this issue, such as a replacement or refund. Thank you for your assistance. |
   
    <br>![](../ex5/images/image2.png)

5.  Click on the F4 (drop-down) button for the 'Account' section.
    <br>![](../ex5/images/image3.png)

6.  A dialog loads all the configured Accounts. Make sure to select 'BestRun' from the list.
    <br>![](../ex5/images/image4.png)

7.  You will see that all other fields like 'Contact', 'Assigned To' etc. will be filled based on your section. Retain these values. Finalize the settings by creating 'Save and Open'. 
    <br>![](../ex5/images/image5.png)
    

    ## Summary
    This wraps up the 'support case' creation process. An event would have been triggered now based on the settings we [defined](../ex1/README.md#exercise-13---create-second-queue-and-subscribe-to-support-case-topic-in-sap-integration-suite-advanced-event-mesh-aem) on the AEM side. The notification will be picked up the the AEM Adapter and trigger the IFlow.

    Once the flow completes, we can review the execution path in the next exercise.



