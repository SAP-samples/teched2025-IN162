# Scenario Introduction

This session scenario helps you to learn and apply real-time vector grounding by connecting structured data to an LLM using a **SAP Hana Cloud vector database, SAP Integration Suite and SAP Integration Suite, advanced event mesh**. You’ll see how updates to the data can instantly influence the model’s responses, keeping them current and contextually accurate. By the end, you’ll build an end-to-end, event-driven flow that reacts to **new sales order events from SAP S/4HANA Cloud and new support case events from SAP Service Cloud Version 2**, providing the context to AI assistant in real time.

## Business Scenario
Assume your organization uses an **AI-powered Customer Success Digital Assistant**. The assistant helps customer success managers prepare for customer meetings by **highlighting and summarizing the latest sales orders and support tickets**, and by suggesting key talking points. To ensure accurate, up-to-date context, the assistant needs real-time access to sales order and support ticket data. And for this we will leverage event-driven integration pattern to deliver real-time grounding data, equipping AI system's with the latest business context to make informed and accurate decisions.

## Scenario Architecture

<img src="/intro/intro1/images/IN162_scenario_architecture.png" width=100% height=100%>

1. The events are triggered/published from source SAP Cloud Solutions.
    <br><br> 1a. Create a new sales order in **SAP S/4HANA Cloud** system. The new sales order event gets **published** using the **REST** interface directly to **SAP Integration Suite, advanced event mesh (AEM)** topic `sap/teched/2025/sap/s4/beh/salesorder/v1/SalesOrder/Created/v1`. For this, we need to do the proper [configurations](/intro/intro2) and this has already been done on the given **SAP S/4HANA Cloud** system.
    <br><br> 1b. Create a new support case in **SAP Service Cloud Version 2** system. The new support case event gets **published** using the **REST** interface directly to **SAP Integration Suite, advanced event mesh (AEM)** topic `sap/teched/2025/servicecloud/supportcase/created`. For this, we need to do the proper [configurations](/intro/intro3) and this has already been done on the given **SAP Service Cloud Version 2** system.

   > **Note:** The same can be accomplished with non-SAP applications as well—for example, **ServiceNow**.

2. Within the **Cloud Integration capability of SAP Integration Suite**, two Integration Flows subscribe to new Sales Order and Support Case events via the Cloud Integration **Advanced Event Mesh sender adapter**, which uses the **Solace Message Format (SMF) protocol**.
   
3. Both Integration Flows then **perform lookup calls** to the downstream systems — **SAP S/4HANA Cloud** and **SAP Service Cloud Version 2** to retrieve additional details for the respective sales order and support case. They then perform data transformation using message mapping and ultimately convert the payload to JSON format.

4. The Integration Flows then **invoke the SAP AI Core API to generate embeddings for the JSON payload** using the `text-embedding-3-small` model.

5. Finally, the **generated embeddings are inserted into the SAP HANA Cloud vector database**, ensuring real-time grounding of Sales Order and Support Case objects. In this session, we’ll create these two integration flows to demonstrate the complete real-time grounding process.
    
## Summary

You should now be familiar with the session scenario.

Now, to learn all the configurations that has been done in <b>SAP SuccessFactors</b> to enable new hire event publication to <b>SAP Integration Suite, advanced event mesh</b>, you can navigate to [SAP S/4HANA Cloud Configuration](/intro/intro2/README.md) section.
