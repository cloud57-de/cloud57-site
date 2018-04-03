---
title: "A Simple Text Editor For Google Drive"
date: 2018-04-03T18:45:09+02:00
draft: true
---

### Motivation
The Google Drive Platform provides some powerful tools (e.g. Docs) out of the box. But sometimes it is necessary to have a simple text editor for displaying and editing simple text files. Although there are several third party apps which do a great job of creating and editing simple text files (and which can also be easily integrated into Google Drive) CLOUD57 decided to create their own simple text editor for Google Drive. The motivation behind this decision was to learn about the Google drive API and how to integrate tools (such as a simple text editor) into Google Drive.  

The following aspects have to be covered:  
1. Find a web-based text editor as a basis which has more features than the Textarea-Tag of HTML5.  
2. Investigate the Google Client API how to sign in and investigate the Google Drive API how to load and save simple text content.  
3. Investigate Google Drive how to integrate web-based tools  
4. Select a proper framework to integrate all of the above into a project which can be build and tested locally before shipping  

### The Editor

Although the Textarea-Tag of HTML5 can be used for a simple text editor, it misses at least one important things: TAB-control. Sure, TAB-control can be implemented with vanilla javascript, but to cover all the details around TAB-controlling, it is easier to use an open-soruce tool that already handles everything a simple text editor needs. After some research CLOUD57 decided to use [ACE](https://ace.c9.io/).

### The Google Drive API
The Google Drive API supports creating, loading and saving of content.  

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
To perform the function, you need the id (folderid) of a Google Drive folder into which the new Google Drive object will be stored. If the function performs without an error, the response contains the id of the newly created Google Drive object. But this object has no content. One needs to upload/save the content (via a multipart request) after retrieving the id, just like this:  

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
The above example uses the variables boundary and multipartRequestBody which are not shown here because of clarity. For details have a look at the Github-Repository of [CLOUD57’s Drive Text Editor] (https://github.com/cloud57-de/drivetexteditor/).  

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
### Integration

To integrate a tool into Google Drive one have to use the Google G Suite developer console.
<br><br><br>
To Be Continued...  
<br><br>
