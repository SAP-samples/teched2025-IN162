# Exercise 4 - Integration Suite – Iflow Support Case Event to Hana Vector DB for AI Grounding

In this exercise, we will configure an IFlow to receive the notification event that is emitted upto the creation of a Sales Order in S/4HANA with an AEM Adapter. Using the Sales Order ID, we will then retrieve the complete Sales Order details from the S/4HANA system. These details will be transformed into vector embeddings through the `text-embedding-3-small` model via SAP Generative Hub’s REST APIs. The resulting embeddings will be stored in a connected HANA Vector Database, enabling efficient retrieval and text summarization when queried through the Joule assistant.

## Exercise 4.1 - Create a package and an IFlow in your designated tenant

1. Text.
<br>![](../ex4/images/image-1.png)

1. Text.
<br>![](../ex4/images/image-2.png)

1. Text.
<br>![](../ex4/images/image-3.png)

1. Text.
<br>![](../ex4/images/image-4.png)

1. Text.
<br>![](../ex4/images/image-5.png)

1. Text.
<br>![](../ex4/images/image-6.png)

1. Text.
<br>![](../ex4/images/image-7.png)

1. Text.
<br>![](../ex4/images/image-8.png)

1. Text.
<br>![](../ex4/images/image-9.png)

1. Text.
<br>![](../ex4/images/image-10.png)

1. Text.
<br>![](../ex4/images/image-11.png)

1. Text.
<br>![](../ex4/images/image-12.png)

1. Text.
<br>![](../ex4/images/image-13.png)

1. Text.
<br>![](../ex4/images/image-14.png)

1. Text.
<br>![](../ex4/images/image-15.png)

1. Text.
<br>![](../ex4/images/image-16.png)

1. Text.
<br>![](../ex4/images/image-17.png)

1. Text.
<br>![](../ex4/images/image-18.png)

1. Text.
<br>![](../ex4/images/image-19.png)

1. Text.
<br>![](../ex4/images/image-20.png)

1. Text.
<br>![](../ex4/images/image-21.png)

1. Text.
<br>![](../ex4/images/image-22.png)

1. Text.
<br>![](../ex4/images/image-23.png)

1. Text.
<br>![](../ex4/images/image-24.png)

1. Text.
<br>![](../ex4/images/image-25.png)

1. Text.
<br>![](../ex4/images/image-26.png)

1. Text.
<br>![](../ex4/images/image-27.png)

1. Text.
<br>![](../ex4/images/image-28.png)

1. Text.
<br>![](../ex4/images/image-29.png)

1. Text.
<br>![](../ex4/images/image-30.png)

1. Text.
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