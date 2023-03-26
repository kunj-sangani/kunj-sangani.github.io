---
weight: 4
title: "Create Rich Text control w/ image and video in SPFx."
date: 2020-07-22T21:57:40+08:00
lastmod: 2020-07-22T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Learn how to use react-quill control to customize rich text fields in Modern SharePoint List Forms, based on Quill and trusted by Microsoft."

tags: ["SPFx", "SharePoint Online", "React-Quill"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

Rich Text Control is one of the most-used controls in classic SharePoint Lists but if we need to create a customization for Modern SharePoint List form using SharePoint Framework there is no easy way to do so. But in this article, we will understand how we can use react-quill control to save the rich text field and also display it for the existing items.

react-quill is based on Quill which is trusted by Microsoft. To know more about the react-quill control please check the below [link](https://github.com/zenoamaro/react-quill).

So let us understand how we can use react-quill in our Custom SharePoint List Form.

**Step 1 - Create a List**

Before creating a Custom form we would create a SharePoint list for saving the data.

List Name:- Custom RichText

Columns:- Title(Single Line of Text)

Description(Multi-Line of Text in Plain Text format)

**Note**

We are using Plain Text but we will save the HTML in it because the images in React quill will be converted in base64 and will get truncated in Rich Text format.

**Step 2 - Create SPFx Solution**

Considering we have the environment setup completed we can use the below command to create new SPFx solution. We will use React framework for creating the Webpart
```
yo @microsoft/sharepoint  
```
Use the below values when asked while creating the solution.

![Rich Text Control With Image And Video In SPFx Using React-Quill](https://f4n3x6c5.stackpathcdn.com/article/rich-text-control-in-spfx-using-react-quill/Images/Rich%20Text%20Control%20With%20Image%20And%20Video%20In%20SPFx%20Using%20React-Quill.jpg)

**Step 3 - Add the reference for react-quill**

To add a reference to React quill in our project we need to use the below command which would install the required dependencies in our solution.
```
npm install react-quill --save  
```
Saving the data we would use PnPJs so let us install the required dependencies for PnPJs.
```
npm install @pnp/sp --save  
```
![Rich Text Control With Image And Video In SPFx Using React-Quill](https://f4n3x6c5.stackpathcdn.com/article/rich-text-control-in-spfx-using-react-quill/Images/Rich%20Text%20Control%20With%20Image%20And%20Video%20In%20SPFx%20Using%20React-Quill1.jpg)

**Step 4 - Update the WebPart Code**

Now that we have all the dependencies added in our solution we can update the code.

Let us set up the sp object so that we can use it to save the list item and retrieve it.

  
Open the file name named RichTextFieldWebPart.ts.

Import sp using the below code.
```
import { sp } from "@pnp/sp/presets/all";  
```
And add the below function to setup sp object.
```
protected onInit(): Promise<void> {  
   return super.onInit().then(_ => {  
      sp.setup({  
         spfxContext: this.context  
      });  
   });  
}  
```
Open the file named RichTextField.tsx to update the code for the rich text field.

Let us add the required reference for React quill using the below code.
``
import ReactQuill from 'react-quill';  
import 'react-quill/dist/quill.snow.css';  
``
Let us now create and  add required interfaces.
```
export interface ICustomRichText {  
    Title: string;  
    Description: string;  
}  
export interface IRichTextFieldWebPartState {  
    item: ICustomRichText;  
}  
```
Now for using react quill we can use custom toolbar so we would add all the required toolbar using the below constant:
```
const modules = {  
    toolbar: [  
        [{  
            'header': [1, 2, 3, false]  
        }],  
        ['bold', 'italic', 'underline', 'strike', 'blockquote'],  
        [{  
            'color': []  
        }, {  
            'background': []  
        }],  
        [{  
            'list': 'ordered'  
        }, {  
            'list': 'bullet'  
        }, {  
            'indent': '-1'  
        }, {  
            'indent': '+1'  
        }],  
        ['link', 'image', 'video'],  
        ['clean']  
    ],  
};  
const formats = ['header', 'bold', 'italic', 'underline', 'strike', 'blockquote', 'list', 'bullet', 'indent', 'link', 'image', 'video', 'background', 'color'];
```
Now in the render method of the component, we will have to add the below code which contains one text field and one React quill text editor:
```
<div className={styles.column}>    
    
<div>Title</div>    
   <input type="text" value={this.state.item.Title} onChange={(ev) => {    
   this.setState({ item: { Title: ev.target.value, Description: this.state.item.Description } });    
   }}></input>    
</div>    
<div className={styles.column}>    
    
<div>Description</div>    
   <ReactQuill theme="snow"    
      modules={modules}    
      formats={formats}    
      value={this.state.item.Description}    
      onChange={(ev) => {    
      this.setState({ item: { Title: this.state.item.Title, Description: ev } });    
   }}    
   ></ReactQuill>    
</div>    
<div className={styles.column}>    
<button onClick={this.saveClick}>Save Item</button>    
</div>  
```
For saving the data and all the on change functions we need to add the below code:
```
constructor(props: IRichTextFieldWebPartProps, state: IRichTextFieldWebPartState) {  
    super(props);  
    var queryParameters = new UrlQueryParameterCollection(window.location.href);  
    if (queryParameters.getValue("itemid")) {  
        const id: number = parseInt(queryParameters.getValue("itemid"));  
        sp.web.lists.getByTitle('Custom RichText').items.getById(id).get().then((val: ICustomRichText) => {  
            this.setState({  
                item: {  
                    Title: val.Title,  
                    Description: val.Description  
                }  
            });  
        }).catch((err) => {  
            console.error(err);  
        });  
    }  
    this.state = {  
        item: {  
            Title: "",  
            Description: ""  
        }  
    };  
}  
public saveClick = () => {  
    sp.web.lists.getByTitle('Custom RichText').items.add(this.state.item).then((val) => {  
        alert('Item Saved');  
    }).catch((err) => {  
        console.error(err);  
    });  
}  
```
We are also fetching the item with the query string value of itemid to display the value of the existing saved item.

So once all of the code is updated we can test our Webpart which would save the data and also display the saved item. We can save images videos and all text formatting which are available.

**Outcome**

![](https://f4n3x6c5.stackpathcdn.com/article/rich-text-control-in-spfx-using-react-quill/Images/outcome-min.gif)

Conclusion
----------

For rich text field in SPFx react quill can be one of the best controls which can be used for saving the rich text field.

  
This will even help us save images and videos in the rich text field in SharePoint List.