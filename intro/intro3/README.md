# SAP Service Cloud Version 2 System Configurations (for your information only)

As per the scenario, we also need to create a new **Support Case** in **SAP Service Cloud Version 2** system.
This will **publish an event** to **SAP Integration Suite, advanced event mesh** from where it will be subscribed by a **SAP Integration Suite integration flow**. Although the standard Support Case business event is the data event but still the integration flow will call the Support Case API to fetch the additional details.

To enable the publishing of **Support Case event** to **SAP Integration Suite, advanced event mesh**, the following configuration steps have already been completed in **SAP Service Cloud Version 2** system for you. 
<br>**This section is just for your knowledge and information only.**

## Exercise 1.1 Sub Exercise 1 Description

After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex1/images/01_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```



## Exercise 1.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

