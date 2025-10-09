# Exercise 5.2 - Monitor Integration Processing Logs for Sales Order creation

In this exercise, we will monitor the processing logs from the Integration Suite's Monitoring tab

1. Navigate to 'Monitor' -> 'Integration and APIs' tab from the Integration Suite's main page. Click on the 'All Artifacts' tile.
   <br>![](../ex5/images/image9.png)

2. This will bring up the 'Monitoring Message Processing' screen. Enter your user identifier (e.g., IN162-000) in the 'ID:' search box. 
   <br>![](../ex5/images/image16.png)

3. You may remember that we had assigned the Application_ID header only when a matching condition was met. Hence, you will see the message entries filtered only for your individual execution runs. You can verify that the 'Custom Status' will always point to 'Successful: Customer ID matched'.
   <br>![](../ex5/images/image17.png)

4. Scroll down further, and in the 'Attachments' section, you will be able to see 2 files as attachments. These files are the representation of the incoming event payload and the final flattened payload we generated after the message mapping step. 
    <br>![](../ex5/images/image18.png)

5. Click on the 'Sales Order Event Payload' link. This will render the attachment in a new window. You can verify that the value of the `SalesOrder` attribute matches the one that we had created in the final step of the previous exercise.
    <br>![](../ex5/images/image19.png)

6. Go ahead and click on the 'Sales Order JSON Payload' link. You will recall that this is the flattened payload we generated after the 'message mapping' step. You can verify that the value of the `SalesOrder` attribute matches the one that we had created in the final step of the previous exercise.
    <br>![](../ex5/images/image20.png)

7. If you are curious to see the entries that correspond to other participants and were discarded, go back to the Overview -> Monitoring Message Processing tile. Click on the '>' button located in the top left side of the screen to open a detailed panel.
   <br>![](../ex5/images/image21.png)

7. Look for the 'Custom Status' drop-down and select 'Terminated: Customer ID mismatch' from the value help.
   <br>![](../ex5/images/image22.png)

8. You can see messages to the same IFlow, but classified with the 'Terminated' custom status. Additionally, you can look at the 'attachments' section and verify that the `SalesOrder` attribute carries a different identifier from yours. 
   <br>![](../ex5/images/image23.png)

   <br>![](../ex5/images/image24.png)

## Summary
This concludes the 'monitoring' aspects of the Sales Order object. In the [next exercise](./ex5_3_details.md), we will set things up to trigger a Support Case in SAP Service Cloud Version 2 and monitor it in SAP Integration Suite.
