# Sending verification OTP to user-email with mailgun in a React, Node and Mongo App

Source Code of the project in Github. Note the branch containing this functionality is named “sending-otp-with-mailgun”. The master branch…

![](https://cdn-images-1.medium.com/max/1200/1*Pjex8k-MHhAeKiHZMBLyYQ.jpeg)
undefined

[**Source Code of the project in Github**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/tree/sending-otp-with-mailgun). Note, in this Github repo the branch containing this functionality is named “**sending-otp-with-mailgun**”. The **master** branch of this same repo has the same app but without the OTP sending feature.

(Note the repo will ONLY work after proper AWS and mailgun credentials (API key, Secret Key etc) are are set up in the .env file and the .env.override file).

Often we see in many site, that to get a download link or viewing a file, I first need to fill a form with my email and name etc and only then either the download link will be sent to that email, or an unique code will be sent first to that email, and then I have to put that code back to the site, and only then the required document will be rendered.

In this simple implementation, I have a React, Node, Mongo App running, where the user uploads file, which is saved in AWS-S3 and then user can view/download or delete the file or can edit the description text of the file. See my [**previous blog**](https://medium.com/@paulrohan/file-upload-to-aws-s3-bucket-in-a-node-react-mongo-app-and-using-multer-72884322aada) on how to build this project.

Here in this post, starting with the above app, I will implement this feature. When an user wants to view or download a file, he/she first needs to be verified by sending an OTP to their email > then he puts that OTP back to the App > only if the OTP matches to what was sent to his email the App render the link for the file.

To implement the above, in my app, the flow of this process is as follows

When in the main page, the user clicks on the View/Download a Material-UI Dialog will open > User types in the email and click Submit > this onSubmit event will generate a random string (using “[shortid](https://www.npmjs.com/package/shortid)” npm package) and will send an email to the one that user just has typed in the modal — at the same time node backend code will also save that OTP in mongo database > and then it will open the second Material-UI Dialog for the user to input the unique string that he has received in his email > after he inputs the same, and clicks on the submit button > my node backend code will check if this code submitted is the one that was saved in Mongo when user clicked on the first Dialog > and if its same the document link is rendered.

As the first step, to build this functionality, create your own account in [**mailgun**](https://signup.mailgun.com/new/signup). They have free scheme under which I guess you can send 10,000 free emails per month. During the signup process putting Credit Card credentials is only optional. And if you dont want to put your Credit Card credentials, then after creating the account, manually set up **Authorized Recipients (i.e. a list of emails)** who can receive emails from your mailgun credentials. But if you put your Credit Card credentials you dont have to setup this **Authorized Recipients.** Note, I have install [**mailgun-js npm package**](https://www.npmjs.com/package/mailgun-js) to be used along with mailgun.

And then set up your .env file in the project root with your own AWS and mailgun credentials with the below format.

![](https://cdn-images-1.medium.com/max/800/1*E9HwVLbXngyDF5RqO6XsEw.png)
undefined

Then in the backend create the [MongoDB schema for saving the OTP](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/models/OTP.js)

![](https://cdn-images-1.medium.com/max/800/1*AxwDduSfxnsQNfjNSe95yQ.png)
undefined

Now in my backend Express code in [**fileUploadRoutes.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/fileUploadRoutes.js), for the two routes to “/sendotptovisitor" (will be hit when user clicks on the first Dialog to view a file) and “/visitor/:id” (will be hit when user clicks on the second Dialog to input the OTP) are as below

![](https://cdn-images-1.medium.com/max/800/1*MflgKE5M_3NNHHilVznOQA.png)
undefined

In the above the reason I need the wrapper function **findLatestOTP** — is because of Node’s asynchronous nature of execution. If I directly do the mongdb query and assign the latest OTP of the user inside the mongoose .find() function without using the wrapper like below

![](https://cdn-images-1.medium.com/max/800/1*iNcWn51QKfgg_oAUOQ7adg.png)
undefined

Then the latestOtp will not be assigned the correct value and will retain its empty array value for the next piece of code.

To check that, I put a <console.log(latestOtp) > after the above code (i.e. the code without the wrapper function), and it will print the initial value of latestOtp, which is an empty array.

This is because, in Node.js all I/O operations are performed asynchronously. Since the database query requires a disk read and doesn’t require any CPU time, node goes ahead and uses this time to execute the next bit of code while it waits for the result of the database query to run.  
In this case the next bit that gets executed is that console.log, and it takes the initial value of latestOtp. So the way to deal with this situation is to write a seperate function (findLatestOTP) and the second argument of this function is the one.

Code in the [**mailSender.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/mailSender.js) file (below image) takes care of actually sending the email and [**mailCompose.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/mailCompose.js) ([along with mailgen npm package](https://www.npmjs.com/package/mailgen)) file composes the email, meaning what will be the content of the email . Refer to [**mailgun’s API**](https://documentation.mailgun.com/en/latest/quickstart-sending.html#send-with-smtp-or-api) for understanding this better.

![](https://cdn-images-1.medium.com/max/800/1*vwQamxqTjSKOXiD_v16vYA.png)
undefined

And in the above within the “/sendotptovisitor” route, I send the email with the line [**mailSender.sendOTPToVisitor(visitorEmail, newGeneratedOTP)**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/808f2e30f4a0dac85582c17af694a326db92b20a/routes/fileUploadRoutes.js#L203)

In the above [**mailSender.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/routes/mailSender.js) file, I am using the [**async package**](https://www.npmjs.com/package/async) (one of the popular and most widely used tools in the Node.js community) and [**async.waterfall**](https://caolan.github.io/async/docs.html#waterfall) method. Lets understand it in more detail.

In waterfall, we pass array of functions to [**async.waterfall()**](https://caolan.github.io/async/docs.html#waterfall). All the functions are executed in series and output of each function is passed to the next function. In case if any error is passed to the function’s callback, then main callback function will be invoked by skipping rest of functions in the queue. That is, In case if we pass error object to any function callback as shown below, we get main callback executed.

The `waterfall` method takes two parameters:

```
async.waterfall([], function (err) {});
```

*   The first argument is an array and the second argument is a function.
*   Using the first argument (i.e. the array), you can specify what you want to run in order.
*   Using the second argument (i.e. the function), you can catch any errors that happens in any of the steps.

```
async.waterfall([  function firstStep(done) {    done(null, 'Value from step 1');  },  function secondStep(previousResult, done) {    console.log(previousResult);    done(null);  }],function (err) {});
```

Every step function takes two arguments, except the first one. The first step function only takes one argument. Using the arguments, you can access the result of the previous step and also invoke the next step.

Notice, in **firstSetp()** function I am calling the `done` function with two arguments: the first argument is any error that we want to pass to the next step, and the second argument is the actual result or value that we want to pass to the next step. I have set error values to `null`, because for now, we don't really care about the errors. Hopefully, it should make more sense why the first step function takes one parameter. It's because nothing has been executed before the first function, so there are no results to be passed onto the first function.

To understand lets take a look at the below example code

![](https://cdn-images-1.medium.com/max/800/1*WS6z4Kj1QUVUJCZVr7JqhQ.png)
undefined

And the output I will get from above is

**_Program Start  
First Step →   
Second Step → 1 2  
Third Step → 3  
Main Callback → final result  
Program End_**

So, in my case here, withing the **async.waterfall** method in **mailSender.js** file, first [**generateTemplate()**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/808f2e30f4a0dac85582c17af694a326db92b20a/routes/mailSender.js#L23) function gets executed and its output from its callback() is passed to the next function [**sendMail()**.](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/808f2e30f4a0dac85582c17af694a326db92b20a/routes/mailSender.js#L30)

Now done with backend, and only thing left to do is the React front end. Most of the jobs about the taking user’s email and sending him the email are performed by the [**VisitorModal.js**](https://github.com/rohan-paul/aws-s3-file_upload-node-mongo-react-multer/blob/sending-otp-with-mailgun/src/Components/VisitorModal.js) component. Here, I am using Material-UI Dialog to open two sequential Dialogs, first one for capturing the user email and the second one of capturing OTP sent to that email.

By [Rohan Paul](https://medium.com/@paulrohan) on [December 23, 2018](https://medium.com/p/56ba7e4ac29).

[Canonical link](https://medium.com/@paulrohan/sending-verification-otp-to-user-email-with-mailgun-in-a-react-node-and-mongo-app-56ba7e4ac29)

Exported from [Medium](https://medium.com) on December 12, 2020.