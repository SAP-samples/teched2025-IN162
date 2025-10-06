# Exercise 2 - Gen AI Hub (With AI Core) – Use an embedding model and expose as an API

In this exercise, we will create configuration and orchestration deployments in SAP Generative AI Hub. Orchestartion Deployment URL will be used for creating embeddings and to generate reports from historical data for sales orders and tickets for a customer. 
### Background Information for understanding
1.	A Resource Group in SAP AI Launchpad is like a project folder that holds and separates all your AI assets (models, configurations, data, etc.) for a specific team or use case.
2.	The Executable is the pre-built SAP template that handles all the technical complexity of connecting your application to external LLMs like Azure OpenAI or AWS Bedrock.
3.	The Configuration is your specific recipe card that links the generic Executable template to a particular LLM (like 'GPT-4') and sets its parameters (like Temperature) to control its behaviour
4.	The Deployment is the final step that flips the power switch on your LLM Configuration, making it a live service with a unique web address (URL) for SAP applications to use.

## Exercise 2.1 Login into the SAP AI Launchpad  
1.	Open URL: https://in162-ntn259xc.ai-launchpad.prod.eu-central-1.aws.ai-prod.cloud.sap/
<br><br>![](/exercises/ex2/images/IN162-1.png)

2.	Enter Usermame/Password as provided by the moderator
<br><br>![](/exercises/ex2/images/IN162-2.png)

3. Check and confirm if you have the resource group IN162-0XX in the SAP AI Lauchpad. We have already created Resource Group for you every group as per your group name. (contact moderators if you do not have resource group as per your group name) 
<br><br>![](/exercises/ex2/images/IN162-3.png)

4. Select the resource group IN162-0XX to enable 3 menu items in left pane (Generative AI Hub, ML Operations, SAP AI Core Administration)
5. Select ML Operations
<br><br>![](/exercises/ex2/images/IN162-4.png)

## Exercise 2.2 Create configuration 
1. Select configuration in the left menu and then click on create button
<br><br>![](/exercises/ex2/images/IN162-5.png)

2. Provide details: Create configuration -> **Enter Name and executable**
   <br>i.	Enter Configuration Name as IN162-0XX Config (provide your group name at XX)
   <br>ii.	Choose default scenario (orchestration)
   <br>iii.	Choose the version available (0.0.1)
   <br>iv.	Choose the executable (orchestration)
<br><br>![](/exercises/ex2/images/IN162-6.png)

3.	Provide details: Create configuration -> **Input Parameters**
<br> i.	Click next **(keep defaults)**
<br><br>![](/exercises/ex2/images/IN162-7.png)

4.	Provide details: Create configuration -> **Input Artifacts**
<br>i.	Click Review
<br><br>![](/exercises/ex2/images/IN162-8.png)

5.	Provide details: Create configuration -> **Review**
<br>i.	Click Create
<br><br>![](/exercises/ex2/images/IN162-9.png)

## Exercise 2.3 Create Deployment
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
