# Exercise 3 - Integration Suite – Iflow S4Hana Sales Order to embedding model to SAP Hana Vector DB

In this exercise, we will begin by creating a Sales Order in the S/4HANA system. The system is configured to emit an event containing the newly created Sales Order ID. This event is captured by Advanced Event Mesh (AEM), which publishes it to the designated topic, making it immediately available to all subscribed consumers.

[Exercise 3.1 - Create a Sales Order in S/4HANA system](./ex3_1_details.md)

We will then configure an IFlow to receive the notification event upon the creation of a Sales Order in S/4HANA. Using the Sales Order ID, we will then retrieve the complete Sales Order details from the S/4HANA system. These details will be transformed into vector embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs. The resulting embeddings will be stored in a connected HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

[Exercise 3.2 - Create an IFlow to receive the Sales Order, create embeddings and store in HANA Vector DB](./ex3_1_details.md)
