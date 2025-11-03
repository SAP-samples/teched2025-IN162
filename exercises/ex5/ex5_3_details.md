# Exercise 5.3 - Create a new Support Case in SAP Service Cloud Version 2 system

In this exercise, we will create a support case in SAP Service Cloud Version 2 that will eventually trigger the integration flos that we built in [Exercise 4](../ex4/README.md)

## Step 1 - Log on to SAP Service Cloud Version 2 system

1.  Log on to [SAP Service Cloud Version 2](https://my1001903.de1.demo.crm.cloud.sap/) tenant. Log on to the same browser window where you would have previously logged into the [AEM](https://eu10.console.pubsub.em.services.cloud.sap/login?tenant-id=8b4a1697-2b58-4571-a986-1377cc070073) tenant as part of [Exercise 1](../ex1/README.md#exercise-11---log-into-advanced-event-mesh-and-explore-it)

    > [Note:]
    > <br>The reason why you are expected to use the same window is that the login session to SAP Service Cloud utilizes SAML browser cookies to extend SSO from AEM.
    > 
    > If you have closed all browser windows, make sure to log into AEM and then open SAP Service Cloud Version 2 in a separate tab of the same window.
    > 
    Click on **'Sign in using SSO'**.

    <img src="../ex5/images/image6.png" width=100% height=100%>

2.  The system will log you in.

    <img src="../ex5/images/image7.png" width=50% height=100%>

3.  Click on **Create Case** tile.

    <img src="../ex5/images/image1.png" width=80% height=100%>

4.  In the **'new case'** window, use the pre-configured Case Type. Use the table to enter the values:
    | Field | Value |
    | ----- | ----- |
    | **Case Type** | Standard Service for TechEd 2025 IN162 Hands-on Session |
    | **Subject** | Damaged goods received in Order#1620** (replace `**` with your assigned participant number) |
    | **Support Case By Customer** | IN162-`0**` (replace `**` with your assigned participant number) |
    | **Priority** | Urgent |
    | **Escalation Status** | Escalated |
    | **Description** | Dear Support Team, <br><br>I am writing to report that some items in my recent order, #1620** (replace `**` with your assigned participant number), arrived damaged. The order was received on 30th of October 2025. I have attached photos showing the damage for your review. Please advise on the next steps to resolve this issue, such as a replacement or refund. <br><br>Thank you for your assistance. |
   
    <img src="../ex5/images/image2.png" width=70% height=100%>

5.  Click on the F4 (drop-down) button for the **'Account'** section.

    <img src="../ex5/images/image3.png" width=70% height=100%>

6.  A dialog loads all the configured Accounts. From the list, select **‘BestRun’** to proceed.

    <img src="../ex5/images/image4.png" width=60% height=100%>

7.  You will see that all other fields, like **'Contact'**, **'Assigned To'**, etc., will be auto filled based on your selection. Retain these values and finalize the settings by creating **'Save and Open'**. 

    <img src="../ex5/images/image5.png" width=60% height=60%>
    
## Summary
This wraps up the Support Case creation process. An event would have been triggered now based on the settings we [defined](../ex1/README.md#exercise-13---create-second-queue-and-subscribe-to-support-case-topic-in-sap-integration-suite-advanced-event-mesh-aem) on the AEM side. The notification will be picked up by the AEM Adapter and trigger the integration flow.

Once the flow completes, we can review the execution path in the [next exercise](./ex5_4_details.md).
