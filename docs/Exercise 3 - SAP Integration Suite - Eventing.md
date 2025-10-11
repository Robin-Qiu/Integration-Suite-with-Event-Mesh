# Overview


> **Objectives**
> 
> After completing this lesson, you will be able to:
> 
> familiarize yourself with the concepts of eventing.


# SAP Integration Suite - Eventing
## Script Overview
### Highlights
In this section, you will familiarize yourself with the concepts of eventing.

Events allow us to react to changes, by informing that something has changed.

In our scenario we will use event mesh to synchronise the quantity from Sales Orders and Maintenance Contracts.

During the course of this exercise, you will create a queue that will receive changed order events. Next, you will build an iflow to consume the changed order data and update the maintenance contract database.

![](vx_images/431526901621573.png)
### Prerequisites
* Your Session-User, provided via mail or by your instructor
* Access to the landing page of your Session-Account via link, provided via mail or by your instructor
### Goal
The goal of this section is to configure Event Mesh and SAP Cloud Integration to react on the changed quantity of a sales order in S/4HANA and update the related Maintenance Contracts database.

### Further information
[SAP BTP Event Mesh documentation](https://help.sap.com/viewer/bf82e6b26456494cbdd197057c09979f/Cloud/en-US/df532e8735eb4322b00bfc7e42f84e8d.html)
### Exercise map
In the following chapters of this exercise you will use SAP Event Mesh.


# Introduction to Eventing


> **Objectives**
> 
> After completing this lesson, you will be able to:
> 
> configure the SAP Event Mesh service to handle a Cloud event.

# Eventing – Event Mesh Queue
In this chapter, we will try to listen to the events created in the **S/4 HANA** On-Premise system, when we change an already created sales order from the previous exercise.

![](vx_images/277736839058017.png)
## Backend Setup
There is no need for the participants to do the backend setup as it is a one time activity that we already done in the setup.

Please find the details on the configuration in the following video.

**Please find the reference blogs that we used for the setup**

[Configure SAP Event Mesh for SAP S4HANA on premise via service key](https://blogs.sap.com/2021/02/15/configure-sap-enterprise-messaging-for-sap-s-4-hana-on-premise-2020-via-service-key/)

[Publishing events from S4HANA to Event Mesh for Cloud Integration for external systems](https://blogs.sap.com/2021/10/28/publishing-events-from-s-4-hana-to-event-mesh-for-cloud-integration-for-external-systems/)

[Enable SAP Cloud Integration to consume messages from Event Mesh](https://blogs.sap.com/2022/06/14/blog-post-enable-sap-cloud-integration-suite-to-consume-messages-from-sap-event-mesh-service/)

## Access the Event Mesh
1. Follow link for the [SAP Event Mesh](https://integration-suite-academy-us10.enterprise-messaging.cfapps.us10.hana.ondemand.com/) service
## Configure Event Mesh Queue
1. Go to **Message Clients**

1. Select Message client named **ems**

    ![](vx_images/263536669015632.png)

1. Click **Queues => Create Queue**

    ![](vx_images/84475592968316.png)

1. In Queue Name: **AC233083U01** and select **Create** button to create queue.

![](vx_images/489114504401499.png)
## Subscribe to a Topic in Event Mesh
1. Click on the action button and select **Queue Subscriptions**

    ![](vx_images/351796659172587.png)

1. Enter Topic : sap/vr/A/s4/ta/**AC233083U01**/*

    ![](vx_images/212225872973890.png)
Select **Add**.

1. Select **Close**

![](vx_images/234438617045127.png)

## Testing Queue
We will test our created queue by changing the order quantity which was created in previous exercises (Chapter 3 exercise 3). Queue will listen to changes made to your sales order details.

1. Click on: S/4 HANA to open the SAP Fiori Launch Pad

1. Login to System using

    * Your Login user: AC233083U01 and
    * Your password, provided via mail or by your instructor
![](vx_images/133706633493867.png)
1. Select tile **Change Sales Order** (VA02)

    ![](vx_images/524377946329195.png)

1. Enter the **Sales Order** number which you got in the Process Integration exercise (Unit 3, Lesson 3) using the Postman tool and select **Continue**.

    ![](vx_images/316828369147614.png)
1. Match your order reference number and change the **quantity** and then do **save**.

    ![](vx_images/61277055907515.png)
1. Open **Event Mesh** and select your **Queue** by searching your userid. You can see now 2 messages in your Queue. One message is notification of change and other contains changed quantity data.

    ![](vx_images/481716914861063.png)
1. Go to **Test** tab to see the data.

    ![](vx_images/322169142040746.png)
1. Select your **Queue** in **Consume Message** part. Now select **Consume Message**

    ![](vx_images/561786303325362.png)
1. First notification message.

    ![](vx_images/332516990587059.png)

1. Again click on **Consume Message** to see Data.

![](vx_images/213279190277246.png)


# Consume Events in Cloud Integration


> **Objectives**
> 
> After completing this lesson, you will be able to:
> 
> consume events and update maintenance contracts.


# Eventing – Integration Flows
In this chapter we will learn to update the Maintenance Contract database in **PostgreSQL** using the updated quantity in **S/4 HANA** with help of **SAP Event Mesh** and **SAP Cloud Integration**. As the conclusion from chapter 1 we found out that we can use data event to get the needed information to update the maintenance contract. So we need to unsubscribe the old topic and subscribe to only data topic.

![](vx_images/523167316526355.png)
## Configure Event Mesh Queue
1. Go to **Message Clients**

1. Select Message client named **ems**

    ![](vx_images/338928785439356.png)
1. Select already created Queue Name : **sap/vr/A/AC233083U01** .

1. Select **Queue Subscriptions** from action button.

    ![](vx_images/559587377982711.png)
1. **Delete** topic `sap/vr/A/s4/ta/AC233083U01/*` from the subscription.

    ![](vx_images/77799760738914.png)

1. Enter Topic : `sap/vr/A/s4/ta/AC233083U01/data`

    ![](vx_images/432348451173929.png)
1. Select **Add**.

1. Select **Close**

![](vx_images/245467593960801.png)
Switch back to the SAP Integration Suite - **Integration and APIs** window, or access it via [SAP Integration Suite](https://integration-suite-academy-us10.integrationsuite.cfapps.us10-002.hana.ondemand.com/shell/home) and navigate to the **Design** section in the left-hand menu.

![](vx_images/405731501497280.png)
## Create the integration flow
When you log into Cloud Integration, you can discover and download predefined integration packages. These packages contain artifacts like integration flows that implement specific use cases. This content is delivered and supported by SAP (or partners) and can be used to quickly implement specific integration projects.

1. Please browse to existing integration packages **Orders_AC233083U01** created in the Proceess Integration exercise (Lesson 3).

    ![](vx_images/223374005719746.png)
1. In the Cloud Integration service, within your package, click on the **Artifacts** tab and then select **Edit** button to make changes.

    ![](vx_images/182071047131181.png)
    > **Note** : If you don’t want to create iflow and want to import it then please scroll down to the section **Import Iflow**.

1. Make sure that you are in **Edit** mode (**Save** is displayed instead of **Edit**) of the package and click on **Add** and select **Integration flow**.

    ![](vx_images/216851588275179.png)
1. Fill in the fields as follows:

    * **Name**: **Update_Maintenance_Contract_AC233083U01**

    * **ID** will be auto-filled with **Update_Maintenance_Contract_AC233083U01**

    * **Description**: “This integration flow will process(update) Sales orders”

    * The fields **Sender** and **Receiver** are not relevant for now.

    * Click on **Add**

    ![](vx_images/355810847842213.png)
1. Your integration flow has correctly been created, and you can now access it by clicking on the row containing the artifact.

![](vx_images/21370727670989.png)
## Develop the integration flow
You can see that the Cloud Integration service has prebuilt parts of the integration flow, including Sender, Receiver, Start and End flow steps.

1. Click on the **Edit** button to start making changes to your integration flow.

    ![](vx_images/350793113129829.png)

1. Click on the **Sender** box and rename it to **Event_Mesh**.

    ![](vx_images/210534654698491.png)
1. Click and drag the arrow symbol to the **Start** flow step.

    ![](vx_images/15314540693742.png)
1. In the pop-up for the Adapter Type, chose **AMQP**.

    ![](vx_images/445564759186841.png)
    * Choose **WebSocket** as **Transport Protocol**.
    * In the integration flow editor, you can now configure the adapter.

1. Define the **Connection Details** as follows:

    * **Host**: enterprise-messaging-messaging-gateway.cfapps.us10.hana.ondemand.com

    * **Port**: 443
 
    * **Path**: /protocols/amqp10ws
 
    * **Proxy Type**: Internet
 
    * **Connect with TLS**: checked
 
    * **Authentication**: OAuth2 Client Credentials
 
    * **Credential Name**: ems_ta

    ![](vx_images/4882903187469.png)
    
1. Define the **Processing** details as follows:

    * **Queue Name**: queue:sap/vr/A/AC233083U01
 
    * **Number of Concurrent Processes**: 1
 
    * **Max. Number of Prefetched Messages**: 5
 
    * **Consume Expired Messages**: checked
 
    * **Max. Number of Retries**: 0
 
    * **Delivery Status After Max. Retries**: REJECTED

    ![](vx_images/462233707883188.png)

1. Select **Save** to persist your changes.

## Enhance integration flow to write to PostgreSQL
We will now proceed to create the request for updating the data into the database. To do so, we will create a SQL-statement using the incoming payload. This will include 2 steps: transforming the incoming JSON data structure into XML and building the SQL statement for the database.

1. In the top menu bar of the integration flow editor, select the **Message Transformers** item and select **Converter** item. Under Converter select the **JSON to XML converter** then drop the JSON to XML converter on the link between the **Start** and **End** flow steps (it highlights green).

    ![](vx_images/27832629238824.png)
    ![](vx_images/198562996984575.png)
    Check that the converter step is placed on the link between Start and End by dragging it around for instance.

    Your integration flow should look like this:

    ![](vx_images/216863837408142.png)

1. In the top menu bar of the integration flow editor, select the **Message Transformers** item and select the **Content Modifier**.

    ![](vx_images/478852557497355.png)

1. Drop the Content Modifier on the link between the **Converter** and **End** flow steps. Check that the content modifier step is placed on the link between Start and End by dragging it around for instance.

    * Rearrange your integration flow so that it stays clean and readable, using the handles of the various items, as well as dragging the items around.

    ![](vx_images/481254225101532.gif)
    * Your integration flow should look like this:

    ![](vx_images/214465566552050.png)

1. Click on **Save** in the editor before moving on to the next step.

    * The content modifier is a mighty flow step that lets you interact with the payload and metadata going through the integration flow.
    > **Note**: you can read the documentation of the flow steps by using the **question mark** symbol on the right of the configuration pane.

1. Click on the **Content Modifier** (if it is not active), open-up the configuration pane (if it’s hidden), click on **Exchange Property** and click on **Add**.

    * We will now read information out of the payload and store it for later use in the integration flow.
    * Remember that we have converted the incoming payload to XML so we can now use XPath expressions in the content modifier to read the payload elements. Once the payload elements are stored in exchange properties, we can reference them in any other part of our integration flow. In our example, we will build a dynamic SQL query.

1. Create the following Exchange Property.

    * OrderReference

        1. Action: Create
        1. Name: OrderReference
        1. Type: XPath
        1. Data Type: java.lang.String
        1. Source Value: //OrderReference

    * Quantity

        1. Action: Create
        1. Name: Quantity
        1. Type: XPath
        1. Data Type: java.lang.String
        1. Source Value: //Quantity

    This is what your content modifier should look like:

   ![](vx_images/296605748201725.png)

1. Click on **Message Body** in your content modifier.

    * The message body allows to alter the payload of the integration flow.
    * Set the **Type** to **Expression**
     
1. Enter the following text as body:

```
UPDATE technicalacademy.maintenance_contracts
SET quantity='${property.Quantity}'
WHERE order_reference ='${property.OrderReference}';
```
![](vx_images/90606384346034.png)
    * As you can see, we are dynamically creating the UPDATE statement.
    * Now that the payload is properly formatted, we need to send it to the PostgreSQL database.
    
9. Click on the **End** flow step and drag the arrow on top of the **Receiver** box.

    ![](vx_images/221863737146445.png)

1. In the **Adapter Type** pop-up window, chose **JDBC**.

    ![](vx_images/40812569136455.png)

1. In the JDBC adapter configuration, click on **Connection** and fill-in the configuration as follows:

    * JDBC Data Source Alias: **AC_PostgreSQL_Cloud**

    ![](vx_images/146923426827194.png)
1. Save your integration flow.
1. Deploy your integration flow
## Import Iflow
> **Note**: In case you did not have time to go through all steps, you can also import the integration flow from the Files folder.

 1. Navigate to your Integration Package **Orders_AC233083U01**.

1. Click on **Edit**

1. Click on **Add** and select **Integration Flow** >

    ![](vx_images/468974148085050.png)
1. Click on **Upload**, then select the file through the **Browse** button, and **rename** your integration flow to **Update_Maintenance_Contract_AC233083U01**. In case you already have an integration flow with the same name, rename the integration flow with something like **Update_Maintenance_Contract_AC233083U01_2**.

    ![](vx_images/70095942650723.png)
1. Open the integration flow

1. Define the **Connection Details** as follows:

    * **Host**: (enterprise-messaging-messaging-gateway.cfapps.us10.hana.ondemand.com)
    * **Port**: 443
    * **Path**: /protocols/amqp10ws
    * **Proxy Type**: Internet
    * **Connect with TLS**: checked
    * **Authentication**: OAuth2 Client Credentials
    * **Credential Name**: ems_ta
    ![](vx_images/443663595392826.png)

1. Define the **Processing Details**: as follows:

    * **Queue Name**: queue:sap/vr/A/AC233083U01
    * **Number of Concurrent Processes**: 1
    * **Max. Number of Prefetched Messages**: 5
    * **Consume Expired Messages**: checked
    * **Max. Number of Retries**: 0
    * **Delivery Status After Max. Retries**: REJECTED
    ![](vx_images/49376102080143.png)
1. Save and Deploy.

## Testing
We will test our created queue by changing the order quantity which was created in previous exercises (Chapter 3 exercise 3). Queue will listen to changes made to your sales order details.

1. Click on: S/4 HANA to open the SAP Fiori Launch Pad

1. Login to System using

    * Your Login user: AC233083U01 and
 
    * Your password, provided via mail or by your instructor

    ![](vx_images/313205761609270.png)
1. Select tile **Change Sales Order** (VA02)

    ![](vx_images/32947969899613.png)

1. Enter the **Sales Order** number which you got in the Process Integration exercise (Unit 3, Lesson 3) using the Postman tool and select **Continue**.

    ![](vx_images/503788333193412.png)

1. Match your order refrence number and change the **quantity** and then do **save**.

![](vx_images/316666441279339.png)

1. Check the quantity update in **PostgreSQL Maintenance Contract Database** using UI exposing its data. UI_Link. Filter by your **Order Reference** Number.

![](vx_images/389335788762203.png)
> **Note** : Please try to reopen the link if you can’t get the new record with an updated quantity.

<br>

**Congratulations!**
You have successfully finished this hands-on workshop!