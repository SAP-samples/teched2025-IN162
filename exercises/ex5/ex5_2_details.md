# Exercise 5.2 - Monitor Message Processing Logs in Cloud Integration after Sales Order Creation

In this exercise, we will monitor the message processing logs in the Integration Suite Monitoring tab.

1. Navigate to **'Monitor' -> 'Integration and APIs'** tab from the Integration Suite's main page. Click on the **'All Artifacts'** tile.
   
   <img src="../ex5/images/image9.png" width=100% height=100%>

2. This will bring up the **'Monitoring Message Processing'** screen. Enter your user identifier **IN162-0**** *(replace ** with the participant number that is assigned to you)* in the **'ID:'** search box.
   
   <img src="../ex5/images/image16.png" width=100% height=100%>

3. You may remember that we had assigned the **Application_ID** header only when a matching condition was met. Hence, you will see the message entries filtered only for your individual execution runs. You can verify that the **'Custom Status'** will always point to **'Successful: Customer ID matched'**.

   <img src="../ex5/images/image17.png" width=100% height=100%>

4. Scroll down further, and in the **'Attachments'** section, you will be able to see 2 files as attachments. These files represent the incoming event payload and the final flattened JSON payload produced after the message mapping step. 

   <img src="../ex5/images/image18.png" width=100% height=100%>

5. Click on the **'Sales Order Event Payload'** link. This will render the attachment in a new window. You can verify that the value of the `SalesOrder` attribute matches the one that we had created in the final step of the previous exercise.

   <img src="../ex5/images/image19.png" width=100% height=100%>

6. Go ahead and click on the **'Sales Order JSON Payload'** link. You will recall that this is the flattened payload we generated after the **'message mapping'** step. You can verify that the value of the `SalesOrder` attribute matches the one that we had created in the final step of the previous exercise.

   <img src="../ex5/images/image20.png" width=100% height=100%>

7. Though inspecting the HANA Database is not part of our hands-on execise, here is a screenshot of the `TechEd25_IN162_Table` table in HANA, that is populated with the SalesOrder payload and the corresponding vectorized text embeddings.
    <img src="../ex5/images/image25.png" width=100% height=100%>

## Summary
This concludes the monitoring aspects of the Sales Order object. In the [next exercise](./ex5_3_details.md), we will set things up to trigger a Support Case in SAP Service Cloud Version 2 and monitor it in SAP Integration Suite.
