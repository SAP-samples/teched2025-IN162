# Exercise 3.1 - Create an Integration Flow from scratch to receive a Sales Order creation event, transform into embeddings, and persist to HANA Vector DB

Make sure you come to this exercise after completing the steps in this [README.md](./README.md).

In this exercise, you will build an integration flow from scratch. Please ensure you follow the steps in the exact sequence outlined in this guide.

## Step 1 - Create a new integration flow 

1. In the Artifacts tab, click on 'Add' -> 'Integration Flow'
   <br>![](../ex3/images/ex162-3-4.png)
2. In the create dialog, name your integration flow as:

>[!NOTE]
>Replace `000` with your assigned user identifier.


**Name**
```
'IN162-`0**` Sales Order Event to Hana Vector DB for AI Grounding'
```

**Description**

```
Integration flow to receive an event from S4/HANA and process it further
```


>[!NOTE]
>Replace `0**` with your assigned user identifier.


**Click on 'Add and Open in Editor'.**
<br>![](../ex3/images/ex162-3-5.png)

3. Now you should see the basic skeleton of the integration flow in the editor, ready for us to start building upon. 

    **Click on 'Edit' to get started.**
  <br>![](../ex3/images/ex162-3-6.png)

## Step 2 - AEM Sender Adapter to receive events from S/4HANA
In this step, we will configure the AEM Adapter to receive events from S/4HANA.

1. Connect the 'Sender' system to the 'Start' block by **click-holding and dragging your mouse pointer.** 

<br>![](../ex3/images/ex162-3-7.png)

2. A dialog with the different adapters will pop open. Select the '**AdvancedEventMesh**' adapter.

<br>![](../ex3/images/ex162-3-8.png)

3. **Click on this newly added Adapter and navigate to the 'Connection' tab** from the properties sheet at the bottom of the screen. Add the following attributes to the 'Sender Connection Details' section.
    | Field | Value |
    | ----- | ----- |
    | Host | tcps://mr-connection-sq0b51wu6s3.messaging.solace.cloud:55443 |
    | Message VPN | teched-2025-europe |
    | Username | solace-cloud-client|
    | Authentication Type | Basic |
    | Password Secure Alias | teched-2025-europe-aem-password |
   
 
   <br>![](../ex3/images/ex162-3-9.png)

4. Next, head over to the **'Processing' tab** and enter IN162-0**_Sales_Order in the Queue Name field **(replace `0**` with your assigned user identifier)**. 
   
   Set the 'Acknowledgment Mode' to 'Automatic on Exchange Complete'.
> [!IMPORTANT]
> Refer to [Exercise 1](../ex1/README.md#exercise-12---create-a-queue-in-advanced-event-mesh) where we created this Queue.
   
Leave all other attributes with their default values.

<br>![](../ex3/images/ex162-3-10.png)
We are done with the first block. 
>[!TIP]
>Keep clicking 'Save' periodically over the course of this exercise so that you don't lose your work if the browser session were to time out.

## Step 3 - Enrich the event data by fetching complete Sales Order information from S/4HANA
In the next few steps, we will enrich the sales order data received from the Adapter and prepare it for further processing.

1. **Click on the (+) Add Flow Step button** (on the 'Start' message block) to add a new step.

<br>![](../ex3/images/ex162-3-11-0.png)

2. Select a '**Groovy Script**' in the 'Add Flow Step' dialog.

<br>![](../ex3/images/ex162-3-11.png)

3. Title this step '**Log Sales Order Event Payload**' in the 'General' tab of the property sheet. This step captures and logs the payload received from the AEM Adapter. 
    
    Click on the 'Create' button of the script step to launch the script editor.

<br>![](../ex3/images/ex162-3-12-0.png)

4. Copy the following lines of code and paste them into the script editor window. (after clearing out the generated script present in the editor)
    ```groovy
    import com.sap.gateway.ip.core.customdev.util.Message

    def Message processData(Message message) {
        def body = message.getBody(java.lang.String) 
        def messageLog = messageLogFactory.getMessageLog(message)
        if (messageLog != null) {
            messageLog.addAttachmentAsString('Sales Order Event Payload', body, 'application/json')
        }
        return message
    }
    ```
    The payload will be added as an attachment and can be inspected in the 'MPL' section when the message executes.

    <br>![](../ex3/images/ex162-3-12.png)
   <br>
> [!TIP]
> Click on 'Apply' to save your changes. Notice that you may receive a warning.
> You can ignore the warning and click on 'Close' and move ahead.
> 
> Make sure to save your changes in the main IFlow editor.
    
<br>

5. Next, after the script step, go ahead and add a new Flow Step.

<br>![](../ex3/images/ex162-3-13-1.png)

6. Add a '**JSON to XML Converter**' step. The default settings of this step are sufficient. No additional settings are needed in the property sheet.

    In this step, we are converting the JSON representation of the notification event to its XML equivalent so that it can be easily extracted later.

<br>![](../ex3/images/ex162-3-13-2.png)

7. Next, after this Converter step, go ahead and add a new flow step.

<br>![](../ex3/images/ex162-3-14.png)
8. Select the '**Content Modifier**' step in this dialog that pops out.

<br>![](../ex3/images/ex162-3-15.png)
<br>
<br>

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-16-1.png)


9. Title this step as 'Extract Sales Order ID'. As you may have guessed, we will extract the Sales Order ID from the XML document's XPath expression.
   
    Go to the '**Exchange Property**' tab of the property sheet of this step. Click on 'Add' twice to add two Properties.
<br>![](../ex3/images/ex162-3-16-2.png)
<br>![](../ex3/images/ex162-3-16-3.png)

11. Copy the values from the table below for the Property settings.
    | Action | Name | Source Type | Source Value | Data Type |
    | ----- | ----- | ----- | ----- | ----- |
    | Create | salesOrderID | XPath | `root/data/SalesOrder` | java.lang.String
    | Create | assignedParticipantID | Constant | IN162-`0**` (replace `0**` with your assiged participant ID)|
      
<br>![](../ex3/images/ex162-3-17.png)

## Step 4 - Call the S/4HANA system to get the full data object
As you may have observed, the event triggered upon Sales Order creation provides only the Sales Order ID — essentially serving as a notification event. To retrieve the complete details, we must use this ID to query the S/4HANA system and obtain the full set of Sales Order attributes.

1. Add a new Flow Step after the 'Extract Sales Order ID' content modifier step.
<br>![](../ex3/images/ex162-3-18.png)

2. Select 'Request-Reply' step in the 'Add Flow Step' dialog.

<br>![](../ex3/images/ex162-3-19.png)

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-20-1.png)

3. Title the Request-Reply step as 'Get Sales Order Details'. Next, hold and drag the 'Receiver' shape from the right corner of the editor and place it near the 'request-reply' shape.

<br>![](../ex3/images/ex162-3-20-2.png)

4. Point your attention to the 'Connector' button. 

<br>![](../ex3/images/ex162-3-21-0.png)

6. Click and hold on the 'Connector' button and drag it all the way down onto the 'Receiver' shape, and release your mouse pointer after the 'ends' are joined.
<br><img src="../ex3/images/image24.png" width=100% height=100%>


7. A dialog pops out with a list of Adapters to choose from. Select the 'OData' Adapter.

<br>![](../ex3/images/ex162-3-21.png)

8. Select OData version V2.

<br>![](../ex3/images/ex162-3-22.png)

9. Click on the 'Connection' tab of the property sheet of the Adapter. Enter the following values in the 'Connection Details' section:
    | Field | Value |
    | ----- | ----- |
    | Address | `https://my427029-api.s4hana.cloud.sap/sap/opu/odata4/sap/api_salesorder/srvd_a2x/sap/salesorder/0001/` |
    | Proxy Type | `Internet` |
    | Authentication | `Basic`|
    | Credentials Name  | `s4hana_credentials` (has been pre-created) |
    

      <br>![](../ex3/images/ex162-3-23.png)

11. Next, proceed to the 'Processing' section. Enter the following values in the 'Processing Details' section:
  
    | Field | Value |
    | ----- | ----- |
    | Operation Details | Query (GET) |
    | Resource path| ````SalesOrder(SalesOrder='${property.salesOrderID}')```` |
    | Query Options | ````$select=SalesOrder,SoldToParty,SalesOrderDate,PurchaseOrderByCustomer,RequestedDeliveryDate,TotalNetAmount,TransactionCurrency&$expand=_Item($select=SalesOrder,SalesOrderItem,SalesOrderItemText,Product,RequestedQuantity,RequestedQuantityISOUnit,NetAmount,TransactionCurrency,ConfirmedDeliveryDate),_Partner($select=SalesOrder,PartnerFunction,Customer,BusinessPartnerName1,StreetName,CityName,PostalCode,Region,Country)````|


    <br>![](../ex3/images/ex162-3-24.png)

## Step 5 - Filter out 'noisy' events and keep your Sales Order data clean and accurate 
Since we have subscribed to the Sales Order Create event, an event will be emitted on the shared topic (`sap/teched/2025/ce/sap/s4/beh/salesorder/v1/SalesOrder/Created/v1`) each time a participant creates a sales order. You may recall this step from [Exercise 1](../ex1/README.md#sap/teched/2025/ce/sap/s4/beh/salesorder/v1/SalesOrder/Created/v1).

Although each participant has their own Queue subscription, the Topic itself is shared. As a result, your Queue will receive events for sales orders created by all participants, which could lead to inaccurate or misleading summarizations. To address this, we will implement additional filtering steps to remove unnecessary 'noise' and ensure the data remains relevant and accurate.

We will now create two processing routes based on the customer ID retrieved from the Sales Order. If the customer ID matches your individual participant ID, the event will be treated as valid and processed further. If it does not match, it will be considered as belonging to another participant and will be filtered out. 

1. Click on the 'Add flow step' button to add a new step.
<br>![](../ex3/images/ex162-3-25-1.png)

2. Select the 'Content Modifier' step.
<br>![](../ex3/images/ex162-3-25-2.png)
<br>

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-26-1.png)

3. Title this step as 'Extract Customer ID' in the General tab of the property sheet. In the 'Exchange Property' tab, click on 'Add' and add a new property named `customerID`. Set the Source type to `XPath`, source value to `/SalesOrder/SalesOrder_Type/PurchaseOrderByCustomer`, and the Data Type to `java.lang.String`.
<br>![](../ex3/images/ex162-3-26-2.png)
<br>![](../ex3/images/ex162-3-27.png)

4. After this step, click on 'Add Flow Step'.

<br>![](../ex3/images/ex162-3-28-1.png)

5. Select the 'Router' step in the dialog.

<br>![](../ex3/images/ex162-3-28-2.png)

6. Name the step as `Route Customer ID` in the General tab of the Property Sheet.

<br>![](../ex3/images/ex162-3-28-3.png)

7. We will create an additional route now. Go to the 'search step' text box and look for a 'content modifier' step. Click on +.

<br>![](../ex3/images/ex162-3-29.png)

8. Click and place the step right below the router step.

<br>![](../ex3/images/ex162-3-30.png)

9. Title the content modifier step as 'set Customer Status'.

<br>![](../ex3/images/ex162-3-31.png)

10. Click-hold on the 'connector' button of the router step.

<br>![](../ex3/images/ex162-3-32-1.png)

11. Drag the connector and place it on the 'set custom status' content modifier.
<br><img src="../ex3/images/image39.png" width=100% height=100%>


12. You will notice two route paths created (Route 1 and Route 2). Click on Route 1 and title it 'Assigned'.
<br>![](../ex3/images/ex162-3-32-2.png)

13. Click on the 'Assigned' path and navigate to the 'Processing' tab in the property sheet. Set the expression type to 'Non-XML' and the condition as `${property.customerID} = ${property.assignedParticipantID}`.
<br>![](../ex3/images/ex162-3-33.png)

14. Next, click on Route 2 and title it 'Others'. In the 'processing' tab, check this as the 'default' route.

<br>![](../ex3/images/ex162-3-34-1.png)
<br>![](../ex3/images/ex162-3-34-2.png)

15. Click on the 'Set Custom Status' content modifier step. Navigate to the 'Exchange Property' tab in the property sheet. Add a property titled `SAP_MessageProcessingLogCustomStatus` with the source type and value set to 'constant' and `Terminated: Customer ID mismatch` respectively.

> [!NOTE]
> We set `SAP_MessageProcessingLogCustomStatus` attribute here as an enabler to easily filter out meaningful entries from the noisy ones during the monitoring phase when we access the 'message processing logs' (MPL).
> 
<br>![](../ex3/images/ex162-3-35.png)

16. Click on 'Add Flow step' right after this content modifier step.
<br>![](../ex3/images/ex162-3-36-1.png)

17. Look up the 'Terminate Message' step in the 'add flow step' dialog.

<br>![](../ex3/images/ex162-3-36-2.png)


18. Title this step as 'Terminate'. This completes the logic for the 'others' route.
<br>![](../ex3/images/ex162-3-37.png)
<br>

> [!NOTE]
> We have intentionally set the status to 'Terminate' to gracefully end the processing cycle of the 'others' route.

19. Let's get back to the 'Assigned' route. Click on the 'Add flow step' button to add a step on this route.
<br>![](../ex3/images/ex162-3-38.png)

20. Click on the 'content modifier' step 
<br>![](../ex3/images/ex162-3-39.png)
<br>

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-40-1.png)
<br>

21. Title the step as 'Set Application ID and Custom Status' in the General tab of the property sheet. Next, go to the 'message header' tab and 'Add' a header with the following properties. 
   
   Action : `Create`, Name : `SAP_ApplicationID`, Source Type : `Property`, Source Value : `customerID`.
<br>![](../ex3/images/ex162-3-40-2.png)
<br>

22. Click on the 'Exchange Property' tab and add a Property as follows.
   
   Action: `Create`, Name: `SAP_MessageProcessingLogCustomStatus`, Source Type: `Constant`, Source Value: `Successful: Customer ID matched`.
<br>![](../ex3/images/ex162-3-41.png)

This concludes the logic to separate out the valid entries from the noisy ones.

## Step 6 - Perform a message mapping to cleanse the data 
In this step, we will utilize the 'message mapping' functionality to cleanse the quality of the sales order payload from the system. This step is needed to make the demonstration cleaner. We will concatenate and tailor certain files for better readability.

1. Click anywhere on the editor canvas (not on any flow step) to activate the 'Integration Flow' panel in the property sheet. Go to the 'References' tab, and in the Global subtab, click on the 'Add References' button and add a 'Message Mapping'.

<br>![](../ex3/images/ex162-3-42-1.png)
<br>![](../ex3/images/ex162-3-42-2.png)

2. Here, we will specify the source package to import the pre-built mapping from. Bring up the 'Package' drop-down and select 'TechEd 2025 IN162 - Solution Package' as the source.
   
   Select the 'MM_SalesOrder_S4Hana_to_HanaVectorDB' Artifact and click 'ok'.

<br>![](../ex3/images/ex162-3-43-1.png)
<br>![](../ex3/images/ex162-3-43-2.png)

3. Confirm that the mapping is successfully added.

<br>![](../ex3/images/ex162-3-44.png)

4. Head back to the 'Set Application ID and Custom Status' content modifier step and click on the 'Add Flow Step' button.
<br>![](../ex3/images/ex162-3-45.png)

5. Select 'Message Mapping' from the Add Flow Step dialog.

<br>![](../ex3/images/ex162-3-46.png)

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-47-1.png)
<br>

6. Click on the 'Assign' button. Here we will import the message mapping we referenced in the previous step.
<br>![](../ex3/images/ex162-3-47-2.png)

7. Navigate to the 'Global Resources' tab from the 'Select Mapping Resource' dialog. Click on the imported message mapping resource and select OK.

<br>![](../ex3/images/ex162-3-48-1.png)
<br>![](../ex3/images/ex162-3-48-2.png)

8. Verify that the mapping resource is listed in the 'Processing' tab of the flow step. Click on the resource; this will open a new window.
<br>![](../ex3/images/ex162-3-49-1.png)

9. You can inspect the mapping we've created. Here you can see that the SalesOrder entity from S/4HANA has been mapped to a simpler schema.
<br>![](../ex3/images/ex162-3-49-2.png)

10. For example, the 'TotalNetAmount' and 'transaction unit' attributes have been fused into a single entity for better readability. 

<br>![](../ex3/images/ex162-3-49-3.png)
<br>

## Step 7 - Prepare data payload to invoke the embedding model of the AI Service 
In this step, we will utilize the deployment URL of the AI model we consumed in [Exercise 2](../ex2/README.md) using SAP Generative Hub and AI Core's capabilities to generate text embeddings of our sales order payload.

1. Click on (+) button to add a new flow step.
<br>![](../ex3/images/ex162-3-50.png)

2. Find and select the 'XML to JSON Converter' step in the add flow step dialog.
<br>![](../ex3/images/ex162-3-51.png)

3. Go to the 'Processing' block in the property sheet of the converter step and specify the properties as instructed in the table below:
   | Field | Value |
    | ----- | ----- |
    | Use Namespace mapping | Uncheck |
    | JSON Output Encoding |  UTF-8 |
    | Suppress JSON Root Element | Check|
    | Streaming  | Check |
    | Convert XML Elements to JSON Array  | Specified Ones |
    | XML Element  | `/SalesOrder/SalesOrderItems/SalesOrderItem` |
  
    
   <br>![](../ex3/images/ex162-3-52.png)
   
4. Click on (+) to add a new Flow Step

<br>![](../ex3/images/ex162-3-53-0.png)

5. Select 'groovy script' in the Add Flow Step dialog.

<br>![](../ex3/images/ex162-3-53.png)

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-54-1.png)
<br>

6. Title the Groovy script step as 'Log Sales Order JSON Payload'. Click on the 'Create' button on the step.

<br>![](../ex3/images/ex162-3-54-2.png)
<br>

7. Copy the code below and paste it into the code editor window.
      ```groovy
    import com.sap.gateway.ip.core.customdev.util.Message
    import groovy.json.JsonOutput

    def Message processData(Message message) {
        def body = message.getBody(java.lang.String);
        def messageLog = messageLogFactory.getMessageLog(message);
        if (messageLog != null) {
            messageLog.addAttachmentAsString('Sales Order JSON Payload', body, 'application/json');
        }
        // save sales order json as a property to be used later
        message.setProperty("salesOrderJson", body);
        
        // Escape double quotes in sales order json and update the body
        def escapedSalesOrderJson = JsonOutput.toJson(body); 
        message.setBody(escapedSalesOrderJson);
        
        return message;
    }
    ```

    Click on 'Apply', ignore any warnings if presented, and 'Close' the editor
    
<br>![](../ex3/images/ex162-3-55.png)
<br>

8. After this step, click on the (+) button to add a new flow step.

<br>![](../ex3/images/ex162-3-56.png)
<br>

9. Select the 'content modifier' step in the 'add flow step' dialog.

<br>![](../ex3/images/ex162-3-57-1.png)
<br>
> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-57-2.png)
<br>

10. Title this as 'Prepare Embedding Call'. Go to the 'Message Header' tab and click 'Add' twice to prepare to add two header attributes.

<br>![](../ex3/images/ex162-3-57-3.png)
<br>

11. Manage the attribute entries as follows:
   | Action | Name | Source Type | Source Value |
    | ----- | ----- | ----- |----- |
    | Create | content-type | Constant | application/json |
    | Create |  ai-resource-group | Property | assignedParticipantID |
   
    
<br>![](../ex3/images/ex162-3-58.png)
<br>
12. Click on the 'Message Body' tab and enter the following text as an 'Expression'.
```json
    {
        "config": {
            "modules": {
                "embeddings": {
                    "model": {
                        "name": "text-embedding-3-small"
                    }
                }
            }
        },
        "input": {
            "text": ${body}
        }
    }
   ```

<br>![](../ex3/images/ex162-3-59.png)
<br>

13. Click on the (+) button after the previous step to add a new flow step.

<br>![](../ex3/images/ex162-3-60.png)
<br>

14. Select 'Request-Reply step in the 'Add Flow step' dialog.

<br>![](../ex3/images/ex162-3-61-1.png)
<br>

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-61-2.png)
<br>

15. Title this step as 'Get Text Embeddings'. Click on the 'search step' text box on the right and search for the 'Receiver' shape.

<br>![](../ex3/images/ex162-3-61-3.png)
<br>

16. Click and drag the receiver shape right below the request-reply step. You can call this box 'AI_Launchpad'.

<br>![](../ex3/images/ex162-3-62-1.png)
<br>
<br>![](../ex3/images/ex162-3-62-2.png)
<br>

17. Click on the 'connector' button and start dragging it all the way down to join the 'AI_Launchpad' Receiver box.
<br>![](../ex3/images/ex162-3-63.png)
<br>

18. Release the mouse button once the ends are joined. An 'Adapter Type' dialog will pop out.

<br>![](../ex3/images/ex162-3-64-1.png)
<br>

19. Select 'HTTP' for the 'Adapter Type'.

<br>![](../ex3/images/ex162-3-64-2.png)
<br>

20. Proceed to the 'Connection' section in the property sheet for the Adapter. Maintain the following attributes for the properties:
    
| Field | Value |
| ----- | ----- |
| Address | `https://api.ai.prod.eu-central-1.aws.ml.hana.ondemand.com/v2/inference/deployments/<your-deployment-id>/v2/embeddings` (copy the deployment id from [Exercise 2](../ex2/README.md#exercise-22---create-deployment) after you created the deployment) |
| Method | POST |
| Authentication | OAuth2Client Credentials|
| Credential Name  | `aicore_credentials` |
| Request Headers | * |


<br>![](../ex3/images/ex162-3-64-3.png)
<br>
## Step 8 - Prepare data payload to persist text embeddings into HANA Vector DB 
In this step, the generated embeddings are inserted into the SAP HANA Cloud vector database using the JDBC receiver adapter, ensuring real-time grounding of Sales Order objects.

1. After the 'Get Text Embeddings' step, click on the (+) button to 'Add a Flow Step'.
<br>![](../ex3/images/ex162-3-65.png)
<br>

2. Select 'Groovy Script' from the 'Add Flow step' dialog.

<br>![](../ex3/images/ex162-3-66.png)
<br>

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-67-1.png)
<br>

3. Title the step as 'Prepare SQL Statement'. Click on the 'Create' button on the step.

<br>![](../ex3/images/ex162-3-67-1-1.png)
<br>

4. Copy the following Groovy script and paste it into the script editor.
    ```groovy
    import com.sap.gateway.ip.core.customdev.util.Message;
    import groovy.json.JsonSlurper;

    def Message processData(Message message) {
        def body = message.getBody(java.io.Reader);
        // Parse the JSON string
        def jsonSlurper = new JsonSlurper();
        def jsonObject = jsonSlurper.parse(body);
        def embeddings = jsonObject.final_result.data.embedding[0];
        
        //Properties
        def properties = message.getProperties();
        def customerID = properties.get("customerID");
        def customerName = "BestRun " + customerID;
        def escapedSalesOrderJson = properties.get("salesOrderJson");
        
        // Build SQL Insert Statement
        def sqlInsertStatement = "INSERT INTO \"DBADMIN\".\"TechEd25_IN162_Table\" (CUSTOMER_ID, CUSTOMER_NAME, TEXT, TEXT_VECTOR) VALUES (\'${customerID}\',\'${customerName}\',\'${escapedSalesOrderJson}\',TO_REAL_VECTOR(\'${embeddings}\'))";

        message.setBody(sqlInsertStatement);
        return message;
    }
    ```
    Click on 'Apply' and after the changes are applied, click  'Close' to exit the script editor. 
    

<br>![](../ex3/images/ex162-3-67-2.png)
<br>

5. After this step, click on (+) to add a new flow step.

<br>![](../ex3/images/ex162-3-68.png)
<br>
6. Add a 'Request-Reply' step in the 'Add Flow step' dialog. Title the step as 'Insert Embeddings to Vector DB'. 
   
   Click on the 'Search Step' text box and look for a 'Receiver' step.
<br>![](../ex3/images/ex162-3-69.png)
<br>

> [!TIP]
> Format the diagram if required, select the main process, and select Arrange Horizontally
<br>![](../ex3/images/ex162-3-70-1.png)
<br>

<br>![](../ex3/images/ex162-3-70-2.png)
<br>

7. Drag and place the receiver box below the request-reply step. Title the box as 'HANA_DB'. 

<br>![](../ex3/images/ex162-3-72.png)
<br>

8. Click on the 'Connector' button, drag an arrow connecting it to the 'HANA_DB' receiver box. A dialog to select the Adapter pops out.
<br>![](../ex3/images/ex162-3-73-1.png)
<br>


9. Select 'JDBC' for the Adapter type.

<br>![](../ex3/images/ex162-3-73-2.png)
<br>

10. Proceed to the 'Connection' tab of the property sheet and enter `SAPHANACloud` in the 'JDBC Data Source Alias' box. Note that this data source alias has already been built for you.
<br>![](../ex3/images/ex162-3-74.png)
<br>

11. Make sure to save your changes. You can verify the data source by navigating to the 'Manage JDBC Material' tile in the 'Overview' section. You will find the `SAPHANACloud` data source pre-created in the 'Data Source' tab. 

<br>![](../ex3/images/ex162-3-75.png)
<br>
1. Access to the HANA Database itself is not part of this hands-on exercise, but just for your understanding, here is how the structure for the table `TechEd25_IN162_Table` has been defined in the default `DBADMIN` schema. 
<br><img src="../ex3/images/image107.png" width=80% height=100%>

## Step 9 - Deploying the IFlow
1. Congratulations! At this point,you are done with creating the IFlow. Your final model should look like the one pasted in the screenshot below. The deployment status is naturally 'Not deployed' at this point. 
   
    Click on **Save** and the '**Deploy**' button to trigger the deployment.
 
<br>![](../ex3/images/ex162-3-77-1.png)
<br>

 2. Click '**Yes**' in the deployment confirmation dialog to deploy the IFlow into the selected 'Cloud Integration' profile.
   
   <br>![](../ex3/images/ex162-3-77-2.png)
<br>

 3. Click 'OK' to close the dialog.
   
   <br>![](../ex3/images/ex162-3-77-3.png)
<br>
 
 4. Finally, you should see the deployment status change to 'Deployed'. 
   
    This concludes the exercise.
   
<br>![](../ex3/images/ex162-3-77-4.png)
<br>
## Summary

This completes Exercise 3. Next, proceed to [Exercise 4](../ex4/README.md), where we will achieve a similar flow to consume events emitted upon support case creation in SAP Service Cloud V2.
