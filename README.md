# IN162 -  Real-time data for AI agents: Grounding with event-driven architecture

## Description

This repository contains the material for the SAP TechEd 2025 Hands-on Workshop session [IN162 | Real-time data for AI agents: Grounding with event-driven architecture](https://www.sap.com/events/teched/berlin/flow/sap/te25/catalog-inperson/page/catalog/session/1748712252655001fP5Z)  

## Overview

This session introduces attendees to the power of event-driven integration pattern to deliver real-time grounding data, equipping AI system's with the latest business context to make informed and accurate decisions

> [!NOTE]  
> AI Grounding is the process of connecting an AI system's abstract knowledge and responses to specific, real-world data and context, reducing "hallucinations" and increasing the reliability and accuracy of its outputs. 

Although AI Grounding is primarily used for unstructured text documents, it can also be extended to real-time structured business objects by embedding key attributes or metadata. This approach helps improve the factual accuracy and contextual relevance of AI responses when dealing with transactional business data.

In this session, we’ll explore how to apply real-time vector grounding by connecting structured data to an LLM using a vector database using **SAP Integration Suite and SAP Integration Suite, advanced event mesh**. You’ll see how updates to the data can instantly influence the model’s responses, keeping them current and contextually accurate.

By the end of this session, you’ll understand the core principles behind real-time vector grounding, how to design an embedding strategy for structured entities, and how to integrate this into a sample AI workflow.

The following chapter explains the scenario in more detail:

- [Scenario Introduction](intro/intro1/README.md)

## Requirements

There are no dedicated requirement for this session. You would be able to execute the excercises by just following the descriptions even if you do not have any experience with **SAP Integration Suite and SAP Integration Suite, advanced event mesh.** 


However, you will be able to derive more value from this session, if you have some knowledge and understanding of event-driven architectures, namely events, queues, topics, event subscriptions along with SAP Integration Suite, SAP Joule Studio and SAP Build Process Automation Actions.<br/>

You can explore the following SAP Discovery Center missions to get started and build expertise with the SAP BTP services used in this hands-on workshop session.

* [SAP Integration Suite](https://discovery-center.cloud.sap/serviceCatalog/integration-suite)
* [SAP Integration Suite, advanced event mesh](https://discovery-center.cloud.sap/serviceCatalog/advanced-event-mesh)
* [SAP AI Launchpad](https://discovery-center.cloud.sap/serviceCatalog/sap-ai-launchpad)
* [SAP AI Core](https://discovery-center.cloud.sap/serviceCatalog/sap-ai-core)
* [Joule Studio, skill builder](https://discovery-center.cloud.sap/ai-feature/e93aa292-e7f4-449d-9586-f1a8510d5ab6/)
* [SAP Build Process Automation](https://discovery-center.cloud.sap/serviceCatalog/sap-build-process-automation)

You can also gain some further knowledge around SAP Integration Suite and SAP Integration Suite, advanced event mesh by attending the following SAP TechEd Hands-on Workshop sessions:

- [IN165 | Experience event-driven integration with advanced event mesh](https://www.sap.com/events/teched/berlin/flow/sap/te25/catalog-inperson/page/catalog/session/1749789125498001xZYY)
- [IN161 | Modernize and transform your integration to the cloud](https://www.sap.com/events/teched/berlin/flow/sap/te25/catalog-inperson/page/catalog/session/1748711963531001B8Pr)
- [IN160 | Empower your business through enterprise automation](https://www.sap.com/events/teched/berlin/flow/sap/te25/catalog-inperson/page/catalog/session/1748711728862001rC69)

## System URL and login information

For running through the exercises, you need access to the following applications:
- [SAP Integration Suite, advanced event mesh](https://eu10.console.pubsub.em.services.cloud.sap/login?tenant-id=8b4a1697-2b58-4571-a986-1377cc070073)
- [SAP Integration Suite](https://workshop-eu-01a.integrationsuite-cpi033.cfapps.eu10-005.hana.ondemand.com/shell/design)
- [SAP AI Launchpad](https://in162-ntn259xc.ai-launchpad.prod.eu-central-1.aws.ai-prod.cloud.sap/)
- [SAP S/4HANA Cloud](https://my427029.s4hana.cloud.sap/ui)
- [SAP Service Cloud Version 2](https://my1001903.de1.demo.crm.cloud.sap/)
- [SAP Joule: Sample Customer Success Digital Assistant](https://in162-ntn259xc.eu10.sapdas.cloud.sap/webclient/standalone/sap_digital_assistant)



> [!IMPORTANT]
> - _For a smooth experience, tenants have been preconfigured, and you already have all the roles and permissions needed to complete this exercise._
> - _User ID and password information will be provided to you by the instructors._
> - _When you run through the exercise steps, you need to ensure that the technical IDs of the integration artifacts that you will create are unique. Hence, add a participant number to your integration artifacts. The instructors will assign the participant number to you._
> - _Please adhere strictly to the instructions regarding the naming conventions for the artifacts you create. This will ensure successful completion of the tasks without conflicting with other participants._
> - _Do not delete, change or undeploy any artifact in the tenant other than yours._

## Pre-configured Setup
Chapters in this section provide pre-configured setups to support learning, without being part of the hands-on exercises:

- [SAP S/4HANA Cloud System Configurations](intro/intro2/README.md)
- [SAP Service Cloud Version 2 System Configurations](intro/intro3/README.md)

## Exercises
The complete list of exercise steps are listed below, run through them in the given order.
<br>This section also serves as a Table of Contents; use the breadcrumb navigation at the top of the pages to return here at any time.

- [Exercise 1 - Explore and Configure SAP Integration Suite, advanced event mesh (AEM)](exercises/ex1/)
  - [Exercise 1.1 - Log on to SAP Integration Suite, advanced event mesh (AEM) and explore it](exercises/ex1#exercise-11---log-on-to-sap-integration-suite-advanced-event-mesh-aem-and-explore-it)
  - [Exercise 1.2 - Create first queue and subscribe to sales order topic in SAP Integration Suite, advanced event mesh (AEM)](exercises/ex1#exercise-12---create-first-queue-and-subscribe-to-sales-order-topic-in-sap-integration-suite-advanced-event-mesh-aem)
  - [Exercise 1.3 - Create second queue and subscribe to support case topic in SAP Integration Suite, advanced event mesh (AEM)](exercises/ex1#exercise-13---create-second-queue-and-subscribe-to-support-case-topic-in-sap-integration-suite-advanced-event-mesh-aem)
  - [Exercise 1.4 - Send an event from the Try Me! tool to your Topic](exercises/ex1#exercise-15---send-an-event-from-the-try-me-tool-to-your-topic)
- [Exercise 2 - Gen AI Hub (With AI Core) – Use an embedding model and expose as an API](exercises/ex2/)
    - [Exercise 2.1 - Login into the SAP AI Launchpad and create configuration](exercises/ex2#exercise-21-login-into-the-sap-ai-launchpad-and-create-configuration)
    - [Exercise 2.2 - Create Deployment](exercises/ex2#exercise-22-create-deployment)
- [Exercise 3 - Integration Suite – Iflow S4Hana Sales Order to Embedding Model to SAP Hana Vector DB](exercises/ex3/README.md)
    - [Exercise 3.1 - Create an IFLow from scratch to receive a Sales Order creation event, transform into embeddings and persist to HANA Vector DB](./exercises/ex3/ex3_2_details.md)
    - [Exercise 3.2 (Optional and an alternate to Exercise 3.1)  - Copy an existing IFLow to reciever a Sales Order creation event, transform into embeddings and persist to HANA Vector DB](./exercises/ex3/ex3_2_details.md)
- [Exercise 4 - Integration Suite - IFlow Service Cloud Support Case to Embedding Model to SAP Hana Vector DB](exercises/ex4/)
    - [Exercise 4.1 - Create an IFLow from scratch to receive a Support Case creation event, transform into embeddings and persist to HANA Vector DB](exercises/ex4/README.md)
    - [Exercise 4.2 (Optional and an alternate to Exercise 4.1) - Copy an existing IFLow to receive a Support Case creation event, transform into embeddings and persist to HANA Vector DB](exercises/ex4/README.md)
- [Exercise 5 - Triggering the Integration by creating a Sales Order and Monitoring the runtime flows](exercises/ex5/README.md)
    - [Exercise 5.1 - Create a new Sales Order in S/4HANA Cloud system](exercises/ex5/README.md)
    - [Exercise 5.2 - Monitor IFlow execution](exercises/ex5/README.md)
- [Exercise 6 - SAP Build based Joule Assistant with Joule Skill](exercises/ex6/)
    - [Exercise 6.1 - Already been created with one IFlow and Gen AI Hub API](exercises/ex6/README.md)
    - [Exercise 6.2 - Execution with the prompt ](exercises/ex6/README.md)

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
