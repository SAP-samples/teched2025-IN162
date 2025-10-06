# Scenario Introduction

This session scenario helps you to learn more about [**Enterprise Automation**](https://www.sap.com/india/products/technology-platform/enterprise-automation.html) by implementing an end-to-end event-driven scenario where you react and respond to a new hire business event from <b>SAP SuccessFactors</b> using <b>SAP Integration Suite</b> and <b>SAP Build Process Automation</b>.

In this, we implement an end-to-end <b>Recruit-to-Retire (R2R)</b> business subprocess across a heterogeneous landscape. From <b>SAP Integration Suite</b> we would leverage <b>Advanced Event Mesh (AEM)</b> and <b>Cloud Integration</b> capabilities. This also includes human interactions to achieve an end-to-end employee onboarding business process using <b>SAP Build Process Automation</b>.

## Business Scenario
Presume in your organization, you are using <b>SAP SuccessFactors</b> as a central Human Capital Management (HCM) application and you like to automate a few aspects of a newly hired employee in real-time like:

1. You like to send a welcome email to the newly hired candidate along with the survey link to understand the candidate's onboarding experience.

2. You like to initiate the newly hired candidate's equipment and training approval workflow to the manager using the SAP Build Process Automation.

3. Post manager approval, you also like to automate the creation of Purchase Requistion for the approved equipments in SAP S/4HANA Cloud or SAP Ariba system.

Instead of developing point-to-point integration for the above three use cases, you will learn and understand how we can achieve the same via an event-driven integration pattern.

### Scenario Extension Possibilities
The advantage of event-driven integration is that you can easily add or remove subscribers to update your business processes. For example, the given scenario can be easily extended with the following two more integration use cases:

* You like to replicate the new hired data to secondary HCM application in real-time to manage other supporting functions like Employee Benefits.
* Post manager approval, you also like to automatically grant the access to the SAP BTP applications or any other application using the available APIs.

## Scenario Architecture

<img src="/intro/intro1/images/scenario_architecture.png" width=100% height=100%>

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
