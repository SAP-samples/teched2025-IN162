# Exercise 2 - Gen AI Hub (With AI Core) – Use an embedding model and expose as an API

In this exercise, we will create configuration and orchestration deployments in SAP Generative AI Hub. Orchestartion Deployment URL will be used for creating embeddings and to generate reports from historical data for sales orders and tickets for a customer. 
### Background Information for understanding
1.	A Resource Group in SAP AI Launchpad is like a project folder that holds and separates all your AI assets (models, configurations, data, etc.) for a specific team or use case.
2.	The Executable is the pre-built SAP template that handles all the technical complexity of connecting your application to external LLMs like Azure OpenAI or AWS Bedrock.
3.	The Configuration is your specific recipe card that links the generic Executable template to a particular LLM (like 'GPT-4') and sets its parameters (like Temperature) to control its behaviour
4.	The Deployment is the final step that flips the power switch on your LLM Configuration, making it a live service with a unique web address (URL) for SAP applications to use.

## Exercise 2.1 Login into the SAP AI Launchpad  
1.	Open URL: https://in162-ntn259xc.ai-launchpad.prod.eu-central-1.aws.ai-prod.cloud.sap/
<br>![](/exercises/ex2/images/Login1.png)

3.	Usermame/Password: provided by the moderator




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
