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
* When you access the application, you will see the following message alert:
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

The following labs are optional because it contains Experimental features in Opensfhit and/or things that you do only in very specific situations. Unless you have enough time to do these labs, move on to them.

(Optional) Jenkins Pipelines
----------------------------

Before begin this lab, you must use the [latest version](https://raw.githubusercontent.com/openshift/openshift-ansible/master/roles/openshift_examples/files/examples/v1.4/quickstart-templates/jenkins-ephemeral-template.json) of the Jenkins Ephemeral template. CDK is still using the outdated version. Also, we need to add a `edit` rolebinding to the jenkins serviceacount so it can create the pipelines.

To update the template run the following commands:

```bash
$ oc login -u admin -p admin
$ oc create -f https://raw.githubusercontent.com/openshift/openshift-ansible/master/roles/openshift_examples/files/examples/v1.4/quickstart-templates/jenkins-ephemeral-template.json -n openshift
$ oc adm policy add-role-to-user edit system:serviceaccount:sample-project:jenkins -n sample-project
$ oc login -u openshift-dev -p devel
```

After that, follow the steps below:

* Run the following command to create a JSON file containing the BuildConfig object with Jenkins Pipeline configuration:

```bash
$ cat > frontend-pipeline.json<<EOF
{
    "kind": "BuildConfig",
    "apiVersion": "v1",
    "metadata": {
        "name": "frontend-pipeline"
    },
    "spec": {
        "triggers": [
            {
                "type": "GitHub",
                "github": {
                    "secret": "secret101"
                }
            },
            {
                "type": "Generic",
                "generic": {
                    "secret": "secret101"
                }
            }
        ],
        "runPolicy": "Serial",
        "strategy": {
            "type": "JenkinsPipeline",
            "jenkinsPipelineStrategy": {
                "jenkinsfile": "node('maven') { \n  stage 'build\n          openshiftBuild(buildConfig: 'frontend', showBuildLogs: 'true')\\\n  stage 'deploy'\n          openshiftDeploy(deploymentConfig: 'frontend')\n}"
            }
        }
    }
}
EOF
```
* A file called `frontend-pipeline.json` is created. After that, create the buildconfig object with the command below:

```bash
$ oc create -f frontend-pipeline.json
```

* Access [https://10.1.2.2:8443/console/project/sample-project/browse/pipelines/sample-pipeline](https://10.1.2.2:8443/console/project/sample-project/browse/pipelines/sample-pipeline). The Pipelines view page is shown.
* Click on `Start Pipeline` button.
* See the progress.
* Alternatively, you can follow the pipeline progress directly on jenkins by clicking on `View Log` link.

(Optional) Binary Build
-----------------------

Now we will try our last Build lab: The binary. You can reuse the buildconfig already created by openshift. All you need is run the command below to trigger a binary build for `frontend`:

```bash
$ oc start-build bc/frontend --from-dir=.
```

You will see in the build logs the git clone step being skipped.

[Next: Deploy](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/deploy.md)