# Scenario Introduction

This session immerses you in real-time vector grounding — connecting structured enterprise data to a large language model through **SAP HANA Cloud’s vector database, SAP Integration Suite, and Advanced Event Mesh.** You’ll experience how dynamic data updates can instantly refine the model’s responses, ensuring they remain current and contextually precise. By the end, you’ll have built an end-to-end, event-driven flow that reacts to **new sales order events from SAP S/4HANA Cloud** and **support case events from SAP Service Cloud Version 2**, providing the AI assistant with live contextual intelligence.

## Business Scenario

Imagine your organization employs an **AI-powered Customer Success Digital Assistant** that helps customer success managers prepare for meetings by **summarizing the latest sales orders and support tickets**, and by suggesting relevant talking points. To provide accurate, real-time insights, the assistant requires continuous access to up-to-date sales and support data. In this session, we’ll leverage an event-driven integration pattern to deliver real-time grounding data — ensuring the AI system stays contextually aware and equipped with the latest business information to drive informed, accurate decisions.

## Scenario Architecture

<img src="/intro/intro1/images/IN162_scenario_architecture.png" width=100% height=100%>

### Real-time grounding flow

1. The events are triggered/published from source SAP Cloud Solutions.
    - Create a new sales order in **SAP S/4HANA Cloud** system. The new sales order event gets **published** using the **REST** interface directly to **SAP Integration Suite, advanced event mesh (AEM)** topic `sap/teched/2025/sap/s4/beh/salesorder/v1/SalesOrder/Created/v1`. For this, we need to do the proper [configurations](/intro/intro2) and this has already been done on the given **SAP S/4HANA Cloud** system.
    - Create a new support case in **SAP Service Cloud Version 2** system. The new support case event gets **published** using the **REST** interface directly to **SAP Integration Suite, advanced event mesh (AEM)** topic `sap/teched/2025/servicecloud/supportcase/created`. For this, we need to do the proper [configurations](/intro/intro3) and this has already been done on the given **SAP Service Cloud Version 2** system.

> [!Note] 
> The same can be accomplished with non-SAP applications as well—for example, **ServiceNow**.

2. Within the **Cloud Integration capability of SAP Integration Suite**, two Integration Flows subscribe to new Sales Order and Support Case events via the Cloud Integration **Advanced Event Mesh sender adapter**, which uses the **Solace Message Format (SMF) protocol**.
   
3. Both Integration Flows then **perform lookup calls** to the downstream systems — **SAP S/4HANA Cloud** and **SAP Service Cloud Version 2** to retrieve additional details for the respective sales order and support case. They then perform data transformation using message mapping and ultimately convert the payload to JSON format.

4. The Integration Flows then **invoke the SAP AI Core API to generate embeddings for the JSON payload** using the `text-embedding-3-small` model.

5. Finally, the **generated embeddings are inserted into the SAP HANA Cloud vector database** using JDBC receiver adapter, ensuring real-time grounding of Sales Order and Support Case objects. In this session, we’ll create these two integration flows to demonstrate the complete real-time grounding process.

### Customer Success Digital Assistant flow

As the integration flows have been set up to ensure that newly created Sales Orders and Support Tickets are grounded in real-time using event-driven integration pattern, let’s see how this grounded data can be leveraged by the Joule-powered digital assistant, whose role is to help Customer Success Managers prepare for customer meetings by highlighting and summarizing the latest Sales Orders and Support Tickets, and by suggesting key talking points.

6. The **Customer Success Manager (CSM) enters a prompt** into the Joule-powered digital assistant, seeking assistance in preparing for an upcoming meeting with one of their customers.

7. The Joule-powered digital assistant is **created using Joule Skill** that calls an SAP Build Process Automation action, which in turn **triggers an integration flow within Cloud Integration capability of SAP Integration Suite**.
  
8. The integration flow then **invoke the SAP AI Core API to generate an embedding of the prompt** using the same `text-embedding-3-small` model.

9. Using that embedding, the integration flow **queries the SAP HANA Cloud vector database for top cosine-similarity** matches among the customer’s sales orders and support tickets.

10. The integration flow ultimately invokes the `gpt-4.1` **LLM via the SAP AI Core API using the AI Adapter**. The retrieved contextual data is provided to the model to summarize recent sales orders and support tickets and generate relevant meeting talking points, which are then returned through an SAP Build Action to Joule and presented to the CSM.

With this setup, the CSM’s prompt is turned into a vector, matched against up-to-date structured data in SAP HANA Cloud, and then summarized with an LLM to produce relevant, timely talking points. The result is a practical, secure, and real-time RAG workflow integrated across Joule, SAP Build Actions, SAP Integration Suite, SAP AI Core, and SAP HANA Cloud’s vector engine.

## Summary

You should now be familiar with the session scenario.

Now, to learn all the configurations that has been done in **SAP S/4HANA Cloud** to enable new sales order event publication to **SAP Integration Suite, advanced event mesh**, you can navigate to [SAP S/4HANA Cloud Configuration](/intro/intro2/README.md) section.
