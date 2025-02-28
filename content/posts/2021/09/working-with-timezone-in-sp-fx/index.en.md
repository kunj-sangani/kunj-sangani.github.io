---
weight: 4
title: "Working With Timezone In SPFx"
date: 2021-09-21T21:57:40+08:00
lastmod: 2021-09-21T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Working with timezones in SharePoint Framework (SPFx) using Moment Timezone and jstz."

canonicalurl: "https://www.c-sharpcorner.com/article/working-with-timezone-in-spfx/"

tags: ["SharePoint Online", "SPFx", "Moment", "jstz"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

While designing applications, we may come across multiple scenarios where our application could run across multiple locations and timezones, and working with all these timezones could be a difficult task.

With the introduction of SharePoint framework, creating a scalable and UI friendly application is very easy. With modern SharePoint, creating an Intelligent intranet is very easy and is becoming popular. There can be a scenario where intranet users are across multiple locations, therefore we need to create an SPFx solution considering all these locations.

We will use Moment Timezone to get all the timezones and work with conversions of the timezone. To get the current user’s timezone, we will use jstz to fetch the current users' timezone. Before diving into the SPFx solution, let us understand how SharePoint works with timezones out of the box

How SharePoint stores date and time field
-----------------------------------------

Out of the box, SharePoint Converts the date and time field in the regional setting of the site.

  
To understand this let's see an example:

1.  Let's create a sample list name Learning DateTime.
2.  Add a new field named SampleDateTime where we would also store time values.
3.  Check the regional setting of the site collection to check that navigate to site setting and select the regional setting, as displayed in the image below:  
      
![Image](https://f4n3x6c5.stackpathcdn.com/article/working-with-timezone-in-spfx/Images/Screenshot%202120-09-20%20at%204.02.15%20PM.png)  
      
    
4.  Check the regional setting time zone value. So, if a User say for example is creating a new item in the list, the date and time value will be converted as per the regional setting and would be stored in the ISO format in the list.  
      
![Image](https://f4n3x6c5.stackpathcdn.com/article/working-with-timezone-in-spfx/Images/Screenshot%202120-09-20%20at%204.06.19%20PM.png)  
      
    
5.  Let's create a new item in the list and check what is the value that is being stored in the list. As displayed in the image below, we have selected 8 am as the time, but when we fetch the data, it provides the time as (15), which is 3 pm. This means it has added 7 hours to our selected time, which is because of the timezone difference.

![Image](https://f4n3x6c5.stackpathcdn.com/article/working-with-timezone-in-spfx/Images/Screenshot%202120-09-20%20at%204.13.14%20PM.png)

![Image](https://f4n3x6c5.stackpathcdn.com/article/working-with-timezone-in-spfx/Images/Screenshot%202120-09-20%20at%204.16.45%20PM.png)

How to store data in SharePoint List without conversion
-------------------------------------------------------

To store the date-time without converting to site collection regional setting, we can pass the date and time in the iso format using the REST API and this will not convert our entered data in the regional setting

1.  URL:- https://testinglala.sharepoint.com/sites/Test/\_api/web/lists/getbytitle('Learning datetime')/items?$select=Title,SampleDateTime    

3.  Data:- {Title:'Test POST',SampleDateTime:"2021-09-20T15:30:00Z","\_\_metadata": {"type": "SP.Data.Learning\_x0020\_datetimeListItem"} }   

Create an SPFx Solution to handle different timezone
----------------------------------------------------

To create an SPFx solution, let's run the below command and follow the steps as displayed in the below image.
```
mkdir timezones
cd timezones
yo @microsoft/sharepoint
```
![Image](https://f4n3x6c5.stackpathcdn.com/article/working-with-timezone-in-spfx/Images/Screenshot%202120-09-20%20at%204.38.24%20PM.png)

We will also add the required library which would be helpful for our solution and understanding. Use the below command to add those:

1.  PnP js
2.  PnP reusable controls
3.  moment timezone
4.  jstz
```
npm i @pnp/js @pnp/spfx-controls-react moment-timezone jstz - -save - -save-exact
```
Now, let's create a dropdown that would display all the Timezones and users can select the timezone of their choice. To do so, let's update the MultipleTimezone.tsx file.

Add the below import statement in the file at the top of the file along with all the imports.
```
import { PrimaryButton, values } from 'office-ui-fabric-react';    
import { Dropdown, IDropdownOption } from 'office-ui-fabric-react/lib/Dropdown';    
import { DateTimePicker, DateConvention, TimeConvention, TimeDisplayControlType } from '@pnp/spfx-controls-react/lib/dateTimePicker';    
import \* as moment from "moment-timezone";    
import \* as jstz from "jstz";    
import { sp } from "@pnp/sp";    
import "@pnp/sp/webs";    
import "@pnp/sp/lists";    
import "@pnp/sp/items";    
```
Now we will use moment.tz.names() which provides us all the timezone names. We will manipulate it to get the offset for all the time zones. Add the below code in the next line after all the import statements are completed.
```
const selectorOptions = moment.tz.names().reduce((memo, tz) => {  
memo.push({  
name: tz,  
offset: moment.tz(tz).utcOffset()  
});  
return memo;  
}, \[\]).sort((a, b) => {  
return a.offset - b.offset;  
}).reduce((memo, tz) => {  
const timezone = tz.offset ? moment.tz(tz.name).format('Z') : '';  
return memo.concat(\`{"key":"${tz.name}", "text":"(GMT${timezone}) ${tz.name}"},\`);  
}, "");  
const timeZoneOptions = JSON.parse(\`\[${selectorOptions.substring(0, selectorOptions.length - 1)}\]\`);   
```
As we can see from the above code, we created a JSON object of key and text pair which is compatible with office UI fabric dropdown so we will use the timeZoneOptions variable for all the dropdown options. Now to add the actual office UI fabric dropdown, let's remove the HTML provided in the render method and replace it with the below code.
```
<div className={styles.multipleTimeZone}>  
<div className={styles.container}>  
<div className={styles.row}>  
<Dropdown    
placeholder="Select Timezone"    
label="Select Timezone"    
options={timeZoneOptions}    
selectedKey={this.state.selectedTimeZone ? this.state.selectedTimeZone.key : undefined}    
 onChange={(event: React.FormEvent  
<HTMLDivElement>, item: IDropdownOption)=>{this.setState({ selectedTimeZone: item });}}    
/>  
</div>  
</div>  
</div>   
```
We are even storing the timezone selection value, so let's add the state interface and initialize it in the constructor. To do so, add the below interface. It contains additional values as well, which we would use it ahead in our implementation.
```
export interface IMultipleTimeZoneState {  
date: Date;  
dateToStore: string;  
selectedTimeZone: IDropdownOption;  
allItems: any;  
}  
constructor(props: IMultipleTimeZoneProps) {  
super(props);  
this.state = {  
date: undefined,  
dateToStore: undefined,  
selectedTimeZone: undefined,  
allItems: \[\]  
};  
}   
```
Now let's add a date time picker that would help us select date and time. We can use any datetimepicker, but for out article, we have to use pnp reuseable control datetime picker. Update the render method and add the below code:
```
<DateTimePicker label="Select Date"    
dateConvention={DateConvention.DateTime}    
timeConvention={TimeConvention.Hours24}    
value={this.state.date}    
onChange={this.handleChange}    
timeDisplayControlType={TimeDisplayControlType.Dropdown} />     
```
Let's also add the supporting method used in the datetimepicker:
```
private handleChange = (date: Date) => {  
let dateToStore = moment(date).tz(jstz.determine().name()).format('YYYY-MM-DDTHH:mm:ss').concat('Z');  
this.setState({  
date: date,  
dateToStore: dateToStore  
});  
}   
```
The real magic happens over here, as we can observe from the above code we are converting the selected date time into ISO string, but we are considering the user's local date time so it would not add the timezone offset.

**Note**

This is very important so that when we store data in the list, it would not convert it to the regional setting timezone, rather it would store the date time value as it is. Now let's add a new field in our existing list named TimeZone. We will store the selected timezone value in this field so that we can refer to it later.

Let's add a button to store the data to the list. To do so, let's update the render method with the below code:
```
<PrimaryButton style={{ marginTop: 10 }} text="Save Item to List" onClick={this.saveItem} />
```
Let's add the supporting method. To save the data to the list, we are using PnP js to store the data, as it is very easy to use.

```
private saveItem = () => {  
sp.web.lists.getByTitle("Learning datetime").items.add({  
Title: "From SPFx",  
SampleDateTime: this.state.dateToStore,  
TimeZone: this.state.selectedTimeZone.key  
}).then(() => {  
this.getAllItems();  
}).catch((err) => {  
console.log(err);  
});  
}   
```
Now let's fetch the data and convert the DateTime to the local timezone as per the user's selected timezone. To do so, let's first add a method to fetch the data.
```
private getAllItems = () => {  
sp.web.lists.getByTitle("Learning datetime").items.getAll().then((items) => {  
this.setState({  
allItems: items  
});  
}).catch((err) => {  
console.log(err);  
});  
}   
```
Add a call to this method in the constructor so that in the first load of the component we would get all the items and to display it in the HTML. Let's update the render method with the below code:
```
<div className={styles.row}>    
{this.state.allItems && this.state.allItems.map((item, index) => {    
return (  
<div>  
<span style={{ paddingRight: 10 }}>{item.Title}</span>  
<span style={{ paddingRight: 10 }}>{item.SampleDateTime}</span>  
<span style={{ paddingRight: 10 }}>{item.TimeZone}</span>  
<a style={{ paddingRight: 10 }} onClick={() => { this.convertTimeZones(index); }}>  
<span>Convert to Local timezone</span>  
</a>  
<span>{item.convertedTime}</span>  
</div>);    
})}    

</div>    
```
We have added a method which would convert the date time to users local date time. to do so, add the below method:
```
private convertTimeZones = (index: number) => {    
let items = this.state.allItems;    
let convertedTime = moment.tz(items\[index\].SampleDateTime.slice(0, -1), 'YYYY-MM-DDTHH:mm:ss',    
items\[index\].TimeZone).tz(jstz.determine().name()).format('YYYY-MM-DDTHH:mm:ss z');    
items\[index\].convertedTime = convertedTime;    
this.setState({ allItems: items });    
}     
```
The second magic happens over here. We are now converting the time which is stored in the list to users local time zone, but this conversion happens with respect to users selected timezone from the dropdown.

Link to the [complete code](https://github.com/kunj-sangani/TimeZoneinSPFx).

**Outcome**

![](https://f4n3x6c5.stackpathcdn.com/article/working-with-timezone-in-spfx/Images/2021-09-20-18-49-11-online-video.gif)

Conclusion
----------

By using moment timezone and jstz, it becomes very easy to work with timezone in SharePoint. I hope this will help to create a solution where multiple timezones are required. This can even help where we don't want SharePoint to convert the time as per the value of the regional setting.