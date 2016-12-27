Build
=====
Our first labs will be publish the `frontend` application using some of the provided Build types (S2I, Docker and Pipelines). During this section, you will see how you can build (and consequently deploy) applications in OpenShift.

Source-To-Image (S2I)
---------------------
* Right-click on your project in `OpenShift Explorer` view and click on `New -> Application...`
* In the `New OpenShift Application` dialog, search for `nodejs:0.10` Builder image and select it in the list
![Build 1](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/build-1.png)
* Click `Next >`
* In the `Name` field, put the name of your application to `frontend` (or whatever the name you want)
* In the `Git Repository URL`, type the name of your kubernetes-lab GitHub fork
* In the `Git Reference`, type `master`
* In the `Context Directory`, type `frontend`
![Build 2](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/build-2.png)
* Click `Next >`
![Build 3](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/build-3.png)
* Click `Next >` again
![Build 4](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/build-4.png)
* Click `Next >` again^2
* Click `Finish`
* A `Create Application Summary` dialog will show containing all objects will be created. Simply click `OK`
* In the `OpenShift Explorer` view, a new application is created and a build is already started
* Right-click on the build and select `Build log...`
* The `Console` view will be shown and will log all S2I progress.
* After you see the message `Push successful' in the Build log, go back to the `OpenShift Explorer` view. You will see that the build disappeared and a Pod is listed in the application
* Right-click the application and select `Show In -> Web Browser`. The internal Eclipse browser will show and the application will be available in the URL `http://frontend-sample-project.rhel-cdk.10.1.2.2.xip.io/`
* When you access the application, you will see the following message:
```
Error reading the messages. Please check the logs for more details.
```
Don't worry. That just mean it cannot connect to the `guestbook-service` Microservice, which we didn't deploy it yet. We will see in the next lab how to deploy the `guestbook-service`
* Go back to `OpenShift Explorer` view, right-click on the project and select `Properties`
* The `Properties` view will show containing all objects created in this project. Select `Build Configs` tab, right-click on `frontent` Build Config and select `Edit...`
* An Editor will open containing the JSON information about the Build Config. Scroll down in the file until you find the snippet below:
```
        "strategy" : {
            "type" : "Source",
            "sourceStrategy" : {"from" : {
                "kind" : "ImageStreamTag",
                "namespace" : "openshift",
                "name" : "nodejs:0.10"
            }}
        },
```
This is the code that configures the S2I Build for your application, configured to use `nodejs:0.10` image as the base for the build. Next lab we will convert this build to use the bundled Dockerfile inside the repository to build our application.

Docker build
------------
* Right-click on your project in `OpenShift Explorer` view and click on `Properties`
* The `Properties` view will open and show all objects that belongs to the project
* Select `Build Configs`, right-click on the Build Config and select `Edit...`. An editor will open.
* Locate the following snippet in the file:
```
        "strategy" : {
            "type" : "Source",
            "sourceStrategy" : {"from" : {
                "kind" : "ImageStreamTag",
                "namespace" : "openshift",
                "name" : "nodejs:0.10"
            }}
        },
```
* Change the code with the following:
```
        "strategy" : {
            "type" : "Docker",
            "dockerStrategy" : {}
        },
```
* Save the file
* Go back to `Properties` view, right-click on the Build Config and select `Start Build`
* A new build is triggered. Go back to `OpenShift Explorer`, expand the application, right-click the build and select `Build Log...`
* Check for the logs to successfuly complete the build

(Optional) Jenkins Pipelines
----------------------------

(Optional) Binary Build
-----------------------


[Next: Deploy](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/deploy.md)