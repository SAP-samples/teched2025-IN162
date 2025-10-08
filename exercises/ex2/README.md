# Exercise 2 - Expose Embedding and Summarization Models as an API Using Gen AI Hub (AI Core)

In this exercise, you will create configuration and orchestration deployments in SAP Generative AI Hub with AI Core. The Orchestration Deployment URL will then be used to generate embeddings and summarize talking points derived from recent sales orders and support tickets for a customer.

### Conceptual Overview and Key Concepts
1.	A Resource Group in SAP AI Launchpad is like a project folder that holds and separates all your AI assets (models, configurations, data, etc.) for a specific team or a use case.
2.	The Executable is the pre-built SAP template that handles all the technical complexity of connecting your application to external LLMs like Azure, OpenAI or AWS Bedrock.
3.	The Configuration is your specific recipe card that links the generic executable template to a particular LLM (like 'GPT-4') and sets its parameters (like Temperature) to control its behaviour
4.	The Deployment is the final step that flips the power switch on your LLM configuration, turning it into a live service with a unique web address (URL) that SAP applications can use. In this case, it will be called by the integration flows in the Cloud Integration capability of SAP Integration Suite.

## Exercise 2.1 Log on to SAP AI Launchpad and create configuration  
1.	Log on to [SAP AI Launchpad](https://in162-ntn259xc.ai-launchpad.prod.eu-central-1.aws.ai-prod.cloud.sap/) tenant.
   
      > Note: The system login screen may not appear if you are already authenticated, as other systems connected to the same SAP Identity Authentication Service (IAS) tenant can trigger automatic login.

      If the login page appears, log on using the user ID and password provided by the instructors.

  	   <img src="/exercises/ex2/images/ex21-1.png" alt="Pic 21-1" width=40% height=40%>

2. Each team has been assigned a resource group according to the assigned participant number.
   <br>Verify that the resource group <b>IN162-0**</b> *(replace ** with the participant number that is assigned to you)* is visible in the SAP AI Launchpad.
   <br>Select the available resource group to enable the menu items in the left pane: **Generative AI Hub**, **SAP AI Core Administration** and **ML Operations**.

   > Note: If your resource group is missing or does not match your participant number, contact the instructor.

   ![Pic 21-2](./images/ex21-2.png)

   
3. From the left menu, select **ML Operations -> Configuration**, and then click **Create** button.

   ![Pic 21-3](./images/ex21-3.png)

4. Provide the following details under **Enter Name and Executable** wizard step, and then click **Next** button.
   <br>a.	Enter Configuration Name as <b>IN162-0** Config</b> *(replace ** with the participant number that is assigned to you)*
   <br>b.	Choose Scenario as **orchestration**
   <br>c.	Choose the Version as **0.0.1**
   <br>d.	Choose the Executable as **orchestration**

   ![Pic 21-4](./images/ex21-4.png)

5.	Keep the default values in the **Input Parameters** wizard step, and then click **Next** button.

  	![Pic 21-5](./images/ex21-5.png)
 
6.	No need to provide any detail in the **Input Artifacts** wizard step, and directlt click **Review** button.

  	![Pic 21-6](./images/ex21-6.png)

7.	Finally, **review** the provided details, and then click the **Create** button.

  	![Pic 21-7](./images/ex21-7.png)

## Exercise 2.2 Create Deployment
1.	Open Configuration details **(ML Operations->Configuration->IN162-0XX Config)**
2.	Click Create Deployment button (top right)
<br><br>![](/exercises/ex2/images/IN162-10.png)

3.	Provide Details: Create Deployment -> **Select Scenario**
<br> i.	Select Orchestration
<br><br>![](/exercises/ex2/images/IN162-11.png)

4.	Provide Details: Create Deployment -> **Select Executable**
   <br> i.	Select orchestration and click next
<br><br>![](/exercises/ex2/images/IN162-12.png)

5.	Provide Details: Create Deployment-> **Select Configuration**
<br> i.	Select the Configuration (IN162-0XX Config) you created earlier
<br> ii. Click next
<br><br>![](/exercises/ex2/images/IN162-13.png)

6.	Provide Details: Create Deployment -> **Duration**
   <br> i.	Select standard and click next
<br><br>![](/exercises/ex2/images/IN162-14.png)

7.	Provide Details: Create Deployment -> **Review**
   <br> i.	Click Create

8.	Wait for the Deployment to start Running (it takes a minutes to start the deployment so we can start with other activities in this handson exercise)
   <br> i.	**Come back on this screen** once the Deployment status is “running”
  	<br> ii.	**We would need the deployment id and URL** in our Integration Suite

<br><br>![](/exercises/ex2/images/IN162-15.png)


## Summary

You've now a group specific deployment URL and deployment ID. This url and id will be used in Integration flow to automatically trigger data ingestion by creating embeddings and inserting the data in HANA vector DB. These details will be also used while preparing the latest report for customer.

Continue to - [Exercise 3](../ex3/README.md)
