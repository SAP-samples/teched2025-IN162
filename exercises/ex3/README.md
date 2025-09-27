# Exercise 3 - Integration Suite – Iflow S4Hana Sales Order to embedding model to SAP Hana Vector DB

In this exercise, we will configure an IFlow to receive the notification event that is emitted upto the creation of a Sales Order in S/4HANA with an AEM Adapter. Using the Sales Order ID, we will then retrieve the complete Sales Order details from the S/4HANA system. These details will be transformed into vector embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs. The resulting embeddings will be stored in a connected HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

## Exercise 3.1 - Create a package and an IFlow in your designated tenant

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image1.png)

2. Enter the following details to describe the package.
    | Field | Value |
    | ----- | ----- |
    | Name | TechEd 2025 IN162-`000` (replace `000` with your assigned user identifier) |
    | Short Description | Learn how to build an event-driven integration scenario using SAP Integration Suite and advanced event mesh to deliver real-time grounding data to SAP Hana Cloud Vector DB, empowering AI agents with accurate, context-aware insights and responses. |
    | Version | 1.0.0|
    | Vendor | SAP |

    <br>![](../ex3/images/image2.png)
After the Package is successfully created, go to the 'Artifacts' tab.
3. In the Artifacts tab, click on 'Add' -> 'Integration Flow'
   <br>![](../ex3/images/image3.png)
4. In the create dialog, name your integration flow as : 'IN162-`000` Sales Order Event to Hana Vector DB for AI Grounding' (replace `000` with your assgined user identifier). 

    Click on Add and Open in Editor.
<br>![](../ex3/images/image4.png)

5. Now you should see the basic skeleton of the IFlow ready for us to starting builing upon. 

    Click on 'Edit' to get started.
   <br>![](../ex3/images/image5.png)

## Exercise 3.2 - AEM Sender Adapter to receive events from S/4HANA
In this step, we will setup the configure the AEM Adapter to receive events from S/4HANA.

1. Connect the'Sender' system to the 'Start' block by holding and dragging your mouse pointer. 
<br>![](../ex3/images/image6.png)

1. A dialog with the different adapters will appear. Select the 'AdvancedEventMesh' adapter.
<br>![](../ex3/images/image7.png)

1. Select this newly aded Adapter and click on the 'Connection' tab from the properties sheet at the bottom of the screen. Add the following attributes to the 'Sender Connection Details' section.
    | Field | Value |
    | ----- | ----- |
    | Host | tcps://mr-connection-sq0b51wu6s3.messaging.solace.cloud:55443 |
    | Message VPN | techend-2025-europe |
    | Username | solace-cloud-client|
    | Authentication Type | Basic |
    | Password Secure Alias | teched-2025-europe-aem-password |
   
    <br>![](../ex3/images/image8.png)

2. Next, head over to the 'Processing' tab and enter IN162-`000`_Sales_Order in the Queue Name field (replace `000` with your assigned user identifier). 
   
   Leave all other attributes with their default values.
<br>![](../ex3/images/image9.png)

    We are done with the first block. Click 'Save' so that you don't lose your work if the session times out. 

## Exercise 3.3 - Massage the notification event event received from the Adapter
In the next few steps of this section, we will tailor the data we receive for further processing.

1. Click on the (+) Add Flow Step button to add a new step.
<br>![](../ex3/images/image10.png)

1. Select a 'Groovy Script' in the Add Flow Step dialog.
<br>![](../ex3/images/image11.png)

1. Let's title this step as 'Log Sales Order Event Payload'. As the name suggests, we will log the payload we recieve from the AEM Adapter
<br>![](../ex3/images/image12.png)

1. Copy the following lines of code and paste it into the script editor window.
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
    <br>![](../ex3/images/image13.png)
    > [!TIP]
    > Click on 'Apply' to save your changes. Note that you may receive a warning.
    > you can ignore the warning and click on 'Close' and move ahead.
    

2. Next, after the script step, go ahead and add a new flow step.
<br>![](../ex3/images/image14.png)

1. Look for the 'JSON to XML Converter' step. The default settings of this step are sufficient. No changes are needed.
<br>![](../ex3/images/image15.png)

1. Next, after this Converter step, go ahead and add a new flow step.
<br>![](../ex3/images/image16.png)

1. Select the 'Content Modifier' step in this dialog that pops out.
<br>![](../ex3/images/image17.png)

1. Name this step as 'Extract Sales Orer ID'. As you may have guessed, we will extract the Sales Order ID from the XML document's XPath expression.
   
    Go to the 'Exchange Property' tab of the property sheet of this step. Click on 'Add' twice to add two properties.
<br>![](../ex3/images/image18.png)

1. Copy the values from the table for Property settings.
   | Action | Name | Source Type | Source Value | Data Type |
    | ----- | ----- | ----- | ----- | ----- |
    | Create | salesOrderID | XPath | root/data/SalesOrder | java.lang.String
    | Create | assignedParticipantID | Constant | IN162-`000` (replace `000`)|
    

    <br>![](../ex3/images/image19.png)

## Exercise 3.4 - Call the S/4HANA system to get the full data object
As you may have observed, the event triggered upon sales order creation provides only the Sales Order ID — essentially serving as a notification event. To retrieve the complete details, we must use this ID to query the S/4HANA system and obtain the full set of sales order attributes.

1. Add a new Flow Step after the 'Extract Sales Order ID' content modifier step.
<br>![](../ex3/images/image20.png)

1. Select 'Request-Reply' step from the 'Add Flow Step' dialog.
<br>![](../ex3/images/image21.png)

1. Name the flow step as 'Get Sales Order Details'. Next, hold and drag the 'Receiver' shape from the right corner near the 'request-reply' shape.
<br>![](../ex3/images/image22.png)

1. Select the 'Connector' button. 
<br>![](../ex3/images/image23.png)

1. CLick and hold on the 'Connector' button and and drag it all the way down onto the 'Receiver' shape.
<br>![](../ex3/images/image24.png)

1. A dialog pops with a list of Adapters to choose from. Select the 'OData' Adapter.
<br>![](../ex3/images/image25.png)

1. Select OData version V2.
<br>![](../ex3/images/image26.png)

1. Click on the 'Connection' tab of the property sheet of the Adapter. Enter the following values in the connection details section.
<br>![](../ex3/images/image27.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image28.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image29.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image30.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image31.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image32.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image33.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image34.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image35.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image36.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image37.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image38.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image39.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image40.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image41.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image42.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image43.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image44.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image45.png)

1. Next, proceed to the 'Processing' section.
<br>![](../ex3/images/image46.png)