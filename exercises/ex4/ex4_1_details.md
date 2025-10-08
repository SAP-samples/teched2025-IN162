# Exercise 4.1 - Create an IFlow from scratch to receive a Support Case creation event, transform into embeddings, and persist to HANA Vector DB

In this exercise, you will build an IFlow from scratch. Please ensure you follow the steps in the exact sequence outlined in this guide.

## Step 1 - Create a new IFlow

1.  In the Artifacts tab, click on 'Add' -> 'Integration Flow'
<br>![](../ex4/images/image-1.png)

1. In the create dialog, name your integration flow as: 'IN162-`000` Support Case Event to Hana Vector DB for AI Grounding' (replace `000` with your assigned user identifier). 

    Click on 'Add and Open in Editor'.
<br>![](../ex4/images/image-2.png)

1. Now you should see the basic skeleton of the IFlow ready for us to start building upon. 

    Click on 'Edit' to get started.
<br>![](../ex4/images/image-3.png)

## Step 2 - AEM Sender Adapter to receive events from Support Cloud system
In this step, we will configure the AEM Adapter to receive events from the Support Cloud System
1. Connect the 'Sender' system (titled AEM) to the 'Start' event by holding and dragging your mouse pointer. 
<br>![](../ex4/images/image-4.png)

1. A dialog with the different adapters will appear. Select the 'AdvancedEventMesh' adapter.
<br>![](../ex4/images/image-5.png)

1. Select this newly added Adapter and click on the 'Connection' tab from the properties sheet at the bottom of the screen. Add the following attributes to the 'Sender Connection Details' section.
    | Field | Value |
    | ----- | ----- |
    | Host | tcps://mr-connection-sq0b51wu6s3.messaging.solace.cloud:55443 |
    | Message VPN | teched-2025-europe |
    | Username | solace-cloud-client|
    | Authentication Type | Basic |
    | Password Secure Alias | teched-2025-europe-aem-password |

    <br>![](../ex4/images/image-6.png)

1.  Next, head over to the 'Processing' tab and enter 'IN162-`000`_Support_Case' in the Queue Name field (replace `000` with your assigned user identifier). 
> [!IMPORTANT]
> Refer to [Exercise 1](../ex1/README.md#exercise-14---create-an-additional-queue-and-queue-subscription-in-advanced-event-mesh) where we created this Queue. Make sure you enter the same Queue name.
   
   
Leave all other attributes with their default values.
<br>![](../ex4/images/image-7.png)

We are done with the first block. 
>[!TIP]
>Keep clicking 'Save' periodically over the course of this exercise so that you don't lose your work if the browser session were to time out.

## Step 3 - Massage the data event received from the Adapter
In the next few steps, we will enrich the support case data received from the Adapter and prepare it for further processing.
1. Click on the (+) Add Flow Step button to add a new step.
<br>![](../ex4/images/image-8.png)

1. Select a 'Groovy Script' in the Add Flow Step dialog.
<br>![](../ex4/images/image-9.png)

1. Title this step 'Log Support Case Event Payload' in the 'General' tab of the property sheet. This step captures and logs the payload received from the AEM Adapter.

    <br>![](../ex4/images/image-10.png)

1. Copy the following lines of code and paste them into the script editor window (after clearing out the generated script present in the editor)
    ```groovy
    import com.sap.gateway.ip.core.customdev.util.Message

    def Message processData(Message message) {
        def body = message.getBody(java.lang.String) 
        def messageLog = messageLogFactory.getMessageLog(message)
        if (messageLog != null) {
            messageLog.addAttachmentAsString('Support Case Event Payload', body, 'application/json')
        }
        return message
    }
    ```
    <br>![](../ex4/images/image-11.png)

> [!TIP]
    > Click on 'Apply' to save your changes. Notice that you may receive a warning.
    > You can ignore the warning and click on 'Close' and move ahead.
    > 
    > Make sure to save your changes in the main IFlow editor.

5. Next, after the script step, go ahead and add a new flow step.
<br>![](../ex4/images/image-12.png)

1. Look for the 'JSON to XML Converter' step. The default settings of this step are sufficient. No additional settings are needed in the property sheet.

    In the step, we are converting the JSON representation of the data event to its XML equivalent so that it can be easily extracted later.
<br>![](../ex4/images/image-13.png)

1. Next, after this Converter step, go ahead and add a new flow step.
<br>![](../ex4/images/image-14.png)

1. Select the 'Content Modifier' step in this dialog that pops out.
<br>![](../ex4/images/image-15.png)

1.  Name this step as 'Extract Support Case and Customer ID'. As you may have guessed, we will extract the Support Case & the Customer ID from the XML document's XPath expression.
   
    Go to the 'Exchange Property' tab of the property sheet of this step. Click on 'Add' thrice to add three properties.
<br>![](../ex4/images/image-16.png)

1. Copy the values from the table for Property settings.
   | Action | Name | Source Type | Source Value | Data Type |
    | ----- | ----- | ----- | ----- | ----- |
    | Create | supportCaseID | XPath | `root/data/currentImage/id` | java.lang.String
    | Create | customerID | XPath | `root/data/currentImage/extensions/SupportCaseByCustomer` | java.lang.String
    | Create | assignedParticipantID | Constant | IN162-`000` (replace `000` with your assiged participant ID)|
<br>![](../ex4/images/image-17.png)

## Step 4 - Filter out 'noisy' events and keep your Support Case data clean and accurate
Since we have subscribed to the Support Case Create event, an event will be emitted on the shared topic (sap/teched/2025/servicecloud/supportcase/created) each time a participant logs a support ticket. You may recall this step from [Exercise 1](../ex1/README.md#exercise-14---create-an-additional-queue-and-queue-subscription-in-advanced-event-mesh).

Although each participant has their own Queue subscription, the Topic itself is shared. As a result, your Queue will receive events for support cases created by all participants, which could lead to inaccurate or misleading summarizations. To address this, we will implement additional filtering steps to remove unnecessary 'noise' and ensure the data remains relevant and accurate.

We will now create two processing routes based on the customer ID retrieved from the support case. If the customer ID matches your individual participant ID, the event will be treated as valid and processed further. If it does not match, it will be considered as belonging to another participant and will be filtered out.
1. Add a new Flow step by clicking on the (+) button after the previous content modifier step.  Select the 'Router' step in the presented dialog.
<br>![](../ex4/images/image-18.png)

1. Title the step as `Route Customer ID` in the General tab of the Property Sheet. Click on the 'search step' text box on the right side of the screen and search for a 'content modifier' step. Click on the (+) next to the step to start adding it.
<br>![](../ex4/images/image-19.png)

1. Place the box below the 'Route Customer ID' step. Title this step as 'Set Custom Status'.
<br>![](../ex4/images/image-20.png)

1. Click-hold on the 'connector' button of the router step, drag the connector, and place it on the 'Set Custom Status' content modifier.
<br>![](../ex4/images/image-21.png)

1. After this second route is created, title it as 'Others'. Mark this as the 'Default Route' by checking the box. 
<br>![](../ex4/images/image-22.png)

1. Click on the first route (titled Route 1) and rename it to 'Assigned'. Click on the 'Processing' tab in the property sheet and set the 'Expression Type' to 'Non-XML' and the condition as `${property.customerID} = ${property.assignedParticipantID}`.
<br>![](../ex4/images/image-23.png)

1. Click on the 'Assigned' route and 'Add a Flow Step'.
<br>![](../ex4/images/image-24.png)

1. Select 'content modifier' in the 'Add Flow Step' dialog that pops out.
<br>![](../ex4/images/image-25.png)

1. Title the content modifier step as 'Set Application ID and Custom Status' in the General tab of the property sheet. Next, go to the 'message header' tab and 'Add' a header. 
<br>![](../ex4/images/image-26.png)

1. Set the Header properties as specified below:

    Action : `Create`, Name : `SAP_ApplicationID`, Source Type : `Property`, Source Value : `customerID`.
<br>![](../ex4/images/image-27.png)

1. Click on the 'Exchange Property' tab and add a Property as follows.
   
   Action: `Create`, Name: `SAP_MessageProcessingLogCustomStatus`, Source Type: `Constant`, Source Value: `Successful: Customer ID matched`.
<br>![](../ex4/images/image-28.png)

1. Let's turn our attention back to the 'Others' route now. Click on the 'Set Custom Status' content modifier step. Navigate to the 'Exchange Property' tab in the property sheet and 'Add' a Property. Set the attributes for the Property as follows:
    Action: `Create`, Name: `SAP_MessageProcessingLogCustomStatus`, Source Type: `Constant`, Source Value: `Terminated: Customer ID mismatch`.
<br>![](../ex4/images/image-70.png)

1. After this, click on the (+) button on the 'Set Custom Status' content modifier to add a new Flow step.
<br>![](../ex4/images/image-29.png)

1. Look up and select the 'Terminate Message' step in the 'add flow step' dialog.
<br>![](../ex4/images/image-30.png)

1. Title this step as 'Terminate'. This completes the logic for the 'others' route.
<br>![](../ex4/images/image-31.png)

## Step 5 - Perform a message mapping to cleanse the data 
Write text here.

1. Turning our attention now to the 'Assigned' route, click on (+) to add a flow step.
<br>![](../ex4/images/image-32.png)

1. Select the 'Request Reply' step in the Add flow step dialog.
<br>![](../ex4/images/image-33.png)

1. Notice that the layout has a 'Receiver' box at the right end of the canvas. Click on the box and drag it all the way down below the Request Reply step. Rename the shape as 'SSCV2'.
<br>![](../ex4/images/image-34.png)

1. Click on the 'Connector' arrow of the 'Get Support Case Details' request reply shape and start dragging it down.
<br>![](../ex4/images/image-35.png)

1. Drag it all the way down to connect with the SSCV2 receiver box. Release the cursor once the ends are joined. A dialog will pop out listing all the different Adapter types. 
<br>![](../ex4/images/image-36.png)

1. Select 'HTTP' from the Adapter list.
<br>![](../ex4/images/image-37.png)

1. Proceed to the 'Connection' section in the property sheet for the Adapter. Maintain the following attributes for the properties:
   | Field | Value |
    | ----- | ----- |
    | Address | `https://my1001903.de1.demo.crm.cloud.sap/sap/c4c/api/v1/case-service/cases/${property.supportCaseID}` |
    | Authentication | Basic|
    | Credential Name  | `sscv2_credentials` |

    <br>![](../ex4/images/image-38.png)

1. Click anywhere on the editor canvas (not on any flow step) to activate the 'Integration Flow' panel in the property sheet. Go to the 'References' tab, and in the Global subtab, click on the 'Add References' button and add a 'Message Mapping'.
<br>![](../ex4/images/image-39.png)

1. Here, we will specify the source package to import the pre-built mapping from. Bring up the 'Package' drop-down and select 'TechEd 2025 IN162 - Solution Package' as the source.
   
   Select the 'MM_SupportCase_ServiceCloud_to_HanaVectorDB' Artifact and click 'OK'.
<br>![](../ex4/images/image-40.png)

1. Head back to the 'Get Support Case Details' request reply step and click on the 'Add Flow Step' button.
<br>![](../ex4/images/image-41.png)

1.  Select 'Message Mapping' from the Add Flow Step dialog.
<br>![](../ex4/images/image-42.png)

1. Title the message mapping step as 'Message Mapping 1MM_SupportCase_ServiceCloud_to_HanaVectorDB' in the 'General tab of the property sheet. Navigate to the 'processing' tab from the property sheet for the message mapping step and click on the 'Select' button.
<br>![](../ex4/images/image-43.png)

1. This will load a dialog that lets you import existing mapping references in the IFlow. Navigate to the 'Global Resources' section. This displays the mapping we imported in the previous step. Select the message mapping resource and click OK.
 
    <br>![](../ex4/images/image-44.png)

1. Click on the resource to view the mapping. This will load the mapping in a new window.
<br>![](../ex4/images/image-45.png)

## Step 6 - Prepare data payload to invoke the embedding model of the AI Service 
Write text here

1. Click on (+) button to add a new flow step.
<br>![](../ex4/images/image-46.png)

1. Select 'groovy script' in the Add Flow Step dialog.
<br>![](../ex4/images/image-47.png)

1. Title the Groovy script step as 'Log Support Case JSON Payload'. Click on the 'Create' button on the step.
<br>![](../ex4/images/image-48.png)

1. Copy the code below and paste it into the code editor window.
      ```groovy
    import com.sap.gateway.ip.core.customdev.util.Message
    import groovy.json.JsonOutput

    def Message processData(Message message) {
        def body = message.getBody(java.lang.String);
        def messageLog = messageLogFactory.getMessageLog(message);
        if (messageLog != null) {
            messageLog.addAttachmentAsString('Support case JSON Payload', body, 'application/json');
        }
        // save support case json as a property to be used later
        message.setProperty("supportCaseJson", body);
        
        // Escape double quotes in support case json and update the body
        def escapedSupportCaseJson = JsonOutput.toJson(body); 
        message.setBody(escapedSupportCaseJson);
        
        return message;
    }
    ```

    Click on 'Apply', ignore any warnings if presented, and 'Close' the editor
<br>![](../ex4/images/image-49.png)

1.  After this step, click on the (+) button to add a new flow step.
<br>![](../ex4/images/image-50.png)

1. Select the 'content modifier' step in the 'add flow step' dialog.
<br>![](../ex4/images/image-51.png)

1. Title this as 'Prepare Embedding Call'. Go to the 'Message Header' tab and click 'Add' twice to prepare to add two header attributes.
    Manage the attribute entries as follows:
   | Action | Name | Source Type | Source Value |
    | ----- | ----- | ----- |----- |
    | Create | content-type | Constant | application/json |
    | Create |  ai-resource-group | Property | assignedParticipantID |
    
    <br>![](../ex4/images/image-52.png)

1. Click on the 'Message Body' tab and enter the following text as an 'Expression'.
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
    <br>![](../ex4/images/image-53.png)

1. Click on the (+) button after the previous step to add a new flow step. Select 'Request-Reply step in the 'Add Flow step' dialog.
<br>![](../ex4/images/image-54.png)

1. Title this step as 'Get Text Embeddings'. Click on the 'search step' text box on the right and search for the 'Receiver' shape.
<br>![](../ex4/images/image-55.png)

1. Click and drag the receiver shape right below the request-reply step. You can call this box 'AI_Launchpad'. Click on the 'connector' button.
<br>![](../ex4/images/image-56.png)

1. Start dragging the connector button all the way down to join the 'AI_Launchpad' Receiver box. Release the mouse button once the ends are joined. An 'Adapter Type' dialog will pop out.
<br>![](../ex4/images/image-57.png)

1. Select 'HTTP' for the 'Adapter Type'.
<br>![](../ex4/images/image-58.png)

1. Proceed to the 'Connection' section in the property sheet for the Adapter. Maintain the following attributes for the properties:
   | Field | Value |
    | ----- | ----- |
    | Address | `https://api.ai.prod.eu-central-1.aws.ml.hana.ondemand.com/v2/inference/deployments/db1ce5ede0291ce0` |
    | Method | POST |
    | Authentication | OAuth2Client Credentials|
    | Credential Name  | `aicore_credentials` |
    | Request Headers | * |

    <br>![](../ex4/images/image-59.png)

## Step 7 - Prepare data payload to persist text embeddings into HANA Vector DB 
enter text here...

1. After the 'Get Text Embeddings' step, click on the (+) button to 'Add a Flow Step'.
<br>![](../ex4/images/image-60.png)

1. Select 'Groovy Script' from the 'Add Flow step' dialog.
<br>![](../ex4/images/image-61.png)

1. Title the step as 'Prepare SQL Statement'. Click on the 'Create' button on the step.
<br>![](../ex4/images/image-62.png)

1.  Copy the following Groovy script and paste it into the script editor.
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
        def escapedSupportCaseJson = properties.get("supportCaseJson");
        
        // Build SQL (HANA requires embeddings inline, not as ?)
        def sqlInsertStatement = "INSERT INTO \"DBADMIN\".\"TechEd25_IN162_Table\" (CUSTOMER_ID, CUSTOMER_NAME, TEXT, TEXT_VECTOR) VALUES (\'${customerID}\',\'${customerName}\',\'${escapedSupportCaseJson}\',TO_REAL_VECTOR(\'${embeddings}\'))";

        message.setBody(sqlInsertStatement);
        return message;
    }
    ```
    Click on 'Apply' and after the changes are applied, click  'Close' to exit the script editor. 
<br>![](../ex4/images/image-63.png)

1. After this step, click on (+) to add a new flow step.
<br>![](../ex4/images/image-64.png)

1. Add a 'Request-Reply' step in the 'Add Flow step' dialog.
<br>![](../ex4/images/image-65.png)

1. Title the step as 'Insert Embeddings to Vector DB'. 
   
   Click on the 'Search Step' text box and look for a 'Receiver' step.
<br>![](../ex4/images/image-66.png)

1. Drag and place the receiver box below the request-reply step. Title the box as 'HANA_DB'. Click on the 'Connector' button, drag an arrow connecting it to the 'HANA_DB' receiver box. A dialog to select the Adapter pops out.
<br>![](../ex4/images/image-67.png)

1. Select 'JDBC' for the Adapter type.
<br>![](../ex4/images/image-68.png)

1. Proceed to the 'Connection' tab of the property sheet and enter `SAPHANACloud` in the 'JDBC Data Source Alias' box. Note that this data source alias has already been built for you.
<br>![](../ex4/images/image-69.png)
