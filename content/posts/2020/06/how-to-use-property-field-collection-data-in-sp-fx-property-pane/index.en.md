---
weight: 4
title: "How To Use PropertyFieldCollectionData In SPFx Property Pane"
date: 2020-06-22T21:57:40+08:00
lastmod: 2020-06-22T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "This article shows the basic Markdown syntax and format."

canonicalurl: "https://www.c-sharpcorner.com/article/how-to-use-propertyfieldcollectiondata-in-spfx-property-pane/"

tags: ["SPFx", "SharePoint Online"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

With the introduction of the SharePoint Framework (SPFx) ,  front end development has become very easy and hosting of the webpart is taken care of by SharePoint, which does not require an additional server. So the use of SPFx is growing exponentially and the best part of SPFx webparts is they are easy to configure.

To Configure SPFx web part we have Property Pane which opens up in a panel and we can add any controls as per our need. Out of the box SPFx has given us many controls but there are a few controls which are not available. To tackle this we have the PnP Community which takes care of creating reusable property pane controls. To check all the property pane controls check the [link](https://pnp.github.io/sp-dev-fx-property-controls/).

Let us look into one of the controls which is very helpful if we want to fetch the data in SPFx configuration. This control is known as PropertyFieldCollectionData. To check the complete documentation go to this [link](https://pnp.github.io/sp-dev-fx-property-controls/controls/PropertyFieldCollectionData/).

Types of Fields which we can create in PropertyFieldCollectionData

| Type | Description |
| ------ | ----------- |
| string | This will create a text field to store the string value |
| number | This will be a text field which accepts only number value |
| Boolean | This will create a checkbox which can store a Boolean value |
| dropdown | This will create dropdown fields where we can specify options value |
| fabricIcon | This will create an icon |
| Url | This will create a URL field |
| custom | This gives control for custom rendering of the field |

In this article, we will cover the step by step process of how to use this control in our SPFx Project.

**Step 1- Create a SPFx solution and webpart**

Run the below sequence of commands in the command prompt or nodejs command prompt.

Below command will be used to create a folder
```
md PnPPropertyFieldCollectionData    
cd PnPPropertyFieldCollectionData   
```
Let us now create the SPFx project in the above-created folder.
```
yo @microsoft/sharepoint  
```
![How To Use PropertyFieldCollectionData In SPFx Property Pane](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-propertyfieldcollectiondata-in-spfx-property-pane/Images/1_CreateSPFxSolution.png)

Now let us install the required npm package to use PnP Property pane control.
```
npm install @pnp/spfx-property-controls  
npm install @pnp/spfx-controls-react --save --save-exact
```
**Step 2 - Update the code**

Open the PnpPropertyFieldCollectionDataWebPart.ts file and add the below code.

Update the import with the below code.
```
import { PropertyFieldCollectionData, CustomCollectionFieldType } from '@pnp/spfx-property-controls/lib/PropertyFieldCollectionData';   
```
Add the variable in the property interface, the variable would be of type any\[\] as it would store collection of data items.
```
export interface IPnpPropertyFieldCollectionDataWebPartProps {  
  description: string;  
  collectionData: any[];  
}  
```
We can have many different types of columns for our data collection and if we need to have a custom column as well we can create them.

For the demo we are using string, number, dropdown, Boolean and in the custom field we are using PnP People Picker control.

Add the below code in getPropertyPaneConfiguration() method insides the groups collection.
```
PropertyFieldCollectionData("collectionData", {  
    key: "collectionData",  
    label: "Collection data",  
    panelHeader: "Collection data panel header",  
    manageBtnLabel: "Manage collection data",  
    value: this.properties.collectionData,  
    fields: [  
        {  
            id: "Title",  
            title: "Firstname",  
            type: CustomCollectionFieldType.string,  
            required: true  
        },  
        {  
            id: "Lastname",  
            title: "Lastname",  
            type: CustomCollectionFieldType.string  
        },  
        {  
            id: "Age",  
            title: "Age",  
            type: CustomCollectionFieldType.number,  
            required: true  
        },  
        {  
            id: "City",  
            title: "Favorite city",  
            type: CustomCollectionFieldType.dropdown,  
            options: [  
                {  
                    key: "pune",  
                    text: "Pune"  
                },  
                {  
                    key: "junagadh",  
                    text: "Junagadh"  
                }  
            ],  
            required: true  
        },  
        {  
            id: "Sign",  
            title: "Signed",  
            type: CustomCollectionFieldType.boolean  
        }, {  
            id: "customFieldId",  
            title: "People picker",  
            type: CustomCollectionFieldType.custom,  
            onCustomRender: (field, value, onUpdate, item, itemId, onError) => {  
                return (  
                    React.createElement(PeoplePicker, {  
                        context: this.context,  
                        personSelectionLimit: 1,  
                        showtooltip: true,  
                        key: itemId,  
                        defaultSelectedUsers: [item.customFieldId],  
                        selectedItems: (items: any[]) => {  
                            console.log('Items:', items);  
                            item.customFieldId = items[0].secondaryText;  
                            onUpdate(field.id, items[0].secondaryText);  
                        },  
                        showHiddenInUI: false,  
                        principalTypes: [PrincipalType.User]  
                    }  
                    )  
                );  
            }  
        }  
    ],  
    disabled: false  
})  
```
Let us understand the above code,

  
We are creating six columns for our Data out of which two are of type string  column Title and Lastname.

The age field is of type number,  city field is of type dropdown with values as "Pune" and "Junagadh." Sign is of type boolean and for the custom field, we are using the PnP People picker field for storing user email id.  
  
For the custom field, onCustomRender is the mandatory and important function which indicates which field we would like to display. For storing the data we are using onUpdate function which would update the value of the collection.

**Step 3 - Use the data which is added in the property pane**

Update the interface with the values as displayed below, 
```
export interface IPnpPropertyFieldCollectionDataProps {  
  description: string;  
  collectionData: any[];  
}  
```
Now pass the collected data to the react component to display it in our page.

Update the render method in the .ts file of the web part as displayed below.
```
public render(): void {  
    const element: React.ReactElement<IPnpPropertyFieldCollectionDataProps> = React.createElement(  
      PnpPropertyFieldCollectionData,  
      {  
        description: this.properties.description,  
        collectionData: this.properties.collectionData  
      }  
    );  
  
    ReactDom.render(element, this.domElement);  
  }  
```
Now to display it in the HTML open the .tsx file of the webpart and append the below code in the render method.
```
<div className={styles.row}>  
{this.props.collectionData && this.props.collectionData.map((val) => {  
  return (<div><span>{val.Title}</span><span style={{ marginLeft: 10 }}>{val.Lastname}</span></div>);  
})}  
</div>
```
**outcome**

![How To Use PropertyFieldCollectionData In SPFx Property Pane](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-propertyfieldcollectiondata-in-spfx-property-pane/Images/Outcome.gif)

Complete code is available in the below [link]( https://github.com/kunj-sangani/PnPPropertyFieldCollectionData).

Conclusion
----------

So with PropertyFieldCollectionData control, we can create a complete form with different fields where the data can be stored and can be retrieved. This can be very helpful when we need to use this for configuring email templates and when we want the webpart to have multiple configurations.