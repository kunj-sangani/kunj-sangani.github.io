---
weight: 4
title: "Update Profile Picture Using Graph API In SPFx"
date: 2021-08-11T21:57:40+08:00
lastmod: 2021-08-11T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Using Microsoft Graph API to update profile picture for all Microsoft 365 applications via SPFx webpart."

canonicalurl: "https://www.c-sharpcorner.com/article/update-profile-picture-using-graph-api-spfx/"

tags: ["Teams Tab", "TeamsFx", "Teams Toolkit"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

Microsoft 365 has a different application in its suite. To combine all these data for developers Microsoft came up with Graph API. This has helped developers to access data across applications in Microsoft 365. One such example which we would leverage today is to save profile picture which would reflect across all the applications in Microsoft 365.

In this article, we will create a SPFx webpart where user can select their profile picture and on click of the save button, the profile picture will be updated for all apps in Microsoft 365.

**Step 1 - Create a SPFx Solution**

To create SPFx solution, use the below command:
```
yo @microsoft/sharepoint
```
Provide values for different questions which are asked while creating the solution as displayed in the image below:

![Update Profile Picture using Graph API SPFx](https://f4n3x6c5.stackpathcdn.com/article/update-profile-picture-using-graph-api-spfx/Images/Update%20Profile%20Picture%20using%20Graph%20API%20SPFx.jpg)

**Step 2 - Install fluent UI**

To use the icon for the file selection and deleting the file, we use the fluent UI icon. Use the below command to install it.
```
npm install @fluentui/react --save
```
**Step 3 - Add Graph API Scopes**

Open the solution in your choice of code editor.

  
Open the file name package-solution.json which would be present inside the config folder.

Add the below code after the line "isDomainIsolated": true:
```
"webApiPermissionRequests": [    
   {    
      "resource": "Microsoft Graph",    
      "scope": "User.ReadWrite"    
   }    
]  
```
**Step 4 - Add the Code for Uploading the Image as a Profile Picture**

To start updating the code, we are required to access context in our component. To do so, we need to update two files:

1.  UpdateProfilePictureWebPart.ts
2.  IUpdateProfilePictureProps.ts

Below is the code update that we need to do in the above-mentioned files:

In IUpdateProfilePictureProps.ts, replace the below code in the file:
```
import { WebPartContext } from "@microsoft/sp-webpart-base";    
    
export interface IUpdateProfilePictureProps {    
   description: string;    
   context:WebPartContext;    
}   
```
In UpdateProfilePictureWebPart.ts, update the render method pass additional parameter named context to UpdateProfilePicture component. Hence, the final render method will look as mentioned below:
```
public render(): void {  
    const element: React.ReactElement < IUpdateProfilePictureProps > = React.createElement(UpdateProfilePicture, {  
        description: this.properties.description,  
        context: this.context  
    });  
    ReactDom.render(element, this.domElement);  
}   
```
Now let us update the component. To update the component, we need to update the file named UpdateProfilePicture.tsx.

Replace it with the below code. In the below code we are using:  

*   HTML file picker which is hidden but is called in click on the icon button
*   On Selection of the file, we would display a delete icon and display the selected image as with the help of URL.createObjectURL() which would help us get the blob value of the file
*   On a click of "Save as Profile Picture," we are uploading the file with the help of MSGraphClient

For displaying a success or error message in the dialog box:
```
import * as React from 'react';  
import styles from './UpdateProfilePicture.module.scss';  
import { IUpdateProfilePictureProps } from './IUpdateProfilePictureProps';  
import { IconButton, PrimaryButton } from '@fluentui/react/lib/Button';  
import { MSGraphClient } from "@microsoft/sp-http";  
import { Dialog, DialogFooter } from 'office-ui-fabric-react/lib/Dialog';  
  
export interface IUpdateProfilePictureState {  
  imageFile: FileList;  
  open: boolean;  
  savedMessage: string;  
  savedMessageTitle: string;  
}  
  
export default class UpdateProfilePicture extends React.Component<IUpdateProfilePictureProps, IUpdateProfilePictureState> {  
  
  constructor(props: IUpdateProfilePictureProps) {  
    super(props);  
    this.state = {  
      imageFile: null,  
      open: true,  
      savedMessage: "",  
      savedMessageTitle: "",  
    };  
  }  
  
  
  private saveAsProfile = () => {  
    var reader = new FileReader();  
    var tempfile = this.state.imageFile[0];  
    this.props.context.msGraphClientFactory  
      .getClient().then((client: MSGraphClient) => {  
        client  
          .api("me/photo/$value")  
          .version("v1.0").header("Content-Type", tempfile.type).put(tempfile, (err, res) => {  
            if (!err) {  
              this.setState({  
                open: false, savedMessage: "Your profile picture is updated",  
                savedMessageTitle: "Profile Picture Saved"  
              });  
            } else {  
              this.setState({  
                open: false, savedMessage: err.message,  
                savedMessageTitle: "Error while saving Profile Picture"  
              });  
            }  
          });  
      });  
  }  
  
  private closeDialog = () => {  
    this.setState({ open: true });  
  }  
  
  
  public render(): React.ReactElement<IUpdateProfilePictureProps> {  
    return (  
      <div className={styles.updateProfilePicture}>  
        <div className={styles.container}>  
          <div className={styles.row}>  
            <label>Select File</label>  
            <input id="fileUpload" type="file" accept="image/*" style={{ display: "none" }}  
              onChange={(ev) => {  
                console.log(ev.target.files);  
                if (ev.target.files.length > 0) {  
                  this.setState({ imageFile: ev.target.files });  
                } else {  
                  this.setState({ imageFile: null });  
                }  
              }}></input>  
            <IconButton iconProps={{ iconName: 'PictureFill' }} title="PictureFill" ariaLabel="PictureFill"  
              style={{ display: this.state.imageFile ? "none" : "block" }}  
              onClick={() => {  
                document.getElementById('fileUpload').click();  
              }} />  
          </div>  
          {this.state.imageFile && <div className={styles.row}>  
            <div style={{  
              height: 250, width: 250,  
              display: "inline-block",  
              backgroundSize: "Cover", backgroundRepeat: "no-repeat",  
              backgroundPosition: "center",  
              backgroundImage: `url("${URL.createObjectURL(this.state.imageFile[0])}")`  
            }}></div>  
            <IconButton iconProps={{ iconName: 'Delete' }} title="Delete" ariaLabel="Delete"  
              onClick={() => {  
                this.setState({ imageFile: null });  
              }} />  
            <div className={styles.row}>  
              <PrimaryButton text="Save as Profile Picture" onClick={() => { this.saveAsProfile(); }} allowDisabledFocus />  
            </div>  
          </div>}  
          <Dialog  
            hidden={this.state.open}  
            onDismiss={this.closeDialog}  
            dialogContentProps={{ title: this.state.savedMessageTitle, subText: this.state.savedMessage }}  
          >  
            <DialogFooter>  
              <PrimaryButton onClick={this.closeDialog} text="close" />  
            </DialogFooter>  
          </Dialog>  
        </div>  
      </div>  
    );  
  }  
}     
```
**Step 5 - Approve the Graph Request in API Management**

To use Graph API, we need to approve the scope that we are using from SharePoint Admin Portal to do so.  

1.  Navigate to SharePoint Online Admin Portal
2.  On the left Navigation open Advance section and click on API Access.

Click on Approve, as displayed in the image below:

![](https://f4n3x6c5.stackpathcdn.com/article/update-profile-picture-using-graph-api-spfx/Images/ApproveGraph.png)

**Outcome**

![](https://f4n3x6c5.stackpathcdn.com/article/update-profile-picture-using-graph-api-spfx/Images/2021-08-09-17-12-41-online-video.gif)

Conclusion
----------

To update images across all the applications in Microsoft 365, graph API comes to rescue and we can create an a SPFx webpart which can be accessed by all the users in the organization and update their profile picture.