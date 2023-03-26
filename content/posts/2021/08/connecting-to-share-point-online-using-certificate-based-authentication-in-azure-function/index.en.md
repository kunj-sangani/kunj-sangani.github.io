---
weight: 4
title: "Connect SharePoint Online w/ Azure Function & Certificates."
date: 2021-08-08T21:57:40+08:00
lastmod: 2021-08-08T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Connecting to SharePoint Online with Azure Function using Certificate-based authentication and PnP Core."

tags: ["Azure Function", "SharePoint Online", "Azure AD", "Certificate Authentication"]
categories: ["SharePoint Online"]

lightgallery: true
---

Introduction
------------

In this article, we will understand how we can connect SharePoint Online from Azure Function using Application based authentication.

Previously we learned to use Azure Access Control (ACS) where we used to create client id and client secret (from \_layouts/15/appregnew.aspx) but this method has been retired and is not recommended to use and hence in the new Tenants, ACS app-only access is disabled by default. So to overcome this we can use Certificate-based Authentication for connection to SharePoint online.

In this article, we will divide the sections into two halves,

1.  Creating a Certificate that can be used in Azure Active Directory and Azure Function.
2.  Using PnP Core to Connecting to SharePoint Online using Azure Function.

Section 1 - Creating Certificate and Azure AD application
---------------------------------------------------------

Let us first understand how we can generate a Certificate for connecting to SharePoint.

**Step 1**

Open PowerShell and run the below command to Install PnP PowerShell,

(Please note if you already have PnP PowerShell installed skip this step).
```
Install-Module -Name "PnP.PowerShell"

```

**Step 2**

Once the PnP.PowerShell is Installed we can leverage the command Register-PnPAzureADApp which would help us create a new self-signed certificate that would be linked to an Azure AD application and for the testing purpose it would also be installed to the current user store and hence it is recommended to use the below command with PowerShell running as Administration
```
$CertificatePassword="password"
$Tenant="testinglala.onmicrosoft.com"
$CertificateOutDir = ".\Certificates"
if (-not ( Test-Path -Path $CertificateOutDir -PathType Container )) {
    md $CertificateOutDir
}
$app = Register-PnPAzureADApp -ApplicationName "ConnectingToSharePointFromAzureFunction" -Tenant $Tenant -OutPath $CertificateOutDir `
    -CertificatePassword (ConvertTo-SecureString -String $CertificatePassword -AsPlainText -Force) `
    -Scopes "SPO.Sites.FullControl.All" `
    -Store CurrentUser -DeviceLogin
Write-Host $app.'AzureAppId/ClientId'
Write-Host $app.'Certificate Thumbprint'

```

Please change the value of the password as per our requirement even change the value of the tenant name.

(**Note** \- We can even change the scope value as per our requirement as that would add other permission to the Azure AD Application).

The output of the above command would be two values as displayed in the image below one is the Azure AD application ID and the other one is the certificate thumbprint. Keep both the values handy as we would require these values to connect to SharePoint online using Certificate.

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function.png)

Two important files would be created inside the folder named Certificates which would be,

**1) ConnectingToSharePointFromAzureFunction.cer** 

This file is used in the Azure Active Directory Application which we can check by login into our Azure Active Directory and searching for the application with the Client Id obtained from the above PowerShell as displayed in the image below

**2) ConnectingToSharePointFromAzureFunction.pfx**

This file will be used in the Azure Function TLS/SSL Setting which can be uploaded under the Private certificate section we would even require to provide the password which we have set while running the above PowerShell Script

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function1.png)

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function2.png)

Section 2 - Creating Azure Function and using PnP Core to connect to SharePoint Online
--------------------------------------------------------------------------------------

Follow the below steps to create an Azure Function,

**Step 1**

Open Visual Studio and Click on Create a new project.

**Step 2**

Select Azure Function as the project type as displayed in the image below,

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function3.png)

**Step 3**

Provide appropriate project name and project location and click on Create.

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function4.png)

**Step 4**

Select the trigger type for this article. We have to use HTTP trigger but we can leverage any other triggers as well as per our choice.

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function5.png)

**Step 5 - Update the local.settings.json**

Once the project is created open local.settings.json and update it with the below JSON.

**Note**  
Please update the value of SiteUrl, TenantId, ClientId, CertificateThumbprint, and WEBSITE\_LOAD\_CERTIFICATES as per the values obtained from the above PowerShell execution.
```
{
    "IsEncrypted": false,
    "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet",
    "SiteUrl": "https://testinglala.sharepoint.com/sites/Test",
    "TenantId": "52d05dd6-9e21-494f-8502-77658dbe16b8",
    "ClientId": "1ee21bec-967f-4801-8aa1-a0ebbf86280d",
    "CertificateThumbPrint": "F0..............................",
    "WEBSITE_LOAD_CERTIFICATES": "F0..............................."
    }
}

```

**Step 6 - Add required Nuget Package**

Add below Nuger Packages which would be required for extending the Azure Function and connecting SharePoint Online using PnP Core,

1.  Microsoft.Azure.Functions.Extensions
2.  PnP.Core.Auth

![Connecting to SharePoint Online using Certificate based Authentication in Azure Function](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/Connecting%20to%20SharePoint%20Online%20using%20Certificate%20based%20Authentication%20in%20Azure%20Function6.png)

**Step 7**

Add AzureFunctionSettings.cs file into the Solution which will help us to get all the configuration for the Azure Function and update it with the below code,
```
using System;
using System.Collections.Generic;
using System.Security.Cryptography.X509Certificates;
using System.Text;

namespace ConnectSharePointOnline
{
    class AzureFunctionSettings
    {
        public string SiteUrl { get; set; }
        public string TenantId { get; set; }
        public string ClientId { get; set; }
        public StoreName CertificateStoreName { get; set; }
        public StoreLocation CertificateStoreLocation { get; set; }
        public string CertificateThumbprint { get; set; }
    }
}
```

**Step 8**

Add a file named Startup.cs which will help us to add DependencyInjection to all the Azure Functions in the solution.

Update the Startup.cs file with the below code,
```
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using PnP.Core.Auth;
using System;
using System.Collections.Generic;
using System.Security.Cryptography.X509Certificates;
using System.Text;
[assembly: FunctionsStartup(typeof(ConnectSharePointOnline.Startup))]
namespace ConnectSharePointOnline {
    class Startup: FunctionsStartup {
        public override void Configure(IFunctionsHostBuilder builder) {
            var config = builder.GetContext().Configuration;
            var azureFunctionSettings = new AzureFunctionSettings();
            config.Bind(azureFunctionSettings);
            builder.Services.AddPnPCore(options => {
                // Disable telemetry because of mixed versions on AppInsights dependencies
                options.DisableTelemetry = true;
                // Configure an authentication provider with certificate (Required for app only)
                var authProvider = new X509CertificateAuthenticationProvider(azureFunctionSettings.ClientId, azureFunctionSettings.TenantId, StoreName.My, StoreLocation.CurrentUser, azureFunctionSettings.CertificateThumbprint);
                // And set it as default
                options.DefaultAuthenticationProvider = authProvider;
                // Add a default configuration with the site configured in app settings
                options.Sites.Add("Default", new PnP.Core.Services.Builder.Configuration.PnPCoreSiteOptions {
                    SiteUrl = azureFunctionSettings.SiteUrl,
                        AuthenticationProvider = authProvider
                });
            });
        }
    }
}
```

**Step 9**

Update the Function1.cs file where we can connect to SharePoint Online. For this example purpose we will bring all the documents present in the Shared Documents library.

```
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using PnP.Core.Services;
using PnP.Core.Model.SharePoint;
using System.Linq;
namespace ConnectSharePointOnline {
    public class Function1 {
        private readonly IPnPContextFactory pnpContextFactory;
        public Function1(IPnPContextFactory pnpContextFactory) {
                this.pnpContextFactory = pnpContextFactory;
            }
            [FunctionName("Function1")]
        public async Task < IActionResult > Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger log) {
            log.LogInformation("C# HTTP trigger function processed a request.");
            using(var pnpContext = await pnpContextFactory.CreateAsync("Default")) {
                log.LogInformation("Getting all the shared documents on site.");
                IList sharedDocuments = await pnpContext.Web.Lists.GetByTitleAsync("Documents", l => l.RootFolder);
                var sharedDocumentsFolder = await sharedDocuments.RootFolder.GetAsync(f => f.Files);
                var documents = (from d in sharedDocumentsFolder.Files.AsEnumerable() select new {
                    d.Name,
                        d.TimeLastModified,
                        d.UniqueId
                }).ToList();
                return new OkObjectResult(new {
                    documents
                });
            }
        }
    }
}
```

**Outcome**

![](https://f4n3x6c5.stackpathcdn.com/article/connecting-to-sharepoint-online-using-certificate-based-authentication-in-azure/Images/outcome.gif)

Conclusion
----------

In this article, we can understand how we can use Application permission to connect to SharePoint online using Certificate-based Authentication in the Azure Active Directory Application.