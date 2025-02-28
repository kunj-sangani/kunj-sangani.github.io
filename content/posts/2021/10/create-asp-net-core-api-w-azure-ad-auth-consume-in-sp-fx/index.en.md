---
weight: 4
title: "Create ASP.NET Core API w/ Azure AD Auth & consume in SPFx."
date: 2021-10-13T21:57:40+08:00
lastmod: 2021-10-07T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "This article shows the basic Markdown syntax and format."

canonicalurl: "https://www.c-sharpcorner.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/"

tags: ["SPFx","WebAPI", "Azure AD", "MSAL.js"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

The practice of creating Custom APIs using ASP.NET core is increasing day by day as ASP.NET core is platform agnostic and is based on the C# language. Securing your API is very important as we do not want everyone to access the data, hence we will explore Azure AD based authentication in ASP.NET Core Web API and consume the API using SharePoint Framework.

There are two aspects to this article:

1.  Creating ASP.NET Core web API secured using Azure AD
2.  Consuming the API in SharePoint Framework

Let us first understand how we can create a secure web API.

ASP.NET Core Web API 
---------------------

To create ASP.NET Core web API we can use .NET Core CLI or Visual Studio. In this article we will use Visual Studio to create a Web API.  
  
Open Visual Studio and select Create a New Project.

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/1_CreateProject.png)

From the available template select ASP.NET Core Web Application as the type of the Project with C# as language

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/2_SelectProjectType.png)

Provide the project name as "SecuredWebAPI" and click on create

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/3_ProjectName.png)

In the next Screen Select API and then change the authentication type from No Authentication to Work or School Accounts. Provide the domain name of your tenant and click on OK login with tenant admin user if not already logged in.

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/4_AuthType.png)

Internally when the API gets created there is an Azure AD application which also gets created as displayed in the image below

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/5_AzureAD.png)

  
In this Azure AD Application there is another thing called Expose an API which also has an entry with Scope configured. This is created by Visual Studio itself for our convenience. 

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/6_AzureADScopes.png)

We need to update the code in startup.cs so that it will accept a valid audience and valid Issuers. To do that update the below code in ConfigureServices method, comment the services.AddAuthentication line and replace it with the below code which will include valid audience and valid Issuers. We will also enable CORS as we will be consuming the Web API in SharePoint which would have a different domain.
```
services.AddCors();  
services.AddAuthentication(sharedoptions =>  
{  
sharedoptions.DefaultScheme = JwtBearerDefaults.AuthenticationScheme;
}).AddJwtBearer(options =>  
{  
options.Authority = "https://login.microsoftonline.com/52d05dd6-9e21-494f-8502-77658dbe16b8";  
options.TokenValidationParameters = new Microsoft.IdentityModel.Tokens.TokenValidationParameters  
{  
ValidAudiences = new List<string> { "https://testinglala.onmicrosoft.com/SecuredWebAPI" },  
// set the following issuers if you want to support both V1 and V2 issuers  
ValidIssuers = new List<string> { "https://sts.windows.net/52d05dd6-9e21-494f-8502-77658dbe16b8/","https://login.microsoftonline.com/52d05dd6-9e21-494f-8502-77658dbe16b8/v2.0" }  
};  
});  
```

Now let us start the web API and test it in Postman to check whether it is authenticating or not.

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/7_GraphCAll.png)

  
  
As displayed in the image the API provides us 401 Unauthorized as the output code

Consume the web API using SPFx
------------------------------

  
To consume the web API in SPFx let us first create an SPFx solution.

Use the below code in Node.js command prompt to create SPFx solution.
```
md callSecuredAPIGraph  

cd callSecuredAPIGraph  
yo @microsoft/sharepoint   
```
Select the below answers when prompted with questions as displayed in the image below.

We will use MSAL.js to fetch the token and use that token to call our web API. This token would be a bearer token so let us add the dependency of MSAL.js using the below command.
```
npm install msal --save  
```
Now update the below code in the WebPart.ts file.

Import the reference to MSAL.js using the below code:
```
import \* as Msal from 'msal';  
```
Add the below variable as constants as we will not update those (Please update the clientId, authority and redirectUri as per your tenant and application id in azure ad and tenant Id):
```
const msalConfig = {  
auth: {  
clientId: '914a7422-244b-4684-9e47-6f020d014967',  
authority: "https://login.microsoftonline.com/52d05dd6-9e21-494f-8502-77658dbe16b8",  
redirectUri: 'https://testinglala.sharepoint.com/\_layouts/15/workbench.aspx'  
}  
};  
const msalObj = new Msal.UserAgentApplication(msalConfig);   
```
Now let us add the below code to fetch the token. We will fetch this before our code gets rendered update the render method with the below code:

```
public render(): void {  
msalObj.handleRedirectCallback(error1 => {  
console.log(error1);  
});  
if (!msalObj.getAccount()) {  
msalObj.loginRedirect({  
scopes: \["https://testinglala.onmicrosoft.com/AzureADCoreAPI/user\_impersonation"\]  
});  
} else {  
msalObj.acquireTokenPopup({  
scopes: \["https://testinglala.onmicrosoft.com/AzureADCoreAPI/user\_impersonation"\]  
}).then((val) => {  
console.log(val);  
const element: React.ReactElement < ICallSecuredApiProps > = React.createElement(CallSecuredApi, {  
description: this.properties.description,  
context: this.context,  
accessToken: val.accessToken  
});  
ReactDom.render(element, this.domElement);  
}).catch((error) => {  
console.log(error);  
});  
}  
}   
```

To fetch the data from Web API let us update our CallSecuredApi.tsx file. Add a state variable response and initialize it with empty array and add below method which would fetch the API data:
```
public componentDidMount() {  
this.props.context.httpClient.get('https://localhost:44364/api/values', HttpClient.configurations.v1, {  
headers: {  
'Authorization': \`Bearer ${this.props.accessToken}\`  
},  
 method: 'GET'  
}).then((response: HttpClientResponse) => {  
response.json().then((val) => {  
console.log(val);  
this.setState({  
response: val  
});  
}).catch((erro) => {  
console.log(erro);  
});  
}).catch((error) => {  
console.log(error);  
});  
}   
```
Update the render method with the below code to display the obtained data from the web API.
```
public render(): React.ReactElement  
<ICallSecuredApiProps> {    
return (
<div className={styles.callSecuredApi}>  
<div className={styles.container}>  
<div className={styles.row}>  
<div className={styles.column}>  
<span className={styles.title}>Welcome to SharePoint!</span>    
{this.state.response && this.state.response.map((val) => {    
return (  
<div>{val}</div>);    
})}    

</div>  
</div>  
</div>  
</div>    
);    

}     
```
Now we can start our SPFx solution using the below code
```
gulp serve  
```
**Outcome**

![Create An ASP.NET Core API With Azure AD Authentication And Consuming It In SPFx](https://f4n3x6c5.stackpathcdn.com/article/create-an-asp-net-core-api-with-azure-ad-authentication-and-consuming-it-in-spfx/Images/Outcome.gif)

Conclusion
----------

Securing Web API in ASP.NET core is very easy and consuming it in SPFx using MSAL.js gives us a good opportunity to create an end to end solution for enterprise which is scalable. We can leverage ASP.NET core API which can help us to create robust Web API for our desired application.

Happy Codding !!! 