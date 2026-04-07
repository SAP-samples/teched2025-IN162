# Exercise 4 - Create an Integration Flow for Service Cloud Support Case event to embedding model to SAP HANA Vector DB for AI Grounding in SAP Integration Suite
In this exercise, we will build an integration flow to receive the data event triggered when a Support Case is created in SAP Service Cloud Version 2 using an AEM Adapter. Unlike the scenario in Exercise 3, where SAP S/4HANA emitted only a notification event, SAP Service Cloud includes the complete data payload as part of its event framework. Although the standard Support Case business event is the data event but still the integration flow will call the Support Case API to fetch the additional details. These relevant details from the Support Case event will be transformed into text embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs, as we accomplished in the [previous](../ex2/README.md) exercise. 

The resulting embeddings will be stored in a connected SAP HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

We assume that you have already completed Exercise 3 and, hence, you are logged into the SAP Integration Suite tenant and have created an Integration Package titled '**TechEd 2025 IN162-0`**`**', replace `**` with your assigned user identifier. If not, navigate to [this](../ex3/README.md#step-1---log-into-your-designated-integration-suite-tenant) section to log on and create a **Package** and come back to this section.

Now [copy an existing second integration flow - Exercise 4.1](./ex4_1_details.md) 
