# Custom Connector To Connect Graph API In PowerApps


Introduction
------------

PowerApps is a platform where we can build no code or less code application. This tool is available with Microsoft 365 suit and is very powerful in terms of application development with lots of functions available as per your needs.

But in most cases, when we create a custom application, we would like to connect to the different data sources. One of those data sources for Microsoft 365 is Graph API. This API gives us access to all the applications across the M365 platform. Therefore, to use graph API in Power Apps we need to create a custom connector. Let's understand how we can create a custom connector for Graph API in PowerApps

**Prerequisites**
-----------

Create an Azure AD Application and provide appropriate access for Graph API. To create an Azure AD application, follow the below steps.

*   Navigate to [here](https://portal.azure.com/).
*   Select Azure Active Directory Azure Service
*   Select the App registration option and click on the + New registration button to register a new application
*   Provide the Name of application "PowerAppGraphAPI" and click on Register

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/3_AzureADApplication.png)

*   Now let's create a client secret by selecting the option Certificates & secrets and then selecting + New client secret. This would ask for the description and the expiry time of the secret let us provide "secret" in the description and select never for the expiry time duration.

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/3_AzureADSecret.png)

*   Copy the secret that was generated. It will add the custom connector for PowerApps
*   Now we need to provide access to Graph API to do so click on API permission and click on + Add permission from the Microsoft Graph API section under delegated permission we can select any permission as per our need and can grant admin consent
*   Copy the Azure AD Application ID which was generated and is used while registering the custom connector for Azure AD application

**Step 2 - Create a Custom Connector for Graph API in PowerApps**
-----------

For registering the custom navigator, follow the below steps:

*   Navigate [here](https://make.powerapps.com/).
*   Expand the Data section and select Custom Navigator
*   Click on + New custom connector and select create from a blank
*   Provide the name of the connector as GraphAPI and click on continue

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/1_CreateNewConnector.png)

*   We would be provided with a form for creating a custom connector with 4 different sections General, Security, Definition, Test
*   In the General, Section Update the hostname with the below value  
      
    _graph.microsoft.com_

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/2_AddHost.png)

*   Click on the Security to move forward in the form
*   In the security, section change the authentication type from No authentication to OAuth 2.0
*   Change the identity provider to Azure Active Directory and paste the client ID and client secret which we generated from the above section while registering Azure AD application
*   In the Resource, URL section update the value as https://graph.microsoft.com
*   Now click on Definition to move forward and complete the registration

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/3_AuthType.png)

*   Click on Create Connector
*   Now navigate back to security and copy the redirect URL and paste In the Azure AD created in the pre-requisite section and add it by clicking on Add a Redirect URI and click on + Add platform and select web and paste the URI and click on configure

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/4_AzureADRedirectURL.png)

*   Now in the definition section click on + New action to add the graph API endpoint
*   Add the values as displayed in the image below

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/5_NewAction.png)

*   In the request part, click on Import from the sample and select Get and in the URL section add /v1.0/me/

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/6_ImportRequest.png)

*   In the response part, click on add default response and past the JSON by navigating to graph explorer and getting the JSON [here](https://developer.microsoft.com/en-us/graph/graph-explorer/preview).  
```
{  
    "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",  
    "businessPhones": ["+1 412 555 0109"],  
    "displayName": "Megan Bowen",  
    "givenName": "Megan",  
    "jobTitle": "Auditor",  
    "mail": "MeganB@M365x214355.onmicrosoft.com",  
    "mobilePhone": null,  
    "officeLocation": "12/1110",  
    "preferredLanguage": "en-US",  
    "surname": "Bowen",  
    "userPrincipalName": "MeganB@M365x214355.onmicrosoft.com",  
    "id": "48d31887-5fad-4d73-a9f5-3c356e68a038"  
} 
```   

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/7_ImportResponse.png)

*   Click on Test and click on Update Connector. Now let's use this connector in our Power App

**Step 3 - Create PowerApp and use the custom connector**
-----------

To use the custom connector we created, we create a blank canvas application and add the connection to the graph connector that we created

Follow the below steps to create PowerApp and add a reference to the custom connector:

*   Navigate to https://make.powerapps.com
*   Click on create
*   Select Canvas app from blank as a type of PowerApps
*   Provide the name of the app as GraphAPIUser and select the format as Tablet
*   Add a four text input, four labels, and button
*   Change the label text as "First Name", "Last Name", "Email Address", "User ID"
*   Change the text of the button as "Fetch User Data"
*   Now add a connection to the GraphAPI connector that we created by selecting data source and expanding connectors section and selecting the connector that we created.

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/8_AddConnection.png)

*   Select the button and update the formula to fetch the user data by adding the below formula in OnSelect field  
``` 
UpdateContext({MyProfile:GraphAPI.getmyuserprofile()})  
``` 

*   Update the default property of all the four text field with the below values:

1.  MyProfile.givenName
2.  MyProfile.surname
3.  MyProfile.mail
4.  MyProfile.id

*   Finally, the application is ready. Now, when we click on the Fetch user Data button, we will get current users details from Graph API.

**Outcome**

![](https://f4n3x6c5.stackpathcdn.com/article/custom-connector-to-connect-graph-api-in-powerapps/Images/outcome.gif)

Conclusion 
-----------

With the help of a custom connector, we can leverage graph API in PowerApps. Even custom connector becomes very handy if we want to connect to any third party application whose connector is not available
