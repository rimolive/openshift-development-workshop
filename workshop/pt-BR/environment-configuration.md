Environment Configuration
=========================

Lab: Configuring CDK in JBoss Developer Studio
----------------------------------------------
* Open `JBoss Developer Studio` (`JBoss Developer Suite` for Windows users). You will see the main page of Red Hat Central in the first time you open the IDE.
![JBoss Developer Studio](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/jbdevstudio.png)

* Below the IDE you will find the `Servers` tab. Click on it then Right-Click on the blank view and select `New -> Server`
* In the `New Server` window, find the `Red Hat Container Development Kit` server and click `Next >`
![CDK](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/cdk.png)

* Click `Add...` button in the username field. Fill your Red Hat Developers username and password and click `OK`
* Click `Browse` and point to your CDK installation (the location is $CDK_HOME/components/rhel/rhel-ose)
![CDK Browse](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/cdk-browse.png)

* Click `Next >`
* Click `Finish`
![CDK Finish](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/cdk-finish.png)

* After that, the CDK is added in `Servers` view. From there you can start/stop your CDK, also note that when you start CDK from the IDE it will show the `OpenShift Explorer` tab. From that view you can control your projects, applications, etc.

Lab: Configure kubernetes-lab Git repository
--------------------------------------------
* In the menu bar, Click on `Window -> Perspective -> Open Perspective -> Other...`
* Select `Git` perspective then click `Ok`. You will see that the views changed.
![Git Perspective](https://raw.githubusercontent.com/rimolive/openshift-development-workshop/master/images/git-perspective.png)

* Click `Add an existing local Git repository`
* Click `Browse...` and point to the location where you cloned kubernetes-lab code.
* Mark the repository and click `Finish`
* The repository is added in `Git` perspective. After that, Right-click on the repository and select `Import Maven projects...`
* Go to `JBoss` perspective
* Both `helloworld-service` and `guestbook-service` will be added in your workspace.

Since `frontend` is a Node.js application it won't be added as a Maven project, but in case you want to add follow the below steps:

* Go back to the `Git` perspective and in the `kubernetes-lab` repository expand the `repo -> Working Tree`
* Right-click on `frontend` folder and select `Import projects...`
* In the `Import projects` dialog, make sure the `Import as a general project` option is checked and then click `Next >`
* Change the project name (or keep it as is if you want) then click `Finish`
* Go to `JBoss` perspective, the new project will be shown in your workspace

[Próximo: Build](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/pt-BR/build.md)