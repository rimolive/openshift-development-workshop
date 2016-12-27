Templating
==========
Templating is the easiest part when all components are already up and running in your project. After ensuring that the application is running without any issues, simply run the following command in a terminal:
```
$ oc export dc,bc,svc,route --as-template=guestbook-application -o json > guestbook-template.json
```
Now you have a JSON file containing all objects your project needs to provision the environment. There you will find:
* All `DeploymentConfig` objects
* All `BuildConfig` objects
* All `Service` objects
* All `Route` objects

However, you might want to parametrize your environment to let the user select some Database options or even add a PersistentVolume so the Database can persist its data in an external Storage.

[Next: Conclusion](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/conclusion.md)