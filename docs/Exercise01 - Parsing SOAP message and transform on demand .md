# <font color=#ee9955 size=8 face="黑体"> Exercise 01 - Parsing SOAP message and transform on demand </font>



## Details

##### Define and edit integration flow
1. Navigate to **Artifacts** Tab
![](vx_images/204035469315867.png)

2. Click on **Edit** and choose  **Add-> Integration Flow**

 ![](vx_images/288384051359984.png)
![](vx_images/141201755614541.png)

3. Select **Create** and enter the following details:
    a. Name: **Exercise01_XX**
    b. Description: **Exercise01_WebUI_SOAP_Mail_MessageTransformers_Router_Filter_TestMapping_Converter_ContentModifier**
    c. Sender: **SOAP**
    d. Receiver: **Mail**
    e. Click on **OK**
**Note**: Please remember to save after every action
![](vx_images/289942763578925.png)

4. Click on **Exercise01_XX**
![](vx_images/26694045647033.png)

5. Click on **Edit** button on the upper corner in the right-hand side.
![](vx_images/178402805864430.png)

6. Click on Sender system and set Name as **IT_WebPortal**
![](vx_images/254194137815173.png)

7. Click on Receiver system and set Name as **Individual_Mail**
![](vx_images/331504241398332.png)

8. Click on **IT_WebPortal** sender system, select **Connector** and drag it to **Start** message step.
    This would open the sender adapter list.
    Select **SOAP** adapter
    Select **SOAP 1.x** as Message Protocol
![2025-12-03_18-26-52 (1)](vx_images/275625245375707.gif)

9. Switch to **Connection** tab, enter the following details:
    a. Address: **/Exercise01_XX** (make sure this address is unique on tenant)
    b. Service Definition as
    **WSDL**
    c. URL to WSDL, click on **Select**
    d. Click on **Upload from File System**
    e. Select **userdata_wp.wsdl** (provided by instructor)
    f. Processing Settings as **WS Standard**
    g. Authorization as **User Role**
    **Save**
![](vx_images/488825766856933.png)

10. Click on the empty space near Integration scenario, the following namespace mapping is added automatically in **Runtime Configuration**
**xmlns:p1=http://sapcd.com/taxagt**
![](vx_images/352947962590657.png)

11. Click on **End** message step, select **Connector** and drag it to **Individual_Mail** receiver system.
    This would open the receiver adapter list.
    Select **Mail** adapter
![2025-12-03_18-46-54 (1)](vx_images/199688266268690.gif)

12. Switch to **Connection** tab, enter the following details: 
    a. Address: **smtp.gmail.com:465**
    b. Protection:    **SMTPS**
    c. Authentication: **Plain User/Password**
    d. Credential Name:    **GMAIL_USER**
    e. From: Mail ID from where you want to send the mail.
    (In exercise we use: developer.qiu@gmail.com)
    f. To: Mail ID where you want to receive the mail.
    (eg. testto@gmail.com)
    g. Subject: **Income Tax Details for PAN5678**
    h. Under Attachments, click on **Add**, enter:
        1) Name: **Income_Tax_Details.csv**
        2) Mime-Type: **Text/CSV**
        3) Source: **Body**
    **Save**
   
![SNAGHTML6b6bf83](_v_images/20190829155125738_12175.png) 
![](vx_images/479883180914812.png)

![](vx_images/7387558207825.png)

13. (Optional) In **Monitor** tab, click on **Security Material** tab to deploy your Gmail account username and password using which you want to send an email. Then:
    a. Click in **Add -> User Credentials**
    b. Enter name as **GMAIL_USER** and your gmail user and password.
    c. **Deploy** the artifact.
![2025-12-03_19-18-15 (1)](vx_images/228725647400023.gif)

    d. Click on **Connectivity Tests**
    e. Open tab **SMTP**
    f. Host: **smtp.gmail.com**
    g. Port: **465 (SMTPS)**
    h. Authentication: **Plain User/Password**
    i. Credential Name: **GMAIL_USER**
    j. Click **Send**

    ![2025-12-03_19-22-37 (1)](vx_images/268885804096220.gif)

    k. Click **Download** and unzip the file
    l. Click on **Keystore**
    m. Click in **Add -> Certificate**
    n. Browse the certificate files and Deploy one by one 


![2025-12-03_19-25-20 (1)](vx_images/542285770674967.gif)


14. From the palette, select **Message Transformers -> Content Modifier** and drop it on the connection between **Start** message and **End** message in the Integration flow. This would automatically create the connections.
Switch to **Exchange Property** tab, click on **Add** and add following properties:
a. Action: **Create**
    Name: **Account_Type**
    Type: **XPath**
    Data Type: **java.lang.String**
    Value: **//Account_Type**
b. Action: **Create**
    Name: **PAN**
    Type: **XPath**
    Data Type: **java.lang.String**
    Value: **//TaxID**
![2025-12-03_19-30-36 (1)](vx_images/37684467045415.gif)
![](vx_images/235366395816546.png)

15. From the palette, select **Message Routing -> Router** and drop it on the connection between **Content Modifier** and **End** message in the integration flow.
![2025-12-03_19-34-38 (1)](vx_images/322266205985959.gif)

16. Select connection between **Router** and **End Messag**e and configure the route with following values:
a. Name: **Individual**
b. Expression Type: **Non-XML**
c. Condition:
**${property.Account_Type} = 'Individual'**
![](vx_images/270746551098597.png)



17. Click on **Save**.
    Ignore the errors on Integration Flow after save
![](vx_images/147547866750293.png)

18. Click on **Participant** and add one more **Receiver** to the integration flow.
Select the newly added receiver and set the name as **Insurance_Dept**
![2025-12-03_19-39-18 (1)](vx_images/467186962589010.gif)

19. From palette, select **Message Transformers -> Converter**
    Click on **XML to JSON** Converter
    Add it to the Integration Flow
![2025-12-03_19-41-19 (1)](vx_images/25374895535620.gif)

20. Connect **Router** to **XML to JSON Converter**
![2025-12-03_19-42-55 (1)](vx_images/385027062703704.gif)

21. Configure **Router** condition, click on Connection:
    a. Name: **Department_I**
    b. Expression Type: **Non-XML**
    c. Condition: **${property.Account_Type} = 'Department_I'**
![](vx_images/361187030870050.png)
![](vx_images/149468766844375.png)

22. Click **Events**, select **End Message** and drop it on the integration flow
    Connect **XML to JSON Converter** to **End** Message
    Click on **Save**
![2025-12-03_19-47-27 (1)](vx_images/583605203299198.gif)

23. Connect **End** Message to **Insurance_Dept** Receiver
    This would open the receiver adapter list.
    Select **Mail** adapter
![2025-12-03_19-49-13 (1)](vx_images/476508332839843.gif)

24. Switch to **Connection** tab, enter the following details:
    a. Address: **smtp.gmail.com:465**
    b. Protection:    **SMTPS**
    c. Authentication: **Plain User/Password**
    d. Credential Name:    **GMAIL_USER**
    e. From: Mail ID from where you want to send the mail.
    (In exercise, we use: developer.qiu@gmail.com)
    f. To: Mail ID where you want to receive the mail.
    (eg. testto@gmail.com)
    g. Subject: **Income Tax Data**
    **Save**
![](vx_images/292505723822302.png)
![](vx_images/50867055132065.png)

25. From the palette, Choose **Terminate Message**
![2025-12-03_19-57-51 (1)](vx_images/145697780787748.gif)

26. Connect **Router** to the **Terminate Message**
![2025-12-03_19-59-41 (1)](vx_images/37233334713153.gif)

27. Configure **Router** condition, click on Connection:
    a. Name: **Other Department**
    b. Check **Default Route**
    **Note**: There is an error in Router (red cross) as it is mandatory to have a Default route. This error would disappear when we set this route as Default and Save
![](vx_images/432551041286881.png)
![](vx_images/211072381348382.png)

28. Now we would work on the first receiver (Mail to individuals), so select the connection between **Router** and **End** message connected to **Mail** adapter
From the palette on the left side, select **Message Transformers -> Filter** and drop it on the connection between Router and End message in the integration flow
Enter the following details:
    a. XPath Expression: **/tax:IncomeTaxMessage/UserData[TaxID='PAN5678']**
    (You can try to change the value marked in red and take it from another UserData record)
    b. Value Type: **Nodelist**
![2025-12-03_20-04-36 (1)](vx_images/594094283136938.gif)
![](vx_images/87101719306070.png)


29. Namespace **xmlns:tax **needs to be added to the Runtime configuration.
    Click on the empty area as shown
    Navigate to Runtime Configuration
    Copy and paste the below details
   ```
   xmlns:p1=http://sapcd.com/taxagt;xmln`s:tax=https://taxschemas.netweaver.neo.com/taxflow
   ```
   

![](vx_images/272771768322761.png)

30. Add **Content Modifier** after **Filter**
from the palette, select **Message Transformers -> Content Modifier**
Switch to **Message Body** tab and enter the following:
    ```
    <tax:IncomeTaxMessage xmlns:tax="https://taxschemas.netweaver.neo.com/taxflow">${in.body}</tax:IncomeTaxMessage>
    ```
    Save
    **Note**: This is used to add envelope to XML as output of filter will not be a well-formed XML
    **Explanation**: As pension department needed Income Tax Message as root element of the payload, we are using content modifier to add this root node to the payload sent by IT department
![2025-12-03_20-09-40 (1)](vx_images/529672092539415.gif)
![](vx_images/109711661578094.png)

31. From the palette, select **Mapping -> Message Mapping** and drop it on the connection between **Content Modifier** and **End** message in the integration flow
![2025-12-03_20-27-28 (1)](vx_images/126934396856353.gif)

32. Click on **Create** icon .
    **Create Message Mapping** dialog will open, enter the Name of your mapping.
    E.g. **MM_TaxDetails_WebPortal**
    Click on **Create** button
    Mapping editor screen will open as shown in the screenshot.
    Click on A**dd source message**, then select the **userdata_wp.wsdl** for source message.
    Click on Add Target message, then click on **Upload from File System**.
    Select **userdata.wsdl** (provided by instructor)

![](vx_images/81004703183879.png)
![](vx_images/39376599930027.png)
![](vx_images/262976092917521.png)
![](vx_images/299138083629183.png)



33. Drag and drop the source elements to the target elements and the overall mapping should look like as shown in figure.
    Map all the fields 1:1, except **userID**, **FullName**, **Street and City**
![](vx_images/191571593256758.png)

34. Message Mapping: **userID**
    a. Hover on **userID** and click on quick action icon
    b. Click on **Assign Constant**
    c. Click on **+ Assign**
    d. Click on **Upload from File system**
    e. Select **UserDefinedFunctions.gsh** (from Artifacts folder). With this, it should appear under **Functions -> Custom**
    f. Click on **UserDefinedFunctions**. Click on **generateID**
    g. This would make it available in Mapping editor
    h. Delete connection between **constant** and **userID** and insert **generateID** in between
![](vx_images/107364391406477.png)
![](vx_images/365131430834622.png)

![2025-12-04_00-13-04 (1)](vx_images/443465232338922.gif)


35. Message Mapping: **FullName**
    a. Drag and drop **FirstName** to **FullName**
    b. Drag and drop **LastName** to **FullName**
    c. Delete the connection between **FirstName** and **FullName** (if it is there)
    d. Select **Text** from Mapping Expressions
    e. Select **concat**
    f. Enter **space** as Delimiter String
    g. Connect **FirstName** and **LastName** to **concat**
    h. Connect **concat** to **FullName**
![2025-12-04_00-24-01 (1)](vx_images/357282527148956.gif)



36. Message Mapping: **Street & City**
    a. Map **Address** to **Street**
    b. Click on **UserDefinedFunctions**. Click on **mapAddressLine**
    This would make it available in Mapping editor
    c. Click on **Constants** and select **copyValue (Index: 0)**
    d. Create connections as shown in screenshot. Mapping should look like this
    **Repeat the steps for City, but with copyValue (Index: 1)**

![2025-12-04_00-34-39 (1)](vx_images/407754522246685.gif)
![2025-12-04_00-37-55 (1)](vx_images/285164739709385.gif)

37. Click on **Simulate** button at the top right corner to test the mapping.
![](vx_images/77598169534876.png)

38. Click on **Upload Input** button at bottom right corner to test the mapping.
    a. Select the **“TaxInputSimulate.xml”** file provided by the instructor and click on **Open**.
    b. File is uploaded as test data as seen in the screenshot.
    c. Click on **Test** button at bottom right corner and click on Test
![](vx_images/163708346328338.png)

39. The output of the mapping is shown in the simulation screen after successful execution
![](vx_images/544994994898949.png)

40. Click on the **Close** button to move back to the mapping environment.
![](vx_images/67625980020742.png)

41. Click on the **OK** button to save the mapping changes
![](vx_images/258797001561218.png)

42. Select Mapping, give your mapping a meaningful name as shown in the screenshot.
    Eg. **MM_TaxDetails_WebPortal**
    Click on **Save** to save the changes
![](vx_images/334147526616755.png)


43. From the palette on the left side, select **Message Transformers -> Converter -> XML to CSV** converter and drop it on the connection between **Content Message Mapping** and **End** message in the integration flow.
![2025-12-04_00-47-48 (1)](vx_images/218347615250186.gif)

> 44. Change the **Namespace Mapping** in **Runtime Configuration**: 

```
xmlns:p1=http://sapcd.com/taxagt;xmlns:tax=https://taxschemas.netweaver.neo.com/taxflow;xmlns:ns0=http://sap.com/MergeMultiplePOs
```

![](vx_images/383623164973415.png)

44. Select the **XML to CSV Converter** step in the process window and configure the properties as mentioned below
    Path to Source Element in XSD: **ns1:TaxDetails/UserData**
    Field Separator in CSV:
    Select **Comma (,)** as field separator from the Drop Down.
    Click on **Save** to save the changes
![](vx_images/19247499301604.png)

45. Your integration flow should look like this.
![SNAGHTML846030f](_v_images/20190829230731655_129.png)

##### Deploy Integration Project on tenant
1. Press **Deploy** in the Integration Flow Task Bar.
![](vx_images/576711437302749.png)

2. You will receive a confirmation. Click on **Yes**.
    Once the deployment is completed successfully you will receive a 2nd notification.
![](vx_images/430542808096864.png)

![](vx_images/86811744207542.png)

3. Verify if the deployment is successful:
    In the first level Menu Bar switch to Section **Monitor** and then click on **All** in **Manage Integration Content**.
    You should see an entry with your integration flow.
    Check the ‘status’. It should be in status **Started**.
    Click into it
![](vx_images/269173557622827.png)
![](vx_images/485414535275618.png)

##### Execute end to end scenario
1. Open your **Postman** and import the **IS Exercises** collecion
    a. Open **Exercise01** request
![](vx_images/510345442935456.png)

5. Click **Body** tab  
    a. choose **raw**
    b. Input the code as this:
    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tax="https://taxschemas.netweaver.neo.com/taxflow">
    <soapenv:Header/>
    <soapenv:Body>
        <tax:IncomeTaxMessage>
            <!--1 or more repetitions:-->
            <UserData>
                <!--Zero or more repetitions:-->
                <TaxID>PAN5678</TaxID>
                <Account_Type>Individual</Account_Type>
	        <!--Zero or more repetitions:-->
                <Currency>EUR</Currency>
                <!--Zero or more repetitions:-->
                <EOM>true</EOM>
                <!--Zero or more repetitions:-->
                <Annual_Income>80000</Annual_Income>
                <!--Zero or more repetitions:-->
                <Tax_Percent>40</Tax_Percent>
                <!--Zero or more repetitions:-->
                <Tax_Amount>32000</Tax_Amount>
                <!--Zero or more repetitions:-->
                <FirstName>John</FirstName>
                <!--Zero or more repetitions:-->
                <LastName>Doe</LastName>
                <!--Zero or more repetitions:-->
                <Department>ABC</Department>
                <!--Zero or more repetitions:-->
                <Address>Dietmar-Hopp-Allee 16, 69190 Walldorf</Address>
                <!--Zero or more repetitions:-->
                <EMail>john.doe@abc.com</EMail>
                <!--Zero or more repetitions:-->
                <PhoneNumber>+49 123456789</PhoneNumber>
            </UserData>
        </tax:IncomeTaxMessage>
    </soapenv:Body>
    </soapenv:Envelope>
    ```
![SNAGHTMLae404d9](_v_images/20190830111920691_1338.png)
c. Click **Send**
    **Note**: There should be no error message in the response body.
![SNAGHTMLc14fea4](_v_images/20190830165227876_3954.png)

6. Switch to Section **Monitor** -> Select **Messages**.
    On this dashboard, you will find all the messages processed as per the status. If there is any error, you will find the processing/ error log in the Error Messages section on the dash board.
    Click on **All messages**
![](vx_images/485144598512141.png)

7. If message is successful, it can be seen with **Completed** status. Message Processing    Log (MPL) can be checked on clicking respective links on the right side
![](vx_images/116597474475202.png)

8. Go to the Message Details and check the log for successful processed message.
![](vx_images/427037636374075.png)

9. To check the output file, open attachment in mail received and view the output which should be a CSV file containing the Tax Details
![SNAGHTMLce87901](_v_images/20190830204326507_15988.png)

10. Repeat steps 5-9 with
    **Account_Type: Department_I**
    Check the mail
![SNAGHTMLcea2f8b](_v_images/20190830204518792_23943.png)
![SNAGHTMLceae3a8](_v_images/20190830204604913_5240.png)

