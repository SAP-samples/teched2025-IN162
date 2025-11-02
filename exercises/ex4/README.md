# Exercise 4 - Integration Suite – IFlow Support Case Event to SAP HANA Vector DB for AI Grounding

In this exercise, we will build an IFlow to receive the notification event triggered when a Support Case is created in SAP Service Cloud Version 2 using an AEM Adapter. Unlike the scenario in Exercise 3, where SAP S/4HANA emitted only a notification event, SAP Service Cloud includes the complete data payload as part of its event framework. These relevant details from the Support Case event will be transformed into text embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs, as we accomplished in the [previous](../ex2/README.md) exercise. 

The resulting embeddings will be stored in a connected SAP HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

For your convenience, this exercise is provided in two formats, following the same approach as Exercise 3:

- Recommended Approach ([Exercise 4.2](./ex4_2_details.md)): Copy a fully prepared IFlow, applying only minimal configuration before deployment. This option is ideal for participants who have limited time or prefer to work with pre-built content.
- Alternate Approach ([Exercise 4.1](./ex4_1_details.md)): Build the IFlow from scratch by following step-by-step instructions. This approach offers a more immersive hands-on experience and a clearer understanding of the flow design.

> [!TIP] 
> Even if you follow the recommended approach, it is strongly advised to review Exercise 4.1 in detail to gain a comprehensive understanding of its functionality.

We will assume that you have already completed Exercise 3 and, hence, you are logged into the the assigned Integration Suite tenant and have created an Integration Package titled 'TechEd 2025 IN162-`000`' (replace `000` with your assigned user identifier). If not, navigate to [this](../ex3/README.md#step-1---log-into-your-designated-integration-suite-tenant) section to log in to the right tenant, create a Package and come back to this section.

 From this point on, follow either [Exercise 4.2](./ex4_2_details.md)(recommended - easy and quick) or [Exercise 4.1](./ex4_1_details.md)(expert grade - thorough and long).
