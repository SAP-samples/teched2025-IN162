# Exercise 4.2 - Copy an existing IFlow to receive a Support Case creation event, transform into embeddings, and persist to HANA Vector DB

In this exercise, instead of building an IFlow from scratch, we will copy a completed IFlow from the source package and configure the parameters that have been externalized for the participants.

> [!IMPORTANT]  
> Note that we will be accessing the main solution package in this exercise. Please don't change anything in the solution package in any way. 


## Step 1 - Copy the IFlow from the source Package 

1. Navigate to the 'Integration and APIs' section from the 'Design' tab of your Integration Suite tenant.
   
   Click on a Package titled 'TechEd 2025 IN162 - Solution Package
   <br>![](../ex3/images/image92.png)

2. Navigate to the 'Artifacts' tab of this package, look for an Integration Flow titled 'Support Case Event to Hana Vector DB for AI Grounding' and click on the '...' Action button to bring up the action menu and click on 'Copy'.
<br>![](../ex4/images/image-71.png)

1. A copy dialog will pop open. **Do not** click on 'Copy' yet. Click on 'Select' to specify the target Package to copy this content into.
<br>![](../ex4/images/image-72.png)

 1. A package Selection dialog will open up. Select the target package as 'TechEd 2025 IN162-`000`. Note that we created this Package in the first segment of Exercise 4. (`000` will be replaced with your assigned user identifier).
<br>![](../ex4/images/image-73.png)

 2. Back in the 'Copy' dialog, title the Name of the IFlow as `Support Case Event to Hana Vector DB for AI Grounding`. After this, click on 'Copy'.
<br>![](../ex4/images/image-74.png)

1. After the copy is successful, navigate to the copied package by clicking on the 'Navigate' button in the presented dialog.
<br>![](../ex4/images/image-75.png)

## Step 2 - Configure the IFlow with your user settings 
Now that the IFlow has been copied, we will configure it with certain externalized parameters in the next few steps. 

1. Make sure you are in the target package (TechEd 2025 IN162-`000`). Go to the 'Artifact' tab. Select the 'Support Case Event to Hana Vector DB for AI Grounding' IFlow, click on the '...' Action menu button, and click 'Configure'.
<br>![](../ex4/images/image-76.png)

2. The configuration dialog will pop open. In the 'Sender' tab, enter 'IN162-`000`_Support_Case' in the 'Queue Name' text box. (replace `000` with your actual user identifier)
<br>![](../ex4/images/image-77.png)

1. Next, navigate to the 'Receiver' tab and enter the URL `https://api.ai.prod.eu-central-1.aws.ml.hana.ondemand.com/v2/inference/deployments/db1ce5ede0291ce0` in the AI_Launchpad_URL textbox.
<br>![](../ex4/images/image-78.png)

1. Move over to the 'More' tab and enter 'IN162-`000`' in the Assigned_Participant_ID text box. (replace `000` with your actual user identifier)

    Click on 'Save' to save your configuration settings.
<br>![](../ex4/images/image-79.png)

 ## Step 3 - Deploy the IFlow 
Now that the configuration is complete, we will move ahead and deploy the IFlow.

1. Click on 'Deploy' in the configuration dialog from the previous step. Select the default 'Cloud Integration' as the runtime profile to deploy the content into.
<br>![](../ex4/images/image-80.png)

1. A dialog will confirm the deploy action.
<br>![](../ex4/images/image-81.png)

1. Head back to the 'Artifact' tab, and a message toast will confirm the successful completion of the deployment step.
<br>![](../ex4/images/image-82.png)

1. The IFLow is in the 'starting' phase now. Click on the IFlow to bring up the model. After a minute or so, you should see the 'Runtime Status' change to 'Started'. This means that the IFlow is ready and listening for changes.
<br>![](../ex4/images/image-83.png)

## Step 4 - Study the sequence of steps in the IFlow 
It is highly recommended that you review the IFlow in detail to gain a broader understanding of its functionality. You can do so by navigating to [Exercise 4.1](./ex4_1_details.md) and inspecting the complete sequence.
