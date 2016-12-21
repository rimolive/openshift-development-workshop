Build
=====

Source-To-Image (S2I)
---------------------

```
        "strategy" : {
            "type" : "Source",
            "sourceStrategy" : {"from" : {
                "kind" : "ImageStreamTag",
                "namespace" : "openshift",
                "name" : "jboss-eap64-openshift:1.4"
            }}
        },
```


Docker build
------------
* Right-click on your project in `OpenShift Explorer` view and click on `Properties`
* The `Properties` view will open and show all objects that belongs to the project
* Select `Build Configs` , right-click on the Build Config and select `Edit...`. An editor will open.
* Locate the following snippet in the file:
```
        "strategy" : {
            "type" : "Source",
            "sourceStrategy" : {"from" : {
                "kind" : "ImageStreamTag",
                "namespace" : "openshift",
                "name" : "jboss-eap64-openshift:1.4"
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


Jenkins Pipelines
-----------------


[Next: Deploy](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/deploy.md)