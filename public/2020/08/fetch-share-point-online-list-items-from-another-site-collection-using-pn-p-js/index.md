# Fetch SP list items from another site using PnP JS.


Introduction
------------

SharePoint Online has gained more popularity with the release of REST API and SPFx. Data operation becomes easy in Client-Side Solution with the help of REST API. All these APIs work under current user permission and there could be a scenario that current users can have access to multiple site collection and we would need to fetch data for cross-site collection.

How we can fetch cross-site collection data is a question, but with the help of PnPJs this question can be easily solved.  
  
Let us look at how we can fetch cross-site collection data using PnPJs in SPFx.

I hope that your environment for SPFx is up and running.

**Step 1 - Create SPFx Solution**

Using the below command we can create an SPFx solution
```
mkdir dataFromOtherSC
cd dataFromOtherSC
yo @microsoft/sharepoint
```
Use the below values when asked for. We will be using React as our front end framework for creating out SPFx webpart.

![](https://f4n3x6c5.stackpathcdn.com/article/use-pnp-js-to-fetch-list-items-from-other-site-collection-in-sharepoint-online/Images/Screenshot%202020-08-02%20at%209.03.13%20AM.png)

**Step 2 - Add Reference to PnPJs**

To add a reference to PnPJs use the below command.
```
npm install @pnp/sp —save   
```
Once the reference is installed we can start the setup. To establish context open the file named:

**DataFromOtherScWebPart.ts**

Import the reference to PnPJs. To do so add the below line:
```
import { sp } from "@pnp/sp/presets/all";   
```
Once the import statement is added we can add the below method to establish context
```
protected onInit(): Promise < void > {  
    return super.onInit().then(_ => {  
        sp.setup({  
            spfxContext: this.context  
        });  
    });  
}    
```
**Step 3 - Fetch data from other site collection or subsite:**

To fetch the data from other site collections we need to created the object of the web. To do so let us update our DataFromOtherSc.tsx file.

Import the below reference in the file:
```
import { Web } from "@pnp/sp/webs";   
```
Create an interface for the state use the below code
```
export interface IDataFromOtherScState {  
   listItems: any;  
} 
```
Now to initiate the object of the web for our required site collection use the below code. This will create a variable which can be accessed in the class.

  
We can provide site collection or subsite URL from where we need to fetch the data.
```
private web = Web("https://testinglala.sharepoint.com/sites/Test/");   
```
To fetch the data from a list of other site collections we can use the below code in componentDidMount method.
```
public componentDidMount = () => {  
    this.web.lists.getByTitle("Employee").items().then((items) => {  
        this.setState({  
            listItems: items  
        });  
    }).catch((err) => {  
        console.log(err);  
    });  
}   
```
Now to access any of the objects for other site collections or subsite we can use the reference of the web.

  
**Note**

This would work with the current user credentials. If the user does not have access then they will not be able to fetch data.

**Outcome**

![](https://f4n3x6c5.stackpathcdn.com/article/use-pnp-js-to-fetch-list-items-from-other-site-collection-in-sharepoint-online/Images/2020-08-02-09-37-20-online-video.gif)

Conclusion
----------

Using PnPJs it is very easy to fetch data across any site collection with the help of web object and this can help give the user a seamless experience for their data across site collections without authenticating again.
