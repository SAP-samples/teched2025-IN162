# IN162 -  Real-time data for AI agents: Grounding with event-driven architecture.

## Description

This repository contains the material for the SAP TechEd 2025 session called Session ID - Session Title.  

## Overview

This session introduces attendees to..

> [!IMPORTANT]  
> Check out the following chapters to understand and achieve the end-to-end grounding with event-driven architecture as documented in this session:
>
> - [Scenario Introduction](intro/intro1/README.md)
> - [SAP S/4Hana Cloud System configuration (for your information only)](intro/intro2/README.md)
> - [SAP Service Cloud Version 2 configuration (for your information only)](intro/intro3/README.md)

## Requirements

There are no dedicated requirement for this session. You would be able to execute the excercises by just following the descriptions even if you do not have any experience with SAP Integration Suite and SAP Integration Suite, advanced event mesh. <br/>
However, you will be able to derive more value from this session, if you have some knowledge and understanding of event-driven architectures, namely events, queues, topics, event subscriptions along with SAP Integration Suite and SAP Build Process Automation.<br/>

You can check out the following SAP Discovery Center missions that will help you in getting started with SAP Integration Suite, SAP Build Proces Automation and SAP Integration Suite, advanced event mesh 

* [SAP Integration Suite](https://discovery-center.cloud.sap/serviceCatalog/integration-suite)
* [SAP Integration Suite, advanced event mesh](https://discovery-center.cloud.sap/serviceCatalog/advanced-event-mesh)

## System URL and login information

For running through the exercises, you need access to the following applications:
- [SAP S/4Hana Cloud system](https://my427029.s4hana.cloud.sap/ui)
- [SAP Integration Suite, advanced event mesh tenant](https://eu10.console.pubsub.em.services.cloud.sap/login?tenant-id=8b4a1697-2b58-4571-a986-1377cc070073)
- [SAP Integration Suite tenant](https://workshop-eu-01a.integrationsuite-cpi033.cfapps.eu10-005.hana.ondemand.com)
- [SAP Service Cloud Version 2](https://my1001903.de1.demo.crm.cloud.sap/)


> [!IMPORTANT]
> - We have done everything to make this experience enjoyable. Your tenants are pre-configured and you already have all the roles and definitions you need to complete this exercise.
> - User ID and password information will be provided to you by the instructors.
> - When you run through the exercise steps, you need to ensure that the technical IDs of the integration artifacts that you will create are unique. Hence, add a participant number to your integration artifacts. The instructors will assign the participant number to you.
> - Please adhere strictly to the instructions regarding the naming conventions for the artifacts you create. This will ensure successful completion of the tasks without conflicting with other participants.
> - Do not delete, change or undeploy any artifact in the tenant other than yours.

## Exercises

The complete list of exercise steps are listed below, run through them in the given order.
<br>You can use this section as a Table of Contents. Use the breadcrumb navigation on top of the pages to go back to the Table of Contents.

- [Getting Started](exercises/ex0/)
- [Exercise 1 - Explore SAP Integration Suite, advanced event mesh](exercises/ex1/)
  - [Exercise 1.1 - Log into Advanced Event Mesh and Explore it](exercises/ex1#exercise-11---log-into-advanced-event-mesh-and-explore-it)
  - [Exercise 1.2 - Create a Queue in Advanced Event Mesh ](exercises/ex1#exercise-12---create-a-queue-in-advanced-event-mesh)
  - [Exercise 1.3 - Create a Queue Subscription in Advanced Event Mesh](exercises/ex1#exercise-13---create-a-queue-subscription-in-advanced-event-mesh)
  - [Exercise 1.4 - Create an additional Queue and Queue Subscription in Advanced Event Mesh](exercises/ex1#exercise-14---create-an-additional-queue-and-queue-subscription-in-advanced-event-mesh)
  - [Exercise 1.5 - Send an event from the Try Me! tool to your Topic](exercises/ex1#exercise-15---send-an-event-from-the-try-me-tool-to-your-topic)
- [Exercise 2 - Gen AI Hub (With AI Core) – Use an embedding model and expose as an API](exercises/ex2/)
    - [Exercise 2.1 - Exercise 2 Sub Exercise 1 Description](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Exercise 2 Sub Exercise 2 Description](exercises/ex2#exercise-22-sub-exercise-2-description)
- [Exercise 3 - Integration Suite – Iflow S4Hana Sales Order to Embedding Model to SAP Hana Vector DB](exercises/ex3/README.md)
    - [Exercise 3.1 - Create a new Sales Order in S/4Hana Cloud system](./exercises/ex3/ex3_1_details.md)
    - [Exercise 3.2 - Create an IFLow with the created Sales Order, transform into embeddings and persist to HANA Vector DB](./exercises/ex3/ex3_2_details.md)
    - [Exercise 3.3 - Monitor IFlow execution](exercises/ex3#exercise-33---monitor-iflow-execution)
- [Exercise 4 - Integration Suite - Iflow Service Cloud Support Case to Embedding Model to SAP Hana Vector DB](exercises/ex4/)
    - [Exercise 4.1 - Create a Support Ticket with iflow execution](exercises/ex4#tbd)
- [Exercise 5 - Gen AI Hub (With AI Core) – Use an embedding model and expose as an API](exercises/ex5/)
- [Exercise 6 - AP Build based Joule Assistant with Joule Skill](exercises/ex6/)
    - [Exercise 6.1 - Already been created with one iflow and Gen AI Hub API](exercises/ex6#tbd)
    - [Exercise 6.2 - Execution with the prompt ](exercises/ex6#tbd)

  
**OR** Link to the Tutorial Navigator for example...

Start the exercises [here](https://developers.sap.com/tutorials/abap-environment-trial-onboarding.html).

**IMPORTANT**

Your repo must contain the .reuse and LICENSES folder and the License section below. DO NOT REMOVE the section or folders/files. Also, remove all unused template assets(images, folders, etc) from the exercises folder. 

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
