---
weight: 4
title: "Learn PnP DateTimePicker in SPFx."
date: 2020-07-21T21:57:40+08:00
lastmod: 2020-07-21T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Learn how to use PnP DateTimePicker Control in SPFx to add date and time control in SharePoint form customization with the step-by-step guide."

tags: ["SPFx", "SharePoint Online", "PnP Controls"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

When customizing the form with the help of SPFx, we may come across the requirement of using Date Time control. However, if we are using fluent UI react controls, formally known as Office UI fabric React controls, then we do not have control with both date and time. Rather, we only have date picker to overcome this PnP Team has created a DateTimePicker Control this can be easily used and configured in SPFx. To know how we can use it, let's see the step-by-step guide.

**Step 1 - Create an SPFx Solution**
------------

To create an SPFx solution, we are required to have nodejs v10.x installed. Along with that, we are required to have yeoman and gulp installed and SharePoint framework yeoman generator. Once the environment is ready, we can create a new solution to create a new solution using the below command:
```
yo @microsoft/sharepoint
```
After running the above command, we are presented with multiple questions which we can provide values to, as mentioned in the image below.

![How To Use PnP DateTimePicker In SharePoint Framework Webpart(SPFx)](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-datetimepicker-in-sharepoint-framework-webpartspfxs/Images/CreateSolution.png)

**Step 2 - Add reference of PnP SPFx Controls**
------------

To add the reference to the solution for the controls, we can use the below command by adding the reference of PnP SPFx controls we can use many controls which are available. To check all the controls, use the below link.

_https://pnp.github.io/sp-dev-fx-controls-react/#available-controls_
```
npm install @pnp/spfx-controls-react --save --save-exact
```
![How To Use PnP DateTimePicker In SharePoint Framework Webpart(SPFx)](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-datetimepicker-in-sharepoint-framework-webpartspfxs/Images/Add%20Reference.png)

**Step 3 - Update the code**
------------

Now let's use the control in our Web part. Open the file name SamplePnPDateTimePickerWebpart.tsx

To add a reference to the control, use the below import statement:
```
import { DateTimePicker, DateConvention, TimeConvention, TimeDisplayControlType } from '@pnp/spfx-controls-react/lib/dateTimePicker';   
```
Now let us add the below state variable. This is where our selected value for the DateTime would be displayed.
```
export interface ISamplePnPDateTimePickerWebpartState {  
   date: any;  
}   
```
Now let us add the control in our render method of the Web part. Use the below code.
```
<DateTimePicker label="Start Date Time"  
   dateConvention={DateConvention.DateTime}  
   timeConvention={TimeConvention.Hours24}  
   value={this.state.date}  
onChange={this.handleChange} timeDisplayControlType={TimeDisplayControlType.Dropdown} /> 
```
Add the below method to handle change.
```
public handleChange = (date: any) => {  
   alert(date);  
   this.setState({ date: date });  
} 
```
As we can see, there are multiple properties that are used with the control. Let us look into it.

1.  label - This property will accept string value and would be used as the label to control
2.  dateConvention - This property accepts Enum value of DateConvention which is either DateTime or Date to either display date and time both the control or only the date field
3.  timeConvention - This property also accepts the Enum value of TimeConvention which is either Hours12 or Hours24
4.  value - To indicate where the value is stored can also be used for pre-populating the value by updating the value in the constructor
5.  timeDisplayControlType - This field is only helpful when dateConvention is DateTime and not Date. This field also accepts Enum value either as Text or Dropdown

For additional properties please check this [link](https://pnp.github.io/sp-dev-fx-controls-react/controls/DateTimePicker/) with a list of properties.

**Outcome**

![How To Use PnP DateTimePicker In SharePoint Framework Webpart(SPFx)](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-datetimepicker-in-sharepoint-framework-webpartspfxs/Images/Blog4Video.gif)

Conclusion
----------

  
We can use this control for Selection of Date and Time, as this control is currently not available in Fluent UI.

We can even use other libraries for Date and Time Picker like Material UI, Ant Design as per the requirements.