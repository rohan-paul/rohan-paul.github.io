# Deploying a React-Node-MongoDB app to Google Cloud Platform’s Google App Engine

We will go through the steps of deploying a full-stack Node-Express-React-MongoDB applications to Google App Engine (GAE).

(Updated this post in January-2020)

![](https://cdn-images-1.medium.com/max/1200/1*e5t0qZZVMMac4tH8MR9G0Q.jpeg)
undefined

undefined
undefined

We will go through the steps of deploying a **full-stack Node-Express-React-MongoDB applications to Google App Engine (GAE).**

**Link to the Repo source code**— [https://github.com/rohan-paul/material-ui-table-with-node-mongodb](https://github.com/rohan-paul/material-ui-table-with-node-mongodb)

**Link to the deployed App running in Google App Engine** — [https://mui-table-3.appspot.com](https://mui-table-3.appspot.com)

Generally in this article I shall focus on deploying a prototype React-Node Application, an app which is more of a proof of concept and testing and development — means my focus would be not to exhaust too much of Google-Cloud resource and also limiting Cloud-cost during this protyping phase.

I built the app with create-react-app. And my mongodb is hosted in MongoAtlas.

#### Setting up MongoAtlas for hosting my database

I pretty much [**followed this**](https://cloud.google.com/community/tutorials/mongodb-atlas-appengineflex-nodejs-app) to set up a FREE new cluster in mongo-atlas choosing Google Cloud Platform, and then whitelisted everything (choosing ‘Allow Access from anywhere’ (which is not the most secured one though, however for my demo project this is acceptable risk). Please note when actually putting something into production, you will want to narrow the scope of where your database can be accessed and specify a specific IP address/CIDR block.

Then put MongoDB-Atlas’s link in the .env file of your project root for “MONGO\_DB”.

**Note choosing Google Cloud Platform (and NOT AWS or Azure), while creating the cluster in MongoAtlas is very important,** otherwise the hosted database will not be served properly in Google Cloud Platform.

![](https://cdn-images-1.medium.com/max/800/1*N3C1pp6-HdfcTPdj-YOyPQ.png)
undefined

During the MongoAtlas cluster creation process, in the Cluster Tier section, select the M0 option to create your free tier cluster. It offers 512 MB of storage space, a recent version of MongoDB with WiredTiger as the storage engine, a replica set of three nodes, and a generous 10 GB of bandwidth per week.

After creating a cluster, and before you start using the cluster, you’ll have to provide a few security-related details, so switch to the Security tab.

First, in the MongoDB Users section, you must create a new user for yourself by pressing the Add new user button. In the dialog that pops up, type in your desired username and password, select the Read and write to any database privilege, and press the Add User button.

Then under Security > Network Access > click on “Add IP Address”.

So, in my project root’s .env file I have a line like below for connection to Atlas

mongodb+srv://admin:<password>@clusterName\_Of\_Database-vofxj.gcp.mongodb.net/test?retryWrites=true

For getting the above connection string — You’ll need a valid connection string to connect to your cluster from your application. To get it, go to the **Overview** tab and press the **Connect** button.

In the dialog that opens, select the **Connect Your Application** option and press the **I’m using driver 3.6 or later** button. You should now be able to see your connection string. It won’t have your actual password, so you’ll have to put it in manually. After you do so, make a note of the string so you can use it later.

![](https://cdn-images-1.medium.com/max/800/1*IiIW9hT-5GX52ZgRR-03dg.png)
undefined

**As a first step, create an account with** [**Google Cloud Platform**](https://cloud.google.com/billing/docs/how-to/manage-billing-account)**.** You have to enter your Debit/Credit card info, just for validating, and you will not be charged for the free tier which expires in 12 months or when you exhaust the $300 credit. [They very clearly mention](https://cloud.google.com/free/docs/gcp-free-tier), _If you spend your entire $300 credit before the end of the 12-month period, your free trial ends at that time and your account is paused. Your credit card is not charged unless you upgrade to a paid account._ “_No autocharge after free trial ends We ask you for your credit card to make sure you are not a robot. You won’t be charged unless you manually upgrade to a paid account_.”

During the process of creating your free GCP billing account, you have to take a photo of your Debit/Credit Card showing only the last 4 digits of your card, and submit that photo along with the form. Important point is, till the Debit / Credit card is fully verified, the GCP account will not be activated and it will take around three to four hours. And in my case, after submitting my Debit/Credit Card info along with the photo and also the dummy Rs 1 transaction (that was to be reversed), some three hours later, I got an email from GCP Support that my account was suspended as verification could not be completed with the information I have already submitted. So the email requested me to also send them my credit card statement with only the last 4 digits of credit card number and name visible. So I did that too and only then contacted GCP support through a live chat system. And over the chat, they very promptly activated my billing account with the $300 free credit to start with. While this whole Debit/Credit Card verification process took some extra steps for me, I have to say, I was pretty much delighted by the fast and very helpful chat support that I received from GCP.

You will get to see your leftover credit at any time in billing dashboard like below

![](https://cdn-images-1.medium.com/max/800/1*3pmm_xQ5ckv0xu0YttaO-A.png)
undefined

Before deploying lets understand some basics how a React+Node app run together.

#### Preparing you Project and React Build folder for deployment

First, understand that there are two modes a React-Node app can run.

A> When running locally in localhost the app gets started with **<npm run dev>** which, (as per the configuration I have made in the project root’s package.json file), will in turn execute **npm run server** (which runs on port 8080) then **npm start — prefix client** which runs on port 3000.   
In other words **npm run dev** will concurrently run the server and then run the client on your browser.

And then,

B> When I am building for deployment (port 8080) via **npm run build** (which creates an index.html)

**Now lets build the React app for production**

When running **npm run start** your app will proxy from port 3000 to port 8080 automatically. However when you create a production build your entire app should be running from port 8080, as express and the API’s are running from the same server (in this setup).

According to [React Official guide](https://facebook.github.io/create-react-app/docs/deployment) — `**npm run build**` creates a `build` directory with a production build of your app. Set up your favorite HTTP server so that a visitor to your site is served `**index.html**`

So inside your React client directory run `**npm run build**` to creates a `build` directory. And when the build process finish, should actually say something like the following

```
The build folder is ready to be deployed.You may serve it with a static server:
```

```
npm install -g serveserve -s build
```

The build script is building your entire app into the build folder, **ready to be statically served.** However actually serving it require some kind of static file server, like the the one they propose (serve).

After running the command `**serve -s build**` you can access your production build at localhost (on the specified port).

You can of course run whatever static file server you like, in this project, I am using the most popular one express.

Basically, what this will do under the hood is, for my entire React front-end UI having probably 30/40/50 .js and .css / .scss files, I wll serve a single static file called **/build/index.html**. But you can’t just open this `index.html` to directly run the front-end because this file is supposed to be **served with a static file server**.  
This is because most React apps use client-side routing, and you can’t do that with `file://` URLs.

**In production**, you can use Nginx, Apache, Node (e.g. Express), or any other server to serve static assets. Just make sure that if you use client-side routing, you serve `index.html` for any unknown request, like `/*`, and not just for `/`

**Under the hood, Create React App uses Webpack with** [**html-webpack-plugin**](https://www.npmjs.com/package/html-webpack-plugin)**.**

Create-React-App’s configuration [specifies](https://github.com/facebookincubator/create-react-app/blob/21fe19ab0fbae8ca403572beb55b4d11e45a75cf/packages/react-scripts/config/webpack.config.prod.js#L68-L70) that Webpack uses `src/index.js` as an [“entry point”](https://webpack.js.org/concepts/#entry). So that’s the first module it reads, and it follows from it to other modules to compile them into a single bundle.

When webpack compiles the assets, it produces a single (or several if you use code splitting) bundles. It makes their final paths available to all plugins. We are using one such plugin for injecting scripts into HTML.

CRA has enabled [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin) to generate the HTML file. In CRA’s configuration, they [specified](https://github.com/facebookincubator/create-react-app/blob/21fe19ab0fbae8ca403572beb55b4d11e45a75cf/packages/react-scripts/config/webpack.config.prod.js#L233-L235) that it should read `public/index.html` as a template. CRA has also set `inject` [option](https://github.com/jantimon/html-webpack-plugin#configuration) to `true`. With that option, `html-webpack-plugin` adds a `<script>` with the path provided by Webpack right into the final HTML page. This final page is the one you get in `build/index.html` after running `npm run build`, and the one that gets served from `/` when you run `npm start`

For more details [refer to this guide.](https://facebook.github.io/create-react-app/docs/production-build)

So in my case my [**Express server had this index.js**](https://github.com/rohan-paul/material-ui-table-with-node-mongodb/blob/master/index.js) file which will server the static React file inside /client/build/index.html as follows.

![](https://cdn-images-1.medium.com/max/800/1*uJ8L_vkbtl6JcZ-ZEsMJbw.png)
undefined

**Very important point above that the “catchall” route (/\*) should be at the bottom most postion after the rest of the routes. Else, a race condition will prevail and the app will not work as expected.**

The above pretty much means is that for any routes after “/” serve the single compiled react’s index.html from /client/build/index.html

#### _The package.json in the root of your Project_

Make sure that your _package.json_ contains a “start” script. After App Engine instances start , it will run the this script. Don’t forget to start your server with something like _node server.js or nodemon server.js. or nodemon index.js_

Below is the crucial part from my [**package.json at the project root.**](https://github.com/rohan-paul/material-ui-table-with-node-mongodb/blob/master/package.json)

![](https://cdn-images-1.medium.com/max/800/1*GpO0IniuIkhr5yjFivExqA.png)
undefined

#### Create a New Project in Google App Engine

Log into the [GCP console](http://cloud.google.com), and create a new project!

![](https://cdn-images-1.medium.com/max/800/1*Xbm3IR_xclxixoa3AvVwkA.png)
undefined

Note, you cannot change project ID.

You have an option to use a custom domain, e.g. [www.myCompany.com](http://www.myCompany.com), in which case projectID is something that only your internal code needs to know.

For adding a custom domain for your application  
In the Google Cloud Console, go to App Engine > Settings > Custom Domains >Click Add a custom domain to display the Add a new custom domain form and then follow the steps. The offical documentation is [**here**](https://cloud.google.com/appengine/docs/standard/python/mapping-custom-domains)**.** But in this case the verifications involved will test you to prove that you own that custom domain. You’ll end up on Webmaster Central which is Google’s automated domain name verification tool. If you do not yet have a custom domain name, buy one from a domain name registrar before doing this step in GCP.

#### Setting up app.yaml file

Create an **app.yaml** file in your project root, which lets App Engine know what configuration you desire.

If you have any _process.env_ variables, this is where they should be specified by using a parameter named “env\_variables”. For a full list of what this file can contain, read on [here](https://cloud.google.com/appengine/docs/flexible/nodejs/configuring-your-app-with-app-yaml).

[**Here is my app.yaml file**](https://github.com/rohan-paul/material-ui-table-with-node-mongodb/blob/master/app.yaml)

![](https://cdn-images-1.medium.com/max/800/1*XAACMi1Z7SFBRNIQVg5e1Q.png)
undefined

Lets understand the parametes in the app.yaml above

#### [**runtime**](https://cloud.google.com/appengine/docs/the-appengine-environments)

[specifying nodejs means,](https://cloud.google.com/appengine/docs/flexible/nodejs/runtime) the Node.js runtime is the software stack responsible for installing your application’s code and its dependencies and running your application. If both a `package-lock.json` and `yarn.lock` exist, your deployment will fail with an error. Note, the default package manager is `npm.` If you need both files, you can add one of them to the `[skip_files](https://cloud.google.com/appengine/docs/flexible/nodejs/reference/app-yaml#general)` [section of your](https://cloud.google.com/appengine/docs/flexible/nodejs/reference/app-yaml#general) `[app.yaml](https://cloud.google.com/appengine/docs/flexible/nodejs/reference/app-yaml#general)` file to resolve which package manager to use.

The runtime starts your application by using `[npm start](https://docs.npmjs.com/cli/start)`, which uses the command specified in `[package.json](https://docs.npmjs.com/files/package.json#scripts)`. For example:

```
"scripts": {  "start": "node app.js"}
```

Your start script should start a web server that responds to HTTP requests on the port specified by the `PORT`environment variable, typically 8080.

#### env — Environment: Standard vs. Flexible

There are two environment types offered by GAE. The documentation [here](https://cloud.google.com/appengine/docs/the-appengine-environments) has greater details. They have implecations for cost.

**Flexible**: In a flexible environment you are paying for usage of **vCPU**, **memory**, and **persistent disks**. It is generally more cost effective if you have regular traffic patterns that require scaling up and down **gradually**.

**Standard**: In a standard environment you are paying for only what you need (e.g. _instance hours_) and can scale to **0 instances** when there is no traffic. It is generally more cost effective for small applications that do not take traffic all of the time.

If there is no **env** key specified, it is deployed to the **standard** environment.

The biggest difference in my opinion between standard and flexible, is the lack of “scale to zero.” With App Engine Standard, if no one is using your application it shuts everything down. The moment a user visits, App Engine spins up an instance in milliseconds to serve the new request.

[See the comparison here.](https://cloud.google.com/appengine/docs/the-appengine-environments#comparing_environments)

Here in my app.yaml file I am using [**flex environment**](https://cloud.google.com/appengine/docs/flexible/) — allowing myApp Engine to have control over several aspects of your deployment, such as automatically scaling your app up and down while balancing the load. Microservices, authorization, SQL and NoSQL databases, traffic splitting, logging, versioning, security scanning, and content delivery networks are all supported natively.

#### **manual\_scaling**

Here, **I am locking instances to 1**, the most important tool to control cloud resources and cost.

The _instance_ is your app’s unit of computing power. It provides memory and a processor, isolated from other instances for both data security and performance. Your application’s code and data stay in the instance’s memory until the instance is shut down, providing an opportunity for local storage that persists between requests.

Within the instance, your application code runs in a _runtime environment_. The environment includes the language interpreter, libraries, and other environment features you selected in your app’s configuration.

Using App Engine Flexible will create VM instances to serve from and this will increase my usage and cost. If you want to lower your usage, and if your application does not require redundancy or high availability, you might restrict your App to just one instance. But this is not what you want in production, but perfect for in prototyping and development cases.

Read more on manual vs auto scaling in [_official docouments._](https://cloud.google.com/appengine/docs/flexible/nodejs/how-instances-are-managed)

#### resources

By setting the appropriate resource sizing we will be matched to the appropriate machine type.

There are three parameters thai I have set: **cpu**, **memory\_gb**, and **disk\_size\_gb**. The other parameters for volumes we don’t need to focus on in this example because we are not mounting any volumes.

By default, you get **1 cpu core** and we will set it at 50% CPU Utilization.

And you can see in below screenshot the actual CPU utilization was just 3% (after I deployed the app).

![](https://cdn-images-1.medium.com/max/800/1*87FTJF8TlyM8GK9ZAK21kw.png)
undefined

By default, you get **0.6 GB of memory\_gb**. The official documentation says _“RAM in GB. The requested memory for your application, which does not include the ~0.4 GB of memory that is required for the overhead of some processes. Each CPU core requires a total memory between 0.9 and 6.5 GB.”_

And there is a formula in the [official documentation](https://cloud.google.com/appengine/docs/flexible/python/reference/app-yaml) that shows you need ~0.4GB for overheard and a minimum total of 0.9GB. The formula from their documentation is

**memory\_gb** = **cpu** \* **\[0.9–6.5\] — 0.4**. 

This means the absolute minimum we can set is 0.5 GB for a single core and that is wher I am keeping it.

I tried setting it to 0.3 and the deployment command failed with the below error

Updating service \[default\] (this may take several minutes)...failed.

ERROR: (gcloud.app.deploy) Error Response: \[3\] App Engine Flexible validation error: Memory GB (0.70) per VCPUs must be between 0.90 and 6.50.

And as to the actual memory usage, after deployment, this is what I saw for my single running instance of this app.

![](https://cdn-images-1.medium.com/max/800/1*uP7QVQM1wdBgKEIStIAk4Q.png)
undefined

By default, you get **10 GB of disk** and since that is the minimum we will set that as such.

#### handlers

[Official documentation says](https://cloud.google.com/appengine/docs/standard/python/config/appref#handlers_element) — “The element provides a list of URL patterns and descriptions of how they should be handled. App Engine can handle URLs by executing application code, or by serving static files uploaded with the code, such as images, CSS, or JavaScript.”

So fundamentally, if an instance of your app is running and available to receive a user request, App Engine sends the request to the instance, and the instance invokes the request handler that corresponds with the URL of the request. a request comes in, a request handler comes to life, a response goes out. During its brief lifetime, the request handler makes a few decisions and calls a few services, and leaves no mark behind.

### app.yaml — general things to consider in configuring app.yaml to reduce cost while prototyping an App

#### 1\. Reduce number of instances in Google App Engine

While creating a new project, as you see above (in the image), its telling me I have 20 more Projects, that I can create. But just couple of days back it was telling me that I can create 6 more project, while I was having only 3 other projects at that time. I then deleted my previous projects along with its 14/18 instances that was running in those projects. And then when I was agin trying to create a new Project, it told me — _“You have 21 projects remaining in your quota. Request an increase or delete projects.”_

So, it completely depends on the no of instances running, and thats why in my **app.yaml** I will have **manual\_scaling and instances set to 1.** While my purpose is testing/prototyping a new App and/or exploring Google App Engine. Basically the below will limit the no of instances running for a project, to just 1.

A prototype or less-frequently application do not need to be running 100% of the time and you shouldn’t be paying for it.

Normally, if you deploy applications on GAE you will start to notice that you are spending money for multiple instances instead of just one when your application **\*is\*** running. Setting **manual\_scaling and instances to 1** menas scaling back to a single instance for applications in _flexible_ environments that do not need to be up and running for 100% of the time or have strict reliability/latency constraints.

We can cut our cost substantially by launching only one instance instead of many. Do this by turning on manual scaling (you don’t need autoscaling for a prototype or testing) and setting the instances to 1.

![](https://cdn-images-1.medium.com/max/800/1*GSWEmXSM7bgWTU3_RAggKA.png)
undefined

#### 2\. Reduce CPU and Memory

resources:  
  cpu: .5  
  memory\_gb: 0.5  
  disk\_size\_gb: 10

The above has got enough RAM and CPU to run most prototype-level workloads, so you are not spending cash on unused resources.

#### Installing **Google Cloud SDK**

Just follow the official guide — [https://cloud.google.com/sdk/docs/quickstart-linux](https://cloud.google.com/sdk/docs/quickstart-linux)

During installation while it asked for the path,   
Enter a path to an rc file to update, or leave blank to use   
\[~/.bashrc\] — (This is for my Ubuntu 18.04 machine )  
I just selected the default by pressing Enter  
Close that terminal and reopen. Or I coule do \`**source ~/.bashrc\`** to reload.  
Then Run **gcloud** to see a list of help commands  
And running **which gcloud** will give me the installed directory of this tool.

#### Then run

**gcloud auth login**

It will open a browser session and I have to do a regular google cloud login with Google OTP received in mobile.

Then run

**gcloud init**

**gcloud init** command to perform several common SDK setup tasks. These include authorizing the SDK tools to access Google Cloud Platform using your user account credentials and setting up the default SDK configuration.

What the [**above command does**](https://cloud.google.com/sdk/gcloud/reference/init) is

*   Authorizes gcloud and other SDK tools to access Google Cloud Platform using your user account credentials, or from an account of your choosing whose credentials are already available.
*   Sets up a new or existing configuration.
*   Sets properties in that configuration, including the current project and optionally, the default Google Compute Engine region and zone you’d like to use.

And follow the steps in the terminal.

This will trigger first a

1.  Login to Gcloud (Linked to usual google login)
2.  Select a project that you have already created in cloud by selecting a number.
3.  Sometimes they may ask you to choose a default region (pick one nearest to you to get maximum speed)

After running `gcloud init` I got

Pick configuration to use:  
 \[1\] Re-initialize this configuration \[mui-3-conf\] with new settings   
 \[2\] Create a new configuration  
 \[3\] Switch to and re-initialize existing configuration: \[default\]  
 \[4\] Switch to and re-initialize existing configuration: \[mui-2\]  
 \[5\] Switch to and re-initialize existing configuration: \[mui-conf\]  
 \[6\] Switch to and re-initialize existing configuration: \[second-conf\]

From above I selected the no \[3\] the default one. Note: reinitializing an existing configuration will remove all its existing properties! Then on another day I have also chosen no \[1\] above for a new app (but deployed to the same GCP Project with new services name.

Finally Run the following command to deploy your app > Select region

**gcloud app deploy --stop-previous-version**

Note, `gcloud app deploy` will look at the current directory first for `app.yaml`. Generally you will change to this project root directory which has app.yaml and your other files before deploying. So **the command should be executed in the directory in which the service’s** `**app.yaml**` **configuration file exists (and has that exact name, can't use a different name).**

For other cases (when you are not in the project root directory, where appl.yaml exists ) deployables must be specified. From `[gcloud app deploy](https://cloud.google.com/sdk/gcloud/reference/app/deploy)` as below

> **_SYNOPSIS_**

> `_gcloud app deploy [DEPLOYABLES …] [--bucket=BUCKET] [--image-url=IMAGE_URL] [--no-promote] [--no-stop-previous-version]_`

> _\[ — version=VERSION, -v VERSION\] \[GCLOUD\_WIDE\_FLAG …\]_

> **_DESCRIPTION_**

> _This command is used to deploy both code and configuration to the App Engine server. As an input it takes one or more_ `_DEPLOYABLES_` _that should be uploaded. A_ `_DEPLOYABLE_` _can be a service's .yaml file or a configuration's .yaml file (for more information about configuration files specific to your App Engine environment, refer to_ [_https://cloud.google.com/appengine/docs/standard/python/configuration-files_](https://cloud.google.com/appengine/docs/standard/python/configuration-files) _or_ [_https://cloud.google.com/appengine/docs/flexible/python/configuration-files_](https://cloud.google.com/appengine/docs/flexible/python/configuration-files)_). Note, for Java Standard apps, you must add the path to the_ `_appengine-web.xml_` _file inside the WEB-INF directory. gcloud app deploy skips files specified in the .gcloudignore file (see_ `_gcloud topic gcloudignore_` _for more information)._

So apart from running the command with no arguments in the directory in which your `app.yaml` exists is to specify the `app.yaml` (with a full or relative path if needed) as a deployable:

```
gcloud app deploy path/to/your/app.yaml
```

This will take quite some time like 10 to 20 mints to complete.

And after running **gcloud app deploy — stop-previous-version**

Before its start the deployment process, wil show me the final configurations.

descriptor: \[~/my-project-root/app.yaml\]  
source: \[~/this-willbe-full-path-name-of-the-root-folder\]  
target project: \[steel-aileron-266311\]  
target service: \[name-of-the-service-that-I-have-given-in-app.yaml-file\]  
target version: \[20200205t210728\]  
target url: \[[https://chalo-app-dot-steel-aileron-266311.appspot.com](https://chalo-app-dot-steel-aileron-266311.appspot.com)\]

As soon as deployment is done, you can run your app in the browser i.e. to view your application in the web browser run:

**gcloud app browse**

And then you can see the server logs in your Terminal by running

**gcloud app logs tail -s default**

#### Few Points to know about GAE to implement proper control

#### _Control the no of Instances running_

when deploying to GAE and using the following command “**gcloud app deploy**”, it creates a new version and sets it as the default, but also and more importantly, it does NOT remove the previous version that was deployed.

More info about versions and instances can be found here: [https://cloud.google.com/appengine/docs/standard/java/how-instances-are-managed](https://cloud.google.com/appengine/docs/standard/java/how-instances-are-managed)

So in my case, when I was experimenting with deployment for the very first time my Node-React-Mongo app and it was not working as expected, hence I was running the **gcloud app deploy command again and again**, without knowing, that each time I was running the above command, Ihad created multiple versions of my simple node app. These versions are still running in case one needs to switch following an error. But these versions also require instances and if you dont control these increasing instances you can exhaust your $300 free credit in matter of weeks or even days in the worst case.

In this respect, Google says:

> _App Engine by default scales the number of instances running up and down to match the load, thus providing consistent performance for your app at all times while minimizing idle instances and thus reducing cost._

**To see the running instances of a Project**

From Leftmost Drawer/Settings > App Engine > Instances

I can delete them all by selecting them.

So when deploying the better approach is to use the

**\--stop-previous-version** 

command options when deploying with **gcloud app deploy**.

And but **— -stop-previous-version** command options will only work if I have **Manual scaling in my app.yaml.** A service with manual scaling uses resident instances that continuously run the specified number of instances irrespective of the load level. So

**\-- stop-previous-version**

This will stop the previously running version when deploying a new version that receives all traffic. Note that if the version is running on an instance of an auto-scaled service, using — stop-previous-version will not work and the previous version will continue to run because auto-scaled service instances are always running.

**After updaing (i.e. some changes in source code) a project locally, how do I update the same in Googel App Engine / i.e. updating my webapp after deploy**

**Simple ans — you need to deploy a completely new version of the app to modify a file.** So the advice would be — make the most of testing your app locally. There is no way to change 1–2 files on the server so that it would update the app. Deployment is the process of updating the live app. **If you want some changes to be made to the app that is already deployed, you will have to redeploy — there is no way around it.** This is why it is recommended to test the app locally before (re)deploying so that you are sure everything is working fine.

#### Now that I have deployed one App in Google Cloud — Do I need to create for each new Google App Engine app new project? Or is there other way to have multiple apps in one project?

The simple ans is NO, I dont need to create another completely new Project for another application.

This is easily done with `services`. When you deploy to App Engine define your `app.yaml` file with a line like: `service: my-second-app`

Complete app.yaml file for another Node.js service:

```
service: my-second-appruntime: nodejsenv: flexautomatic_scaling:   min_num_instances: 1
```

**So basically you do not have to create the new service beforehand, it is created on deployment if you just type the new service name in the app.yaml i.e. service: my-second-app .**

**Thats why, every time you upload something on App Engine you have to define a** [**version name**](https://cloud.google.com/appengine/docs/python/config/appconfig#Python_app_yaml_Required_elements) **and you can upload up to 25 different versions for the same application ID.**

If you omit the version from the URL you are getting the default version that you have chosen from the dashboard. So in theory you can have up to 25 different application running under the same project, but they will share the same datastore. Note that having multiple apps in the same project is a good idea only if these apps are closely related. Otherwise, it becomes very inconvenient, even if you use different App Engine modules to run each app’s server-side code.

And now few super basic things to undersand this picture a little bit more.

First of all, before you can [deploy your apps](https://cloud.google.com/appengine/docs/standard/python/tools/uploadinganapp) to the App Engine standard environment, you typically need to create or set up the following:

1.  [A Cloud project](https://cloud.google.com/appengine/docs/standard/python/console/#create)
2.  [An App Engine application](https://cloud.google.com/appengine/docs/standard/python/console/#create)
3.  [A billing account](https://cloud.google.com/appengine/docs/standard/python/console/#billing)

Now to get more understanding of this, lets see this picture presenting Google App Engine services hierarchy, taken from their [**official guide**](https://cloud.google.com/appengine/docs/standard/nodejs/an-overview-of-app-engine)

![](https://cdn-images-1.medium.com/max/800/1*HwZD5gOpnj3hzeOwB6XJXg.png)
undefined

So you have **an application** under your Google Cloud **project**. An application can have one or more **services**. Services are loosely coupled and are developed and maintained independently. For many people it might be confusing as they may call a _service_ an _application_. Google changed naming convention to use [microservices](https://en.wikipedia.org/wiki/Microservices) nomenclature.

A service can have different **versions** (e.g. v1.0.1, v1.0.2, v2.0.0 etc.) and a version can have multiple instances that handle newtwork traffic.

Obviously there are limitations for number of services, versions and instances and they depend on region and free / paid version as specified

**Issues around service permission and Billing activation after I create a new Project — In Feb-2020 after running <gcloud app deploy — stop-previous-version>**

It failed to deploy first giving me some ‘Access Denied’ issue, I did not do much research, but it could be related to billing-activation issue as well.

And then when I creaed a completely new project by running

gcloud init

And choosing the option of creating a new project instead of an existing one. Then again, I got error some thing like below

Billing must be enabled for activation of service '\[appengineflex.googleapis.com, cloudbuild.googleapis.com, compute.googleapis.com, compute.googleapis.com, compute.googleapis.com, container.googleapis.c

So I started navigating around my GCP Console mainly following this [**official guide**](https://cloud.google.com/billing/docs/how-to/modify-project).

*   In the project drop down, at the top of the Google Cloud Console page, select your project.
*   Open the console **Navigation menu** (menu), and then select **Billing**.
*   If billing is not enabled on the project, a pop-up window will display, with text similar to:

“This project is not linked to a billing account”  
 To enable billing on the project, select Link a billing account.  
 To view a list of billing accounts for your organization, select Manage billing accounts.

So I indeed saw even though there was another project linked to a activated Billing accout, but this newly created one is not linked to any billing account and so I had link and activate it.

#### Issues around the GCP compulsory requirement of creating ONLY default services / default module when I create a new Project and try to deploy the first app from that Project

After I created a fresh new Project in GCP and ran **<gcloud app deploy — stop-previous-version>** got error saying

ERROR: (gcloud.app.deploy) INVALID\_ARGUMENT: The first service (module) you upload to a new application must be the 'default' service (module). Please upload a version of the 'default' service (module) before uploading a version for the 'chalo-app' service (module). See the documentation for more information. Python: (https://developers.google.com/appengine/docs/python/modules/#Python\_Uploading%%20modules) Java: (https://developers.google.com/appengine/docs/java/modules/#Java\_Uploading%%20modules)

And the source of the problem here, that in my app.yaml I had the below

service: some-text

And before starring the deployment process GCP Terminal was showing me the below

target service:  \[some-text\]

But GCP requirement is that for that the first App I deploy after creating a new project, must be the default service module. So I just had to delete that line completely from my app.yaml file

And now when I started deploying command again, it showed

target service:  \[default\]

And everything ran fine from here on, and my app got successfully deployed.

By [Rohan Paul](https://medium.com/@paulrohan) on [April 4, 2019](https://medium.com/p/1ba680447d59).

[Canonical link](https://medium.com/@paulrohan/deploying-a-react-node-mongodb-app-to-google-cloud-platforms-google-app-engine-1ba680447d59)

Exported from [Medium](https://medium.com) on December 12, 2020.