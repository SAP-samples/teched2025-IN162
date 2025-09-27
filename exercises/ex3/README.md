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
In the next few steps f this section, we will tailor the data we receive for further processing.

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image10.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image11.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image12.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image13.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image14.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image15.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image16.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image17.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image18.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image19.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image1.png)

1. Log into your assigned Integration Suite tenant and 'create' a new Package from the 'Integration and APIs' sub-menu under the 'Design' menu.
<br>![](../ex3/images/image1.png)