# Scenario Introduction

This session scenario helps you to learn and apply real-time vector grounding by connecting structured data to an LLM using a **SAP Hana Cloud vector database, SAP Integration Suite and SAP Integration Suite, advanced event mesh**. You’ll see how updates to the data can instantly influence the model’s responses, keeping them current and contextually accurate. By the end, you’ll build an end-to-end, event-driven flow that reacts to **new sales order events from SAP S/4HANA Cloud and new support case events from SAP Service Cloud Version 2**, providing the context to AI assistant in real time.

## Business Scenario
Assume your organization uses an **AI-powered Customer Success Digital Assistant**. The assistant helps customer success managers prepare for customer meetings by **highlighting and summarizing the latest sales orders and support tickets**, and by suggesting key talking points. To ensure accurate, up-to-date context, the assistant needs real-time access to sales order and support ticket data. And for this we will leverage event-driven integration pattern to deliver real-time grounding data, equipping AI system's with the latest business context to make informed and accurate decisions.

## Scenario Architecture

<img src="/intro/intro1/images/IN162_scenario_architecture.png" width=100% height=100%>

1. Add(hire) a new employee in <b>SAP SuccessFactors</b> system.
   <br> 1a. The new employee data event gets <b>published</b> using the <b>REST</b> interface directly to <b>SAP Integration Suite, advanced event mesh</b> topic `SuccessFactors/NewHire/{EmployeeId}` where `EmployeeId` gets dynamically resolved from the new hire payload. For this, we need to do the proper [configurations](/intro/intro2) and this has already been done on the given SAP SuccessFactors system.

2. <b>First Subscriber</b> listens to the AEM queue that is subscribed to the topic `SuccessFactors/NewHire/{EmployeeId}` using the Cloud Integration <b>AMQP</b> sender adapter.
    <br> 2a. It sends a welcome email to the given newly hired candidate's email ID along with the survey link using the Cloud Integration <b>Mail</b> receiver adapter.
    <br> 2b. By clicking the survey link, candidate can provide the onboarding experience feedback.

3. <b>Second Subscriber</b> listens to the different AEM queue that is also subscribed to the same topic `SuccessFactors/NewHire/{EmployeeId}` using the Cloud Integration <b>AMQP</b> sender adapter.
    <br> 3a. It triggers the equipment and training approval workflow in SAP Build Process Automation with Equipment and Training <b>Decisions</b> and assigns the task to the manager on the given manager's email ID using the Cloud Integration <b>HTTPS</b> receiver adapter. This will be visible in the Manager's My Inbox.
    <br> 3b. On manager's approval or rejection, workflow notify the same to the newly hired candidate on the given candidate's email ID.
    <br> 3c. On manager's approval, it also <b>publishes</b> the approval event directly to the <b>SAP Integration Suite, advanced event mesh</b> topic `SBPA/NewHire/{EmployeeId}/Approval` where `EmployeeId` gets dynamically resolved from the new hire payload.

4. <b>Third Subscriber</b> listens to the another AEM queue that is subscribed to the approval topic `SBPA/NewHire/{EmployeeId}/Approval` using the Cloud Integration <b>AMQP</b> sender adapter.
    <br> 4a. It automatically creates a purchase requisition (PR) with the approved equipment list in the S/4HANA Cloud system using the <b>OData</b> receiver adapter.
    <br> 4b. Once the purchase requisition gets created, Cloud Integration sends an email to the given newly hired candidate's email ID with the PR number and direct link to open the same in SAP S/4Hana Cloud system.
    
## Summary

You should now be familiar with the session scenario.

Now, to learn all the configurations that has been done in <b>SAP SuccessFactors</b> to enable new hire event publication to <b>SAP Integration Suite, advanced event mesh</b>, you can navigate to [SAP SuccessFactors Configuration](/intro/intro2/README.md) section.
