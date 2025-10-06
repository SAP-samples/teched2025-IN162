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







<br><br><br><br><br><br><br><br>
----
After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex2/images/Login1.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello ABAP World! | ). 
```



## Exercise 2.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc = 0.
    response->set_status( i_code = 200
                     i_reason = 'Everything is fine').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)
