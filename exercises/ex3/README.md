# Exercise 3 - Integration Suite – Iflow S4Hana Sales Order to embedding model to SAP Hana Vector DB

In this exercise, we will configure an IFlow to receive a notification event upon the creation of a Sales Order in S/4HANA. Using the Sales Order ID, we will then retrieve the complete Sales Order details from the S/4HANA system. These details will be transformed into vector embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs. The resulting embeddings will be stored in a connected HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

## Exercise 3.1 Create a package and an IFlow in your designated tenant

1. Log into your assigned Integration Suite tenant and create a new Package from the 'Integration and APIs' sub-menu within the 'Design' menu.<br>
    Name your package as follows : 'IN162_`XX`_Solution_Package' (replace `XX` with your assgined user identifier)

<br>![](/exercises/ex3/images/image1.png)

2. After the package is created, go to the 'Artifacts' tab, 'Edit' and create a new 'Integration Flow'.
<br>![](/exercises/ex3/images/image2.png)

3. In the create dialog, name your integration flow as : 'IN162_`XX` Sales Order Event to Hana Vector DB for AI Grounding' (replace `XX` with your assgined user identifier). 
   Open the IFLow in the editor.
<br>![](/exercises/ex3/images/image3.png)