# Outgoing Web Hook In Teams Using Yeoman Teams Generator


Introduction
------------

This article will help us understand Outgoing Webhook in MS Teams, and is a step by step guid for creating outgoing webhook using yeoman teams generator.

Outgoing Webhook in MS Teams acts like a bot which can be triggered from channel using @mention, web hook can receive messages posted along with the @mention and can send reply based on the incoming message.

In this article we will focus on creating Outgoing web hook using yeoman generator and use it in MS Teams.

**Note**

Outgoing webhooks are scoped at the team level.

Setup Environment for Yeoman Teams Generator
--------------------------------------------

*   Install Latest version of Nodejs or we can use nvm if we require multiple versions of node js. To check the complete installation guide check my blog  [here](https://www.c-sharpcorner.com/Blogs/how-to-use-multiple-nodejs-version-in-windows-os).

*   Install yeoman and gulp cli  
```      
npm install yo gulp-cli --global
```
*   Install Microsoft teams app generator  
```      
npm install generator-teams --global
```
Create Outgoing Webhook using yeoman Teams generator
----------------------------------------------------

To create the solution we need to use the below command and provide below parameters which will help in creating the solution
```
mkdir webhookwithYeoman
cd webhookwithYeoman    
yo teams
``` 

![Outgoing Web Hook In Teams Using Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/outgoing-web-hook-in-teams-using-yeoman-teams-generator/Images/Screenshot%202020-10-11%20at%201.46.55%20PM.png)

Now let us open the code and check the project structure.

The main folder which we need to concentrate is src folder; inside src we have two folders app and manifest.

The app folder contains the code and manifest folder contains all the necessary files required for deployment in MS Teams

Our code for outgoing web hook will be present inside the folder messagesOutgoingWebhook with the file named message.ts. This will depend on the name of the web hook which you have provided.

If we check the code we can understand it is a simple echo bot which would send the same message which is received in the request
```
import \* as builder from "botbuilder";  
import \* as express from "express";  
import \* as crypto from "crypto";  
import { OutgoingWebhookDeclaration, IOutgoingWebhook } from "express-msteams-host";  

@OutgoingWebhookDeclaration("/api/webhook")  
export class Messages implements IOutgoingWebhook {  

public constructor() {  
}  

public requestHandler(req: express.Request, res: express.Response, next: express.NextFunction) {  
// parse the incoming message  
const incoming = req.body as builder.Activity;  

// create the response, any Teams compatible responses can be used  
const message: Partial<builder.Activity> = {  
type: builder.ActivityTypes.Message  
};  

const securityToken = process.env.SECURITY\_TOKEN;  
if (securityToken && securityToken.length > 0) {  
// There is a configured security token  
const auth = req.headers.authorization;  
const msgBuf = Buffer.from((req as any).rawBody, "utf8");  
const msgHash = "HMAC " + crypto.  
createHmac("sha256", Buffer.from(securityToken as string, "base64")).  
update(msgBuf).  
digest("base64");  

if (msgHash === auth) {  
// Message was ok and verified  
message.text = \`Echo ${incoming.text}\`;  
} else {  
// Message could not be verified  
message.text = \`Error: message sender cannot be verified\`;  
}  
} else {  
// There is no configured security token  
message.text = \`Error: outgoing webhook is not configured with a security token\`;  
}  

// send the message  
res.send(JSON.stringify(message));  
}  
}  
```
From the above code we can understand that we are using crypto to decrypt the message and obtain the hash and we will compare this hash which will be provided from MS Teams when we register our Webhook.

Let us run the application using the below command

1.  gulp ngrok-serve  

![Outgoing Web Hook In Teams Using Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/outgoing-web-hook-in-teams-using-yeoman-teams-generator/Images/Screenshot%202020-10-11%20at%203.32.20%20PM.png)

This will provide us with a dummy domain which we can use as a call back URL in our application.

As displayed in the image HOSTNAME is 0b0294fab980.ngrok.io, hence my Callback URL would be https://0b0294fab980.ngrok.io/api/webhook

Now let us add Webhook in our Teams channel and obtain the security token.

**Step 1**

Click on More options(…) on Teams

**Step 2**

On the fly out menu select "Manage Team"

**Step 3**

Select the Apps Tab in Manage Teams section and click on Create an outgoing webhook

![Outgoing Web Hook In Teams Using Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/outgoing-web-hook-in-teams-using-yeoman-teams-generator/Images/Screenshot%202020-10-11%20at%203.17.03%20PM.png)

**Step 4**

Fill in the form with appropriate data

*   Name - This name would be used in @mention name
*   Callback URL - The URL copied from above _(https://0b0294fab980.ngrok.io/api/webhook)_
*   Description - This would be used for describing the outgoing webhook

![Outgoing Web Hook In Teams Using Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/outgoing-web-hook-in-teams-using-yeoman-teams-generator/Images/Screenshot%202020-10-11%20at%203.55.58%20PM.png)

**Step 5**

Once we click on create we will get an access token which will be used for authenticating the data.

Copy the code and add it in the .env file where the variable name is SECURITY\_TOKEN.

Once we update this value restart gulp ngrok-serve and update the domain in the web hook by following  step 1 and step 2 and then select the Apps Tab and click on the web hook which is already added and update the callback URL with the new domain name

![Outgoing Web Hook In Teams Using Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/outgoing-web-hook-in-teams-using-yeoman-teams-generator/Images/4_token.png)

**Outcome**

![Outgoing Web Hook In Teams Using Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/outgoing-web-hook-in-teams-using-yeoman-teams-generator/Images/Outcome.gif)

Conclusion 
-----------

Creating outgoing webhook with yeoman teams generator is very easy and we can use bots as well and can even use them to send interactive messages using adaptive cards.

This can be very helpful when we do not want to create a dedicated bot but would need a less intense task which does not take much time.

Happy coding !!!
