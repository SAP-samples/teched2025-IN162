# Exercise 4 - Integration Suite – IFlow Support Case Event to Hana Vector DB for AI Grounding

In this exercise, we will put together an IFlow to receive the notification event that is emitted upto the creation of a Support Case in SAP Service Cloud with an AEM Adapter. Unlike the case we dealt with in Exercise 3, SAP Service Cloud will emit the entire data as part of the event framework, as opposed to S/4HANA that only emitted a notification event. These relevant details from the Support Case event will be transformed into vector embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs. The resulting embeddings will be stored in a connected HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

For your convenience, this exercise is provided in two formats, following the same approach as Exercise 3:

- Recommended Approach ([Exercise 4.1](./ex4_1_details.md)): Build the IFlow from scratch by following step-by-step instructions. This method provides a deeper hands-on experience and a better understanding of the flow design.
- Alternate Approach ([Exercise 4.2](./ex4_2_details.md)): Skip the build steps and instead copy a fully prepared IFlow, applying only minimal configuration before deployment. This option is ideal for participants who have limited time or prefer to work with pre-built content.

> [!TIP] 
> If time is limited and you’ve already worked through Exercise 3.1 from scratch, you can consider taking the alternate approach. 

We will assume that you have already completed Exercise 3 and hence, have already created an Integration Package titled 'TechEd 2025 IN162-`000`' (replace `000` with your assigned user identifier). If not, navigate to [this](../ex3/README.md#step-1---create-a-package-in-your-designated-tenant) section, create a Package to get started.

 From this point on, follow either [Exercise 4.1](./ex4_1_details.md) or [Exercise 4.2](./ex4_2_details.md)

