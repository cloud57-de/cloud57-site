---
title: "A Simple Text Editor For Google Drive"
date: 2018-04-03T18:45:09+02:00
draft: true
---

### Motivation
The Google Drive Platform provides some powerful tools out of the box (e.g. Docs). But sometimes it is necessary to have a simple text editor for displaying and editing simple text files. Although there are several third party apps which do a good job of creating and editing simple text files (and which can also be easily integrated into Google Drive) CLOUD57 decided to create their own simple text editor for Google Drive. The motivation behind this decision was to learn about the Google drive API and how to integrate tools (such as a simple text editor) into Google Drive.  

The following aspects had to be covered:  
1. Finding a web-based text editor which has more features than the Textarea-Tag of HTML5.  
2. Investigating APIs how to sign in into Google Drive and how to create, load and save simple text content.  
3. Investigating the Google Cloud Platform how to integrate web-based tools  

### 1. Editor

Although the Textarea-Tag of HTML5 can be used for a simple text editor, it misses at least one important thing: TAB-control. Sure, TAB-control can be implemented with vanilla javascript, but to cover all the details around TAB-controlling, it is easier to use an open-source tool that already handles everything a simple text editor needs. After some research [ACE](https://ace.c9.io/) seemed to be a good choice.

### 2. APIs
The [Google Drive API](https://developers.google.com/drive/v3/web/about-sdk) supports creating, loading and saving of content.  

To load the content of a Google Drive object one can perform a simple request with JQuery like this:  
```

    $.ajax({
        url: "https://www.googleapis.com/drive/v3/files/" + id + "?alt=media",
        headers: {
            "Authorization": "Bearer " + token
        }
    }).then(function(data){
        // do something
    }).catch(function(err){
        // do something
    });

```
<br>
In this request the id of the content element and an authorization token is needed. How to get these information will be shown later. The response to the request contains the content of the requested element (in our example the “pure” text of the requested text file, identified by the id).  

To create a new Google Drive object the Google Drive API provides a create function that can be used like this:  
```

    gapi.client.drive.files.create({
        "name": “my new element”,
        "mimeType": "text/plain",
        "fields": "id",
        "parents": [folderId]
    }).then(function(resp){
        var id = resp.result.id;
    	// do something
    }).catch(function(err){
    	// do something
    });

```
<br>
Note: *gapi.client.drive.files.create* is available via the [Google API Client Javascript Library](https://developers.google.com/api-client-library/javascript/start/start-js).  

To perform the function one needs the id (folderid) of a Google Drive folder into which the new Google Drive object will be stored. If the function performs without an error, the response contains the id of the newly created Google Drive object. But this object has no content. One needs to upload/save the content (via a multipart request) after retrieving the id, just like this:  

```

    gapi.client.request({
        "path": "/upload/drive/v3/files/" + id,
        "method": "PATCH",
        "params": {
            "fileId": id,
            "uploadType": "multipart"
        },
        "headers": {
            "Content-Type": "multipart/form-data; boundary='" + boundary + "'",
            "Authorization": "Bearer " + token
        },
        "body": multipartRequestBody
    }).execute(function(file){
    	// do something
    });

```
<br>
The above example uses the variables *boundary* and *multipartRequestBody* which are not shown here because of clarity of the code snippet. For details have a look at the [Github-Repository of CLOUD57’s Drive Text Editor] (https://github.com/cloud57-de/drivetexteditor/).  

In both examples above (loading and saving) an authorization token was used. This token can be achieved via the Google API Client library like this:  

```

    gapi.auth2.getAuthInstance().signIn().then(function(){
        var token = gapi.auth2.getAuthInstance()
                    .currentUser.get()
                    .getAuthResponse(true)
                    .access_token;
    }).catch(function(err){
        //do something
    });

```
<br>
### 3. Google Cloud Platform

To integrate a web-based tool into Google Drive one have to use the [Google Cloud Platform Developer Console](https://console.developers.google.com/).
Details on how to create a Google Cloud Project and how to activate certain Google APIs for the project could be found at [Googles Cloud Documentation](https://cloud.google.com/resource-manager/docs/creating-managing-projects) and will not be describes here.
<br>

### Try for yourself
To install the CLOUD57 Drive Text Editor just enter  
```

    https://drivetexteditor.cloud57.de/?state=installation

```
<br>
in you web browser and enable popups.  

After the installation has finished open Google Drive (or refresh the website if you've already opened it) and double click on a text document. This will open the CLOUD57 Drive Text Editor with the content of the selected text document.

Notice that a double click will open a new browser tab with an URL similar to the following one   

```

    https://drivetexteditor.cloud57.de/?state=%7B%22ids%22:%5B%22 id %22%5D,%22action%22:%22open%22,%22userId%22:%22 userid %22%7D

```
<br>
where *id* is the unique ID of the selected text document and *userid* is the user ID of the currently logged in user.

To experiment with the CLOUD57 Drive Text Editor just clone the [Github-Repository](https://github.com/cloud57-de/drivetexteditor/) and perform    

```

    npm run dev

```
<br>
This will build the project and start a local server.  

Entering the URL from above but replacing  

```

    https://drivetexteditor.cloud57.de

```
<br>

through  

```

 http://localhost:8080

```
<br>
will use the local server accessing Google Drive.  

**Attention:** It is important to be very careful with local changes because these changes could have direct effects on text documents in Google Drive. CLOUD57 is not responsible for any damage or other problems which result from experimenting with the Drive Text Editor as described above.

<br><br><br>
