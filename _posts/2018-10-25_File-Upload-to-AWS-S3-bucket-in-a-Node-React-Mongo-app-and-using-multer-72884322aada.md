---
layout: post
title: GIT
description: >-
  8th week running @ The Hacking School and one of the thing we are learning
  this week is metaprograming, and Proxy is a major concept within…
comments: true
author: Rohan Paul
categories: []
keywords: []
slug: /@paulrohan/git
---


# File Upload to AWS S3 bucket in a Node-React-Mongo app and using multer

Full Source code of the final working app (both the front and backend) —…

![](https://cdn-images-1.medium.com/max/1200/1*mxhWy2lwPrRLIff5Kh9YTg.jpeg)
undefined

undefined
undefined

Final Source code — [**https://github.com/rohan-paul/aws-s3-file\_upload-node-mongo-react-multer**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer)

This quick tutorial will get you started using Amazon AWS by building an app with Node, React and MongoDB that uploads and reads and reder files from AWS S3 (Simple Storage Service) — one of the most regular feature of any web-app.

**The Key Steps**

*   Create a bucket in AWS S3 which will store my static files.
*   Set up the backend api or the endpoints with Node and Express that will access the AWS S3 bucket via the SDK.
*   Create two simple React components, one to upload a single file to AWS-S3 and the other to just render the uploaded files in a table.

For creating the S3 bucket follow their official quick-start guide, its really fast and simple (assuming I have a fully acitvated AWS account setup before).

[**Step 1: Create an Amazon S3 Bucket - AWS Quick Start Guide: Back Up Your Files to Amazon Simple…**
_First, you need to create an Amazon S3 bucket where you will store your objects._docs.aws.amazon.com](https://docs.aws.amazon.com/quickstarts/latest/s3backup/step-1-create-bucket.html "https://docs.aws.amazon.com/quickstarts/latest/s3backup/step-1-create-bucket.html")[](https://docs.aws.amazon.com/quickstarts/latest/s3backup/step-1-create-bucket.html)

In this write-up I am not going through each of the steps and file details like installing all the extra packages or setting up the server configuration in my app.js file, because I am given above the final link for the source code.

### 1\. Create a bucket in AWS S3

Nevigate to [https://s3.console.aws.amazon.com/s3/home?region=us-east-1](https://s3.console.aws.amazon.com/s3/home?region=us-east-1) click on “create bucket”, enter a name, choose a region, and leave all other options as default.

![](https://cdn-images-1.medium.com/max/800/1*AmbfkYId2h0aLKVR48_C4A.png)
undefined

After you get the **Access ID and the Secret access key** copy them to somewhere safe as you won’t be able to see that again.

In my app here, I have the used a **.env file** to keep all these secret keys and using the dotenv node package ([https://www.npmjs.com/package/dotenv](https://www.npmjs.com/package/dotenv)) to manage my env variables and secret credentials. Basically this module loads environment variables from a .env file that you create and adds them to the process.env object that is made available to the application. **Just remeber NEVER EVER commit the .env file to Github. They also strongly recommend against committing your** `**.env**` **file to any version control. If you do, you will receive an email from Github and also from AWS informing you about this security breach in your account.**

![](https://cdn-images-1.medium.com/max/800/1*SWsmqr7ngHEFbYuibUo-Zg.png)
undefined

**Make the AWS-S3 bucket public (after creating the bucket) — whithout which the upload will NOT work**

If you’re using an Amazon S3 bucket to share files with anyone else, you’ll first need to make those files public. Maybe you’re sending download links to someone, or perhaps you’re using S3 for static files for your website or as a content delivery network (CDN). But if you don’t make the files public, your users will get an XML error message saying the file is unavailable.

1\. Sign in to Amazon Web Services and go to your S3 Management Console.

2\. Select the bucket from the left. At right, click the Properties button if it’s not already expanded.

3\. Go to the Permissions tab > Public Access Settings Tab

4\. Click on Edit > Then

> A) Block new public ACLs and uploading public objects (Recommended)— Make it false (i.e. uncheck it)

> B) Block new public bucket policies (Recommended) — Make it false (i.e. uncheck it)

> C) Block public and cross-account access if bucket has public policies (Recommended) — Make it false (i.e. uncheck it)

> D) Block public and cross-account access if bucket has public policies (Recommended) — Make it false (i.e. uncheck it) — Without this last action, althouh I was able to upload BUT was NOT able to download.

So all the four options that was there (as on 24-Nov-2018) — I made all of them false.

2\. Building the node backend

I am first building a boilerplate with create-react-app ([https://github.com/facebook/create-react-app)](https://github.com/facebook/create-react-app) and to which I will add the backend code.

So run the below command to initiate the project.

> **npx create-react-app aws-s3-file\_upload-node-mongo-react-multer**

After it finishes running and installs all the packages, create a file in ./bin/www with the below contents.

undefined
undefined

The codes above is quite a common for setting up the port and creating the server. Also sets the functions to be called if there is an error while starting the server. The **onError** function.

Then set up the [**app.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/master/app.js) file which my main configuration file for the server. and the [**Mongodb data model file**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/master/models/Document.js)**.**

Next comes main [**route file**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/master/routes/fileUploadRoutes.js) where I have all the codes for hitting AWS S3 bucket and uploading the file.

undefined
undefined

Once the client posts to the server a file to be uploaded to AWS-S3 along with other relevant meta info associated with the file, the server has to do the next part of the job of handling that binary file. **I am using multer node package for for this job.**

Multer ships with two storage engines **DiskStorage and MemoryStorage.** The difference is with DiskStorage, we’re configuring multer to use a local /files/ directory to store uploaded files from the client. It’s important to hold the file somewhere before we send it to REST API, because in most cases we’ll have to provide full path to the files. However, another option is to hold it in memory.

The **memoryStorage** engine stores the files in memory as Buffer objects. But how to release the memory after uploading the file ? As soon as you are done with the request, the memory will be freed. Taking a look at the source code of multer to understand how multer handles the memory storage [multer/storage/memory.js (github)](https://github.com/expressjs/multer/blob/eef091188d3f2c40d0da145d75114434b2b3f840/storage/memory.js)

![](https://cdn-images-1.medium.com/max/800/1*LChwJirXU0yQbkDW0fLdIQ.png)
undefined

So the file is simply being streamed to the callback (cb), so there is no global references that would keep it from living in memory (no references to it = not needed anymore = garbage = GC). The memoryStorage is mainly for handling the file buffer in a temporary way. Which makes it a good usecase here : I want to receive small files and relay them to another server which stores the media. (multer receives stores in memory temp, relays and frees the memory by itself once theres no reference to it anymore).

Lets understand the most important piece of the code which is the upload route.

![](https://cdn-images-1.medium.com/max/800/1*5YVmSa_fpodVxPDn2mVgXA.png)
undefined

So the above function is of the following form.

![](https://cdn-images-1.medium.com/max/800/1*wYpBbRlEtYnNoANUHpTkaA.png)
undefined

Its creating the actual backend route where the client sends FormData and where I am passing multer middleware. When this backend route (or API) receives a file, it goes through the middleware first and is stored with memoryStorage in the buffer. If I was using diskstorage, here in the middleware, I will store it in the file-system of the project.

Then I get to the callback\_function, and by now the file is available as part of req object. From here, I can do whatever is needed, in this case, calling myexternal API (could be S3 or any API that handles files) and passing file and meta info.

I am defining S3 server **Bucket** name, **accessKeyId** and **secretAccessKey** from my Amazon S3 account. Then I shall handle file upload using **post** method.

ContentType: file.mimetype,

ACL: "public-read"

The above two configurations in the **params** variable (that are passed to s3bucket.upload function), makes the file to take its own mimetype when being saved to s3 bucket and makes it available for public viewing. So, if I dont set the mimetype when uploading, after the file is uploaded and I click on the view file option from the front-end it will not be rendered on the browser, and I will only be given the option to download the file. And not setting the ACT property will give me “Access Denied” when clicking on the file.

After the file is uploaded successfully to S3, I am also saving this file’s information in my Mongodb database which will keep a track of the description of the file (that the user can edit anytime) and also the link of the file in S3 for viewing by user.

I can test the upload API from Postman by selecting **_\`_**[**_http://localhost:3000/api/document/upload\`_**](http://localhost:3000/api/document/upload`) and > POST > Body > form-data > under Key type “file” and under Value select file and choose a file.

And I will get back a 200 OK response of the below form-data

{
“data”: {
“ETag”: “a number”,
“Location”: “full link of the file”,
“key”: “original file name of the file that I uploaded”,
“Key”: “original file name of the file that I uploaded”,
“Bucket”: “my AWS s3 bucket name”
}

#### [To display the uploaded file when I click on “view-file” link in the front-end](https://stackoverflow.com/questions/32702431/display-images-fetched-from-s3)

Here, I am just building the uploaded file’s link quite simply, by just adding the file name to the URL schema that AWS-S3 gives to the file.

If my S3 file is public (I have to change it inside AWS-S3 by selecting the bucket and applying an action, by default it isn’t public) I can get the url from this schema:

```
https://s3-<region>.amazonaws.com/<bucket-name>/<key>
```

So if the region is **eu-west-1** with something like this:

```
'https://s3-eu-west-1.amazonaws.com/mybucket/myimage.jpg';
```

Get all the regions names and respectived codes here — [https://docs.aws.amazon.com/general/latest/gr/rande.html](https://docs.aws.amazon.com/general/latest/gr/rande.html)

In my case, I am fetching this from the **.env file** which has the setup per the above scheme like below.

![](https://cdn-images-1.medium.com/max/800/1*1JGjOcgZEe6ps-7n9JqOAw.png)
undefined

Another way I could get the link of the uploaded file is by using getSignedUrl

![](https://cdn-images-1.medium.com/max/800/1*5CYbNEBlamfI1Lw2JR-jtw.png)
undefined

The **getSignedUrl** method takes an operations, a params, and a callback function as arguments. The operation argument is a string that specifies the name of the operation to call, in this case **‘getObject**’. The [**‘getObject**](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getObject-property)’ request from the AWS S3 SDK returns a ‘data.Body’. The **urlParams** are parameters that take the Bucket name and the name of the key, in this case the file name. The callback function takes two arguments, error and url. The url is the string we would want to place in our file linking tag to point to the file in the respective front-end code (In this case my FileUpload.js React Component).

But the real benefit of using [**getSignedUrl**](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getSignedUrl-property) is — when I dont want to use the URL of the file, because if I want to load this file from its URL, the file MUST BE declared ‘public’, otherwise there’s a 403 (forbidden) error message. But when I am loading the file with an **‘s3bucket.getObject’**, with an **‘s3bucket’** declared and authenticated by my access keys. In this way, we can load a file declared private.

**And now the React front-end.**

Its quite straight-forward.

Where, I have three componets which takes care of Uploading a new File, rendering a file and editing the description text of the uploaded file. So basically each of the those components are making an axios request to the relevant API in the back end and rendering that in the page.

**Lets take a look at the handleSelectedFile() function in** [**NewFileUpload.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/master/src/Components/NewFileUpload.js)

![](https://cdn-images-1.medium.com/max/800/1*Z9p_jPJ6cXMXuwvuYu4seg.png)
undefined

`handleSelectedFiles()` function will be called by an event handler (`onChange` in this case) which passed this function. By invoking this `handleSelectedFiles()` function, when the user selects a file, the function set the state of **selectedFile** by accessing the `[FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList "An object of this type is returned by the files property of the HTML <input> element; this lets you access the list of files selected with the  element. It's also used for a list of files dropped into web content when using the drag and drop API; see the DataTransfer object for details on this usage.")`object containing `[File](https://developer.mozilla.org/en-US/docs/Web/API/File "The File interface provides information about files and allows JavaScript in a web page to access their content.")` objects representing the files selected by the user.

The `[FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList "An object of this type is returned by the files property of the HTML <input> element; this lets you access the list of files selected with the  element. It's also used for a list of files dropped into web content when using the drag and drop API; see the DataTransfer object for details on this usage.")` object provided by the DOM lists all of the files selected by the user, each specified as a `[File](https://developer.mozilla.org/en-US/docs/Web/API/File "The File interface provides information about files and allows JavaScript in a web page to access their content.")` object. You can determine how many files the user selected by checking the value of the file list's `length` attribute:

`var numFiles = files.length;`

An object of this `[FileList](https://developer.mozilla.org/en-US/docs/Web/API/FileList "An object of this type is returned by the files property of the HTML <input> element; this lets you access the list of files selected with the  element. It's also used for a list of files dropped into web content when using the drag and drop API; see the DataTransfer object for details on this usage.")` type is returned by the `files` property of the HTML `[<input>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input "The HTML <input> element is used to create interactive controls for web-based forms in order to accept data from the user; a wide variety of types of input data and control widgets are available, depending on the device and user agent.")` element; this lets you access the list of files selected with the `<input type="file">` element. **And All** `**<input>**` **element nodes have a** `**files**` **array on them which allows access to the items in this list**. For example, if the HTML includes the following file input: `<input id="fileItem" type="file">`

The following line of code fetches the first file in the node’s file list as a `[File](https://developer.mozilla.org/en-US/docs/DOM/File "DOM/File")` object: `var file = document.getElementById('fileItem').files[0];`

**And thats exactly what the handleSelectedFile() function is doing here. Its hooking the first file, after the user loads a file that is to be uploaded by using a standard** `**<input type="file">**` **element inside the form. JavaScript returns the list of selected** `**File**` **objects as a** `**FileList**`**.**

And the **event.target** property gets the element on which the event originally occurred, which in this case will return the element **<input>**. And we already know that **all** `**<input>**` **element nodes have a** `**files**` **array on them which allows access to the items in this list**.

Then the next imporant function in **in** [**NewFileUpload.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/master/src/Components/NewFileUpload.js) **is handleUpload()**

![](https://cdn-images-1.medium.com/max/800/1*G_pBqUCrY24jLs3kL6DvtQ.png)
undefined

Its taking care of the all-important part which is to hit the backend’s uploading API (or route) when the form in the React front-end is submitted by firing the onSubmit attribute. Then I am creating a new instance of the `[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)` object which will let me compile a set of key/value pairs to send using `[XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)` with axios.post request. Then I am appending to the object, values for fields named “file”, “state.selectedFile” and “state.description” to send the whole form data.

**Other resources and official doc references:**

[AWS-API References](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putObject-property)

[S3 Documentation](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html)

[AWS-SDK for Javascript in Node.js](http://aws.amazon.com/sdkfornodejs/)

[AWS examples using Node.js](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-examples.html)

[AWS.S3 methods documentation](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html)

By [Rohan Paul](https://medium.com/@paulrohan) on [October 25, 2018](https://medium.com/p/72884322aada).

[Canonical link](https://medium.com/@paulrohan/file-upload-to-aws-s3-bucket-in-a-node-react-mongo-app-and-using-multer-72884322aada)

Exported from [Medium](https://medium.com) on December 12, 2020.