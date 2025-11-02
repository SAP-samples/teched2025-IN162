# Exercise 4.2 - Copy an existing IFlow to receive a Support Case creation event, transform into embeddings, and persist to HANA Vector DB

Make sure you come to this exercise after completing [Exercise 4](./README.md).

In this exercise, instead of building an IFlow from scratch, we will copy a completed IFlow from the source package and configure the parameters that have been externalized for the participants.

> [!IMPORTANT]  
> Note that we will be accessing the main solution package in this exercise. Please don't change anything in the solution package in any way. 


## Step 1 - Copy the IFlow from the source Package 

1. Navigate to the 'Integration and APIs' section from the 'Design' tab of your Integration Suite tenant.
   
   Click on a Package titled '**TechEd 2025 IN162 - Solution Package**'
  
<br>![](../ex4/images/ex162-42-1.png)
<br><br>
2. Navigate to the 'Artifacts' tab of this package, look for an Integration Flow titled '**Support Case Event to Hana Vector DB for AI Grounding**' and click on the '...' Action button to bring up the action menu and click on '**Copy**'.

<br>![](../ex4/images/ex162-42-2.png)
<br><br>

3. A copy dialog will pop open. **Do not** click on 'Copy' yet. Click on 'Select' to specify the target Package to copy this content into.

<br>![](../ex4/images/ex162-42-3.png)
<br><br>

4. A package Selection dialog will open up. Select the target package as 'TechEd 2025 IN162-`0**`. Note that we created this Package in the first segment of Exercise 4. (`0**` will be replaced with your assigned user identifier).

<br>![](../ex4/images/ex162-42-4.png)
<br><br>

1. Back in the 'Copy' dialog, title the Name of the IFlow as   
 ```
   Support Case Event to Hana Vector DB for AI Grounding - IN162-0**
   ```
> [!NOTE]
> Replace the `0**` with your assigned user identifier.
   After this, click on 'Copy'.

<br>![](../ex4/images/ex162-42-5.png)
<br><br>

1. After the copy is successful, navigate to the copied package by clicking on the 'Navigate' button in the presented dialog.

<br>![](../ex4/images/ex162-42-6.png)
<br><br>

## Step 2 - Configure the IFlow with your user settings 
Now that the IFlow has been copied, we will configure it with certain externalized parameters in the next few steps. 

1. Make sure you are in the target package (TechEd 2025 IN162-`0**`). Go to the 'Artifact' tab. Select the '**Support Case Event to Hana Vector DB for AI Grounding**' IFlow, click on the '...' Action menu button, and click 'Configure'.

<br>![](../ex4/images/ex162-42-7.png)
<br><br>

2. The configuration dialog will pop open. In the 'Sender' tab, enter 'IN162-`0**`_Support_Case' in the 'Queue Name' text box. (replace `0**` with your actual user identifier)

<br>![](../ex4/images/ex162-42-8.png)
<br><br>

3. Next, navigate to the 'Receiver' tab and enter the URL: 'https://api.ai.prod.eu-central-1.aws.ml.hana.ondemand.com/v2/inference/deployments/`your-deployment-id`' in the AI_Launchpad_URL textbox.
   > [!NOTE]
   > Copy the `deployment-id-here` from [Exercise 2](../ex2/README.md#exercise-22---create-deployment) where you created the deployment.

<br>![](../ex4/images/ex162-42-9.png)
<br><br>

4. Move over to the 'More' tab and enter 'IN162-`0**`' in the Assigned_Participant_ID text box. (replace `0**` with your actual user identifier)

    Click on 'Save' to save your configuration settings.

<br>![](../ex4/images/ex162-42-10.png)
<br><br>

 ## Step 3 - Deploy the IFlow 
Now that the configuration is complete, we will move ahead and deploy the IFlow.

1. Click on 'Deploy' in the configuration dialog from the previous step OR you can open the editor to 'Deploy' the flow
   Select the default 'Cloud Integration' as the runtime profile to deploy the content into.

<br>![](../ex4/images/ex162-42-11.png)
<br><br>

2. A dialog will confirm the deploy action.

<br>![](../ex4/images/ex162-42-12.png)
<br><br>

3. A message toast will confirm the successful completion of the deployment step.

<br>![](../ex4/images/ex162-42-13.png)
<br><br>

4. The IFLow is in the 'starting' phase now. Click on the IFlow to bring up the model. After a minute or so, you should see the 'Runtime Status' change to 'Started'. This means that the IFlow is ready and listening for changes.

<br>![](../ex4/images/ex162-42-14.png)
<br><br>

## Step 4 - Study the sequence of steps in the IFlow 
It is highly recommended that you review the IFlow in detail to gain a broader understanding of its functionality. You can do so by navigating to [Exercise 4.1](./ex4_1_details.md) and inspecting the complete sequence.

<br>![](../ex4/images/ex162-42-15.png)
<br><br>
## Summary
This completes Exercise 4. Next, proceed to [Exercise 5](../ex5/README.md), where we will initiate the creation of Sales Order and Support Cases from **S/4HANA Cloud** and **SAP Support Cloud V2**, respectively, and monitor the Integration Flows that finally get triggered via the AEM Adapters.
