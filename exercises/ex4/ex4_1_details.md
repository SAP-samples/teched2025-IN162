# Exercise 4.1 - Create an IFlow from scratch to receive a Support Case creation event, transform into embeddings, and persist to HANA Vector DB

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
    | Message VPN | techend-2025-europe |
    | Username | solace-cloud-client|
    | Authentication Type | Basic |
    | Password Secure Alias | teched-2025-europe-aem-password |

    <br>![](../ex4/images/image-6.png)

1.  Next, head over to the 'Processing' tab and enter 'IN162-`000`_Support_Case' in the Queue Name field (replace `000` with your assigned user identifier). 
   
    Leave all other attributes with their default values.
<br>![](../ex4/images/image-7.png)

    We are done with the first block. 
>[!TIP]
>Click 'Save' periodically over the course of this exercise so that you don't lose your work if the browser session were to time out.

## Step 3 - Massage the data event received from the Adapter
In the next few steps of this section, we will tailor the data we receive from the Adapter for further processing.
1. Click on the (+) Add Flow Step button to add a new step.
<br>![](../ex4/images/image-8.png)

1. Select a 'Groovy Script' in the Add Flow Step dialog.
<br>![](../ex4/images/image-9.png)

1. Let's title this step as 'Log Support Case Event Payload'. As the name suggests, we will log the payload we receive from the AEM Adapter
<br>![](../ex4/images/image-10.png)

1. Copy the following lines of code and paste them into the script editor window.
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

5. Next, after the script step, go ahead and add a new flow step.
<br>![](../ex4/images/image-12.png)

1. Look for the 'JSON to XML Converter' step. The default settings of this step are sufficient. No changes are needed.
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
    | Create | assignedParticipantID | Constant | IN162-`000` (replace `000`)|
<br>![](../ex4/images/image-17.png)

## Step 4 - Eliminate noise from unintended events 
Since we have subscribed to the Support Case Create event, an event will be emitted on the shared topic whenever a participant logs a support case. Naturally, this could result in each participant receiving more support cases than expected, leading to inaccurate or misleading summarizations. To prevent this, we will introduce a  filtering step based on the customerID to eliminate unnecessary noise and ensure the data remains relevant and accurate.
1. Add a new Flow step by clicking on the (+) button after the previous content modifer step.  Select the 'Router' step in the presented dialog.
<br>![](../ex4/images/image-18.png)

1. Title the step as `Route Customer ID` in the General tab of the Property Sheet. Click on the 'search step' text box in th right side of the screen and search for a 'content modifier' step. Click on the (+) next to the step to start adding it.
<br>![](../ex4/images/image-19.png)

1. Place the box below the 'Route Customer ID' step. Title this step as 'Set Custom Status'.
<br>![](../ex4/images/image-20.png)

1. Click-hold on the 'connector' button of the router step, drag the connector and place it on the 'Set Custom Status' content modifier.
<br>![](../ex4/images/image-21.png)

1. After this second route is created, title it as 'Others'. Mark this as the 'Default Route' by checking the box. 
<br>![](../ex4/images/image-22.png)

1. Click on the first route (titled Route 1) and rename it to 'Assigned'. Click on the 'Processing' tab in the propety shet and set the 'Expression Type' to 'Non-XML' and the condition as `${property.customerID} = ${property.assignedParticipantID}`.
<br>![](../ex4/images/image-23.png)

1. Click on the 'Assigned' route and 'Add a Flow Step'.
<br>![](../ex4/images/image-24.png)

1. Select 'content modifier' in the 'Add Flow Step' dialog that pops out.
<br>![](../ex4/images/image-25.png)

1. Title the content modifier step as 'Set Application ID and Custom Status' in the General tab of the property sheet. Next, go to the 'message header' tab and 'Add' a header. 
<br>![](../ex4/images/image-26.png)

1. Set the Header properties as specifed below:

    Action : `Create`, Name : `SAP_ApplicationID`, Source Type : `Property`, Source Value : `customerID`.
<br>![](../ex4/images/image-27.png)

1. Click on the 'Exchange Property' tab and add a Property as follows.
   
   Action: `Create`, Name: `SAP_MessageProcessingLogCustomStatus`, Source Type: `Constant`, Source Value: `Successful: Customer ID matched`.
<br>![](../ex4/images/image-28.png)

1. Let's turn our attention back to the 'Others' route now. Click on the 'Set Custom Status' content modifier step. Navigate to the 'Exchange Property' tab in the property sheet and 'Add' a Property. Set the attributes for the Property as follows:
    Action: `Create`, Name: `SAP_MessageProcessingLogCustomStatus`, Source Type: `Constant`, Source Value: `Terminated: Customer ID mismatch`.
<br>![](../ex4/images/image-70.png)

1. After this, Click on the (+) button on the 'Set Custom Status' content modifier to add a new Flow step.
<br>![](../ex4/images/image-29.png)

1. Look up and select the 'Terminate Message' step in the 'add flow step' dialog.
<br>![](../ex4/images/image-30.png)

1. Title this step as 'Terminate'. This completes the logic for the 'others' route.
<br>![](../ex4/images/image-31.png)

1. Text.
<br>![](../ex4/images/image-32.png)

1. Text.
<br>![](../ex4/images/image-33.png)

1. Text.
<br>![](../ex4/images/image-34.png)

1. Text.
<br>![](../ex4/images/image-35.png)

1. Text.
<br>![](../ex4/images/image-36.png)

1. Text.
<br>![](../ex4/images/image-37.png)

1. Text.
<br>![](../ex4/images/image-38.png)

1. Text.
<br>![](../ex4/images/image-39.png)

1. Text.
<br>![](../ex4/images/image-40.png)

1. Text.
<br>![](../ex4/images/image-41.png)

1. Text.
<br>![](../ex4/images/image-42.png)

1. Text.
<br>![](../ex4/images/image-43.png)

1. Text.
<br>![](../ex4/images/image-44.png)

1. Text.
<br>![](../ex4/images/image-45.png)

1. Text.
<br>![](../ex4/images/image-46.png)

1. Text.
<br>![](../ex4/images/image-47.png)

1. Text.
<br>![](../ex4/images/image-48.png)

1. Text.
<br>![](../ex4/images/image-49.png)

1. Text.
<br>![](../ex4/images/image-50.png)

1. Text.
<br>![](../ex4/images/image-51.png)

1. Text.
<br>![](../ex4/images/image-52.png)

1. Text.
<br>![](../ex4/images/image-53.png)

1. Text.
<br>![](../ex4/images/image-54.png)

1. Text.
<br>![](../ex4/images/image-55.png)

1. Text.
<br>![](../ex4/images/image-56.png)

1. Text.
<br>![](../ex4/images/image-57.png)

1. Text.
<br>![](../ex4/images/image-58.png)

1. Text.
<br>![](../ex4/images/image-59.png)

1. Text.
<br>![](../ex4/images/image-60.png)

1. Text.
<br>![](../ex4/images/image-61.png)

1. Text.
<br>![](../ex4/images/image-62.png)

1. Text.
<br>![](../ex4/images/image-63.png)

1. Text.
<br>![](../ex4/images/image-64.png)

1. Text.
<br>![](../ex4/images/image-65.png)

1. Text.
<br>![](../ex4/images/image-66.png)

1. Text.
<br>![](../ex4/images/image-67.png)

1. Text.
<br>![](../ex4/images/image-68.png)

1. Text.
<br>![](../ex4/images/image-69.png)