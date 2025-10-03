# Exercise 3.2 - Copy an existing IFlow to receive a Sales Order creation event, transform into embeddings, and persist to HANA Vector DB
In this exercise, instead of building an IFlow from scratch, we will copy a completed IFlow from the source package and configure the parameters that have been externalized for the participants.

> [!IMPORTANT]  
> Note that we will be accessing the main solution package in this exercise. Please don't change anything in the solution package in any way. 


## Step 1 - Copy the IFlow from the source Package 

1. Navigate to the 'Integration and APIs' section from the 'Design' tab of your Integration Suite tenant.
   
   Click on a Package titled 'TechEd 2025 IN162 - Solution Package
   <br>![](../ex3/images/image92.png)
2. Navigate to the 'Artifacts' tab of this package, look for an Integration Flow titled 'Sales Order Event to Hana Vector DB for AI Grounding ' and click on the '...' Action button to bring up the action menu and click on 'Copy'.
<br>![](../ex3/images/image101.png)
2. A copy dialog will pop open. **Do not** click on 'Copy' yet. Click on 'Select' to specify the target Package to copy this content into.
<br>![](../ex3/images/image93.png)

1. A package Selection dialog will open up. Select the target package as 'TechEd 2025 IN162-`000`. Note that we created this Package in the first segment of Exercise 3. (`000` will be replaced with your assigned user identifier).
<br>![](../ex3/images/image94.png)

1. Back in the 'Copy' dialog, title the Name of the IFlow as `Sales Order Event to Hana Vector DB for AI Grounding`. After this, click on 'Copy'.
<br>![](../ex3/images/image95.png)

1. After the copy is successful, navigate to the copied package by clicking on the 'Navigate' button in the presented dialog.
<br>![](../ex3/images/image96.png)

## Step 2 - Configure the IFlow with your user settings 
Now that the IFlow has been copied, we will configure it with certain externalized parameters in the next few steps. 

1. Make sure you are in the target package (TechEd 2025 IN162-`000`). Go to the 'Artifact' tab. Select the 'Sales Order Event to Hana Vector DB for AI Grounding' IFlow, click on the '...' Action menu button, and click 'Configure'.
<br>![](../ex3/images/image97.png)

2. The configuration dialog will pop open. In the 'Sender' tab, enter 'IN162-`000`_Sales_Order' in the 'Queue Name' text box. (replace `000` with your actual user identifier)
<br>![](../ex3/images/image98.png)

1. Next, navigate to the 'Receiver' tab and enter the URL `https://api.ai.prod.eu-central-1.aws.ml.hana.ondemand.com/v2/inference/deployments/db1ce5ede0291ce0` in the AI_Launchpad_URL textbox.
<br>![](../ex3/images/image99.png)

1. Move over to the 'More' tab and enter 'IN162-`000`' in the Assigned_Participant_ID text box. (replace `000` with your actual user identifier)

    Click on 'Save' to save your configuration settings.
<br>![](../ex3/images/image100.png)

## Step 3 - Deploy the IFlow 
Now that the configuration is complete, we will move ahead and deploy the IFlow.

1. Click on 'Deploy' in the configuration dialog from the previous step. Select the default 'Cloud Integration' as the runtime profile to deploy the content into.
<br>![](../ex3/images/image102.png)

1. A dialog will confirm the deploy action.
<br>![](../ex3/images/image103.png)

1. Head back to the 'Artifact' tab, and a message toast will confirm the successful completion of the deployment step.
<br>![](../ex3/images/image104.png)

1. The IFLow is in the 'starting' phase now. Click on the IFlow to bring up the model. After a minute or so, you should see the 'Runtime Status' change to 'Started'. This means that the IFlow is ready and listening for changes.
<br>![](../ex3/images/image105.png)

## Step 4 - Study the sequence of steps in the IFlow 
It is highly recommended that you review the IFlow in detail to gain a broader understanding of its functionality. You can do so by navigating to [Exercise 3.1](./ex3_1_details.md) and following the complete sequence.
