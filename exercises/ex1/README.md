# Exercise 1 - Explore and Configure SAP Integration Suite, advanced event mesh (AEM)

## Overview

To enable real-time grounding with an event-driven architecture, we use **SAP Integration Suite, advanced event mesh (AEM)**.<br/>
In this exercise, you will explore and familiarize yourself with AEM. Each participant needs to create 2 queues and subscribe to the relevant topic.
These queues will be used later in the session when creating or configuring integration interfaces in the Cloud Integration capability of SAP Integration Suite.

## Exercise 1.1 - Log on to SAP Integration Suite, advanced event mesh (AEM) and explore it
After completing these steps, you will have familiarized yourself with AEM. These steps will give you a first impression and an overview of AEM.

1. Log on to [SAP Integration Suite, advanced event mesh](https://eu10.console.pubsub.em.services.cloud.sap/login?tenant-id=8b4a1697-2b58-4571-a986-1377cc070073) tenant using the user ID and password that is already provided by the instructors.

	![Pic 11-1](./images/ex11-1.png)

2. Explore **SAP Integration Suite, advanced event mesh (AEM)** tenant.
   <br>Check out the areas in the AEM cockpit to discover the different categories of services AEM offers:

	- **Event Portal:** Event Portal provides event management services. This subscribed service provides powerful tools to create, design, share, and manage various aspects of an EDA based on event brokers or other streaming technologies (such as Kafka).

	- **Mission Control:** Mission Control makes it easy to deploy event brokers, create event meshes, and optimize and monitor the health/performance of an event-driven system. Mission Control is a section in the Cloud Console that permits you to access event brokers, visualize and manage your event broker services, and visualize and design event meshes. Mission Control has a Cluster Manager and Mesh Manager that permits you to create event broker services and manage your event mesh.

     	- **Cluster Manager:** event broker services are made available via Cluster Manager. Each event broker service consists of event brokers configured in a High-Availability (HA) setup.

	- **Event Monitoring and Insights:** With Insights, we provide curated dashboards, easy-to-understand visualizations based on historical and real-time metrics, and timely notifications about your event broker services. This advanced information allows you to identify problems before they occur and helps you to better manage your services as your EDA scales. You can work with SAP to configure your monitoring to meet your needs. For advanced monitoring requirements, there's a single entry point to build custom visualizations to meet your organization's requirements. Coupled with visualizations is a notification email framework that alerts you when key metrics fall outside of your established thresholds. These notifications allow you to monitor what's occurring and correct developing issues before they impact or degrade your EDA. You can configure these notifications to integrate with your existing notification and logging systems.

	![Pic 11-2](./images/ex11-2.png)

3. Click on **Cluster Manager**, and ensure that the **"Only show my services"** checkbox is unchecked.
   <br>Then, select the **teched-2025-europe** tile.

	![Pic 11-3](./images/ex11-3.png)  

4. Switch to the **Connect** tab. This tab provides the various connectivity options and details based on the language and protocol.

   ![Pic 11-4](./images/ex11-4.png)
   
5. Filter the **View by Protocol**. From these protocol connectivity option, **REST** connectivity details is used by **SAP S/4HANA Cloud and SAP Service Cloud Version 2 system** to send events to AEM.
   <br>You can get more information about the configurations [here.](../../intro/intro2/README.md)

   ![Pic 11-5](./images/ex11-5.png)
6. From these protocol connectivity options, **Solace Messaging** based on the **Solace Message Format (SMF) protocol** details will be used by the **Integration Flows in the Cloud Integration capability of SAP Integration Suite** to subscribe to AEM Queues.

   ![Pic 11-6](./images/ex11-6.png)
   
7. On the right side of the screen, click the button labeled **Open Broker Manager**. It will open in a new browser tab or window.
	
	![Pic 11-7](./images/ex11-7.png)

8. Explore the **Broker Manager** screen.
   <br>On the left side of the screen, you will find the main sections for navigation:

	- **Message VPN:** VPN-level stats and config (a Message VPN is a virtual partition of a single broker. One AEM broker can host multiple Message VPNs, and each VPN can have different authorization schemes and topic spaces; client/messaging application activity happens within the scope of a VPN).
	- **Clients:** Information about connected and configured client applications.
	- **Queues:** Used for guaranteed/persistent messaging.
	- **Connector Wizards:** Used to connect to a variety of web services.
	- **Access Control:** Used to create new client usernames, access control profiles, and client profiles.
	- **Replay:** Used to enable replay functionality, to allow the broker to send previous messages again.
		> Note: AEM brokers do not use replay for recovery of persistent data (like Kafka). There is a more fine-grained approach in AEM where each individual message is Acknowledged to the broker when the consumer application is done with it.
	- **Try Me!:** Used to connect to WebSocket test applications.

	![Pic 11-8](./images/ex1-exploreBroker.png)  

## Exercise 1.2 - Create first queue and subscribe to sales order topic in SAP Integration Suite, advanced event mesh (AEM)
After completing these steps, you will have created the first queue subscribed to the sales order topic published from the SAP S/4HANA Cloud System in AEM.

1. Go back to the original AEM tab in your browser and click on **Cluster Manager** on the left.
   
2. In the **Cluster Manager: Services** screen, click on the **teched-2025-europe** tile.

	**HINT:** If you cannot see the tile, uncheck the **"Only show my services"** checkbox.

	![Pic 2](./images/ex11-2.png)  

3. Switch to the **Manage** tab, and click on the **Queues** link button.

	![Pic 4](./images/ex1-4.png)     

4. A new browser tab or window will open, listing all the available Queues.
   <br>As mentioned earlier, each participant needs to create 2 queues and subscribe to the relevant topic.
   <br>In this exercise, we will create the first queue and subscribe to the given topic:
   
    - Queue Name: <b>IN162-***_Sales_Order</b> *(replace *** with the participant number that is assigned to you)*
   		- Topic Subscription: **"sap/teched/2025/ce/sap/s4/beh/salesorder/v1/SalesOrder/Created/v1"**

   Click the **+ Queue** button on the top right to create a queue.
   
	![Pic 5](./images/ex1-5.png)        

5. In the pop up, enter the queue name: <b>IN162-***_Sales_Order</b> *(replace *** with the participant number that is assigned to you)* and click on the **Create** button.

	![Pic 6](./images/ex1-6.png)

6. On the next screen, change the **Access Type** to **Non-Exclusive**, as this session does not require message order and exactly-once delivery. Leave the remaining configurations at their default settings, and click on the **Apply** button.
	> Note:
 	> <br>**Non-exclusive queue** → multiple consumers, parallel processing, no EOIO guarantee.
	> <br>**Exclusive queue** → single consumer, preserves EOIO (message order and exactly-once delivery).
	
	![Pic 7](./images/ex1-7.png)       

7. Select and open the queue that you have just created i.e., <b>IN162-***_Sales_Order</b> *(replace *** with the participant number that is assigned to you)*
   
   ![Pic 8](./images/ex1-8.png)
   
8. Switch to the **Subscriptions** tab and click on the **+ Subscription** button on the top right area to create a new topic subscription.

	![Pic 9](./images/ex1-9.png)

9. In the pop up, enter the topic: **"sap/teched/2025/ce/sap/s4/beh/salesorder/v1/SalesOrder/Created/v1"** and click on the **Create** button.

   ![Pic 10](./images/ex1-10.png)

10. Check whether your queue subscription has been created.

	![Pic 11](./images/ex1-11.png)


## Exercise 1.3 - Create second queue and subscribe to support case topic in SAP Integration Suite, advanced event mesh (AEM)
After completing these steps, you will have created the second queue subscribed to the support case topic published from the SAP Service Cloud Version 2 System in AEM.

1. Navigate back to Queues.

   ![Pic 13-1](./images/ex13-1.png) 

2. In this exercise, we will create the second queue and subscribe to the given topic:
   
	- Queue Name: <b>IN162-***_Support_Case</b> *(replace *** with the participant number that is assigned to you)*
   		- Topic Subscription: **"sap/teched/2025/servicecloud/supportcase/created"**
  	
	Click the **+ Queue** button on the top right to create a queue.
   
	![Pic 13-2](./images/ex13-2.png)

3. In the pop up, enter the queue name: <b>IN162-***_Support_Case</b> *(replace *** with the participant number that is assigned to you)* and click on the **Create** button.

	![Pic 13-3](./images/ex13-3.png)

4. On the next screen, change the **Access Type** to **Non-Exclusive**, as this session does not require message order and exactly-once delivery. Leave the remaining configurations at their default settings, and click on the **Apply** button.
	> Note:
 	> <br>**Non-exclusive queue** → multiple consumers, parallel processing, no EOIO guarantee.
	> <br>**Exclusive queue** → single consumer, preserves EOIO (message order and exactly-once delivery).

	![Pic 13-4](./images/ex13-4.png)       

5. Select and open the queue that you have just created i.e., <b>IN162-***_Support_Case</b> *(replace *** with the participant number that is assigned to you)*
   
   ![Pic 13-5](./images/ex13-5.png)
   
6. Switch to the **Subscriptions** tab and click on the **+ Subscription** button on the top right area to create a new topic subscription.

   ![Pic 13-6](./images/ex13-6.png)

7. In the pop up, enter the topic: **"sap/teched/2025/servicecloud/supportcase/created"** and click on the **Create** button.

   ![Pic 13-7](./images/ex13-7.png)

8. Check whether your queue subscription has been created.

   ![Pic 13-8](./images/ex13-8.png)

9. There are now two queues created for your participant number.

	![Pic 13-9](./images/ex1-queuesFinal.png) 

## Summary

At the end of the first exercise, you should have familiarized yourself with SAP Integration Suite, advanced event mesh (AEM), created 2 queues, and subscribed to the relevant topics.
Please make sure that you have used the right nomenclature and replaced <b>***</b> with the participant number that is assigned to you.

Please continue with [Exercise 2](../ex2/README.md)
