# Exercise 5.4 - Monitor Message Processing Logs in Cloud Integration after Support Case Creation

In this exercise, we will monitor the message processing logs in the Integration Suite Monitoring tab.

1. Navigate to **'Monitor' -> 'Integration and APIs'** tab from the Integration Suite's main page. Click on the **'All Artifacts'** tile.

   ![](../ex5/images/image9.png)

2. This will bring up the **'Monitoring Message Processing'** screen. Enter your user identifier **IN162-0**** *(replace ** with the participant number that is assigned to you)* in the **'ID:'** search box.

   ![](../ex5/images/image10.png)

3. You may remember that we had assigned the **Application_ID** header only when a matching condition was met. Hence, you will see the message entries filtered only for your individual execution runs. You can verify that the **'Custom Status'** will always point to **'Successful: Customer ID matched'**.

   ![](../ex5/images/image11.png)

4. Scroll down further, and in the **'Attachments'** section, you will be able to see 2 files as attachments. These files represent the incoming event payload and the final flattened JSON payload produced after the message mapping step. 

   ![](../ex5/images/image12png.png)
   
5. If you are curious to see the entries that correspond to other participants and were discarded, go back to the **Overview -> Monitoring Message Processing** tile. Click on the expand **'>'** button located in the top left side of the screen to open a detailed panel.
   
   ![](../ex5/images/image13.png)

6. Look for the **'Custom Status'** drop-down and select **'Terminated: Customer ID mismatch'** from the value help.

   ![](../ex5/images/image14.png)

8. You can see messages to the same Integration Flow, but classified with the **'Terminated: Customer ID mismatch'** custom status. Additionally, you can look at the 'attachments' section and verify that the `SalesOrder` attribute carries a different identifier from yours.

   ![](../ex5/images/image15.png)

## Summary
This concludes the monitoring aspects of the Support Case object. In the [next exercise](../ex6/README.md), let's go ahead and provide a prompt in the Joule-powered customer success digital assistant to retrieve the summarizations based on these latest sales orders and suport cases.
