# <font color=#ee9955 size=8 face="黑体"> Exercise 00 - Smoke Test Monitoring via WebUI</font>

## Prerequisites  
 - **SAP BTP Account:** You should have created the BTP account and set up the Integration Suite tenant.



---

## Details

##### Follow these steps for integration flow creation in Web UI.
1 . Familiarize yourself with the environment and choose the **“Design”** area. Here, you will create an Integration Package to store your iFlows for different Exercises
In the left-hand navigation, you can switch between the following sections of the Web application:
-	**Discover** → SAP’s Reference Catalog
-	**Design** → Your customer Workspace (Design Time Content)
-	**Monitor** → Message Monitoring and deployed integration flows
![](vx_images/183295087352389.png)

2. Click on **Create** on the right most corner
Enter Details 

    * a)	**Name**: SAP BTP Integration Suite Workshop (Exercises) Group_XX
        
         > **NOTE**: Replace XX with the group number provided by the instructor
 
    * b)	**Short Description**: SAP BTP Adoption Lab, Integration Suite
 
    * c)	**Version**: 1.0.0
 
    * d)	**Vendor**: A&C
![](vx_images/94036974223759.png)


3. Click on **Save**


4. Navigate to **ARTIFACTS** Tab
![](vx_images/395477367718342.png)

5. Click on **Add-> Integration Flow**
![](vx_images/513907790848570.png)

6. Select **Create** and enter following details:

    * a.	Name: **Exercise00_XX**
 
    * b.	Description:  **Exercise00_SAP Cloud Integration_Smoke_Test_Monitoring_via_WebUI**
 
    * c.	Click on **Add**

> **Note**: Please remember to save after every action

![](vx_images/97447806280526.png)

7. 	Click on Exercise00_XX
![](vx_images/457448086784083.png)

##### Define and edit integration flow
1. 	Click on **EDIT** button on the lower corner on top right corner side.
![](vx_images/457576320353142.png)

2.	Click on Sender and select **Delete**

![](vx_images/248610262889570.png)

Similarly, click on Start Message, select **Delete**

![](vx_images/239780978874887.png)

3. 	Integration Flow should appear like this
![](vx_images/35482833664160.png)

4.	Now, let's model the integration flow

    Select **Timer** from Palette
    **Events -> Timer**

    Place it in the integration flow by clicking inside the integration process

![](vx_images/447252934989392.png)

![](vx_images/284191036820720.png)

5.	Select **Timer** and maintain schedule. 

    Choose **Basic** and use the default setting

    > (This would ensure that iFlow is executed as soon as it is deployed)

![](vx_images/598192725892409.png)

6. Connect **Timer to End Message** event
![](vx_images/333641112554413.png)

6. 	From the palette, select **Message Transformers -> Content Modifier** and drop it on the connection between **Timer** and **End messag**e in the Integration flow. This would automatically create the connections. Place it in the integration flow by clicking inside the integration process
![](vx_images/43743444448199.png)
7. Switch to **Message Body** tab, paste the content given below:
    ```xml
    <ndf:NDFDgenByDay xmlns:ndf="https://graphical.weather.gov/xml/DWMLgen/wsdl/ndfdXML.wsdl">
                <latitude>35.4</latitude>
                <longitude>-97.6</longitude>
                <startDate>2025-10-15</startDate>
                <numDays>1</numDays>
                <Unit>m</Unit>
                <XMLformat >DWML</XMLformat>
                <format>24 hourly</format>
    </ndf:NDFDgenByDay>
    ```
    Click **Save**
    > **Note**: This latitude and longitude is of **Oklahoma City**.

![](vx_images/154743902089733.png)

8.  From the palette, select **Call -> External Call -> Request Reply** and drop it on the connection between **Content Modifier** and **End message** in the Integration flow.
![](vx_images/165364034115223.gif)

9. Click on **Request Reply** step, select **Connector** and drag it to **Receiver** system. (You might drag Receiver under Request-Reply for better readability)
This would open the receiver adapter list.
Select **SOAP** adapter

    Select **SOAP 1.x** as Message Protocol
![](vx_images/319645416684934.gif)

10. Switch to **Connection** Tab, enter the following details:
    * a. Address: **https://graphical.weather.gov/xml/SOAP_server/ndfdXMLserver.php**
    * b. Select Authentication as **“None”**
    Keep all other parameters as it is.


    

    Click **Save**
    > **Note**: In case, this service is not available we need to change the service endpoint to mocked weather service which is deployed on the Integration Suite tenant for you. Also, you need to change the Authentication to Basic
    Instructor will provide you the SOAP Endpoint of mocked weather service.

![](vx_images/520153239720182.png)
11.  Click on Receiver system and set Name as **Global_Weather**

![](vx_images/282866627224173.png)

12. From the palette, select **Message Transformers -> Content Modifier **and drop it on the connection between **Request Reply** and **End** message in the integration flow.

![](vx_images/545925594563375.png)

Switch to **Exchange Property** tab, click on **Add** and add following properties:

* Action: **Create**
* Name: **responseResult**
* Type: **XPath**
* Data Type: **java.lang.String**
* Value: **//\* [local-name()='XMLByDayOut']/text()**

Click **Save**

![](vx_images/450414261491325.png)


13. Select **Router** from the palette, drag and drop it on the connection between **Content Modifier** and **End** message
![](vx_images/250186589947013.gif)

14. From the palette, select **Persistence > Data Store Operations > Write** and drop it on the connection between **Router** and **End** message in the integration flow.

![](vx_images/308415897306985.gif)

Provide the following properties:
* a. Data Store Name:
**WeatherData_XX**
* b. Entry ID: **Oklahoma City**
* c. Overwrite Existing Message: **Checked**
> **Note**: Replace XX with your Group ID
Save and ignore all the errors.

![](vx_images/474929170284856.png)


15. From the palette, select **Script > Groovy Script** and drop it on the connection between **Write** data store flow step and **End** message in the integration flow.

![](vx_images/75147032302555.gif)

Click on **Create** icon

![](vx_images/388645800734384.png)

Replace the script default code with the following code:
```java
import com.sap.gateway.ip.core.customdev.util.Message;
import java.util.HashMap;
def Message processData(Message message) {
def body = message.getBody(java.lang.String) as String;
def messageLog = messageLogFactory.getMessageLog(message);
if(messageLog != null){
messageLog.addAttachmentAsString("Log Current Payload:", body, "text/plain");
}
return message;
}
```

Click on **Apply**
> Note: Please click **Close** to ignore the warning message.

![](vx_images/437787128320508.png)


16. From the palette, add another **Groovy Script**
Place it as shown in the integration flow

Click on **Create** icon

![](vx_images/334864565889268.png)

Replace the script default code with the following code:
```java
import com.sap.gateway.ip.core.customdev.util.Message;
import java.util.HashMap;
def Message processData(Message message) {
def messageLog = messageLogFactory.getMessageLog(message);
if(messageLog != null){
messageLog.addAttachmentAsString("Log Current Payload:", "No valid response found from the webservice", "text/plain");
}
return message;
}
```

Click on **Apply**
> Note: Please click **Close** to ignore the warning message.

![](vx_images/169913778801359.png)

17. From the palette, Choose **End Message** and place it after the second Groovy Script.
![](vx_images/64491887264577.gif)


18. Connect **Router** to the **Groovy Script**
![](vx_images/494062622008144.png)
19. Connect **Groovy Script** to the **End 1** Message
![](vx_images/309885013035098.png)


20. Select connection between **Router** and **Write** step and configure the route with following values:
* a. Name: **Success**
* b. Expression Type: **Non-XML**
* c. Condition: **${property.responseResult} != ''**

![](vx_images/133714820586868.png)

![](vx_images/512314432605978.png)

21. Select connection between **Router** and **Groovy Script** step and configure the route with following values:
* a. Name: **No Response**
* b. Default Route: **Checked**

![](vx_images/132616542106223.png)
![](vx_images/576165549180352.png)

22. **Save** your changes
![](vx_images/416416241048354.png)

##### Deploy Integration Project on tenant
1. Press **Deploy** in the Integration Flow Task Bar.

![](vx_images/311385139944047.png)

2. You will receive a confirmation. Click on **Yes**.
Once the deployment is completed successfully you will receive a 2^nd^ notification.

![](vx_images/368025218475473.png)
![](vx_images/90613521649697.png)


3. Verify if the deployment is successful:
In the first level Menu Bar switch to Section **Monitor** and then click on **All** in **Integration and APIs**.
![](vx_images/7022920526518.png)

1. You should see the message status and log payload

    ![](vx_images/399225466662663.png)

   ![](vx_images/102285962295884.png)



1. Navigate back to the to the **Monitor** page and check the data store

    ![](vx_images/395696472944019.png)

1. There should be one weather record stored 
![](vx_images/142396552151981.png)

<br>

**Congratulations!**
You have successfully finished this hands-on workshop!