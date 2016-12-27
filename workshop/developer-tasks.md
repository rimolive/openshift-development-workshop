Developer Tasks
===============
The next labs will be quick but it will be very valuable while coding your applications. There you can find ways to troubleshoot for potential problems you might find in your applications running on OpenShift.

Scaling the application
-----------------------
* Right-click in the `guestbook-service` application in the `OpenShift Explorer` view and select `Scale -> Up`
* Check if a new Pod is created
* Right-click in the `guestbook-service` application in the `OpenShift Explorer` view and select `Scale -> Down`
* Check if one of the pods is removed
* Right-click in the `guestbook-service` application in the `OpenShift Explorer` view and select `Scale -> To...`
* Type the number of pods you want and click `OK` (very careful to not overload CDK)

Viewing application logs
------------------------
* Right-click on any application pod and select `Pod Log...`
* The `Console` view will open and will show all application log.

Accessing Pod terminal
----------------------
* Right-click in the `frontend` pod and select `Show In -> Web Console`
* The Web Browser will open in the Pod status page.
* Select `Terminal` tab.
* Run the following command in the Terminal:
```
sh-4.2$ cat /etc/redhat-release
```
* Check if the output is: 
```
Red Hat  Enterprise Linux 7.2 (Maipo)
```

Incremental Deployment
----------------------
* Right-click on `frontend` application and select `Server Adapter...`
* Click `Finish`
* The `Servers` view will open containing the new server adapter. The `Console` will open and some scp commands will be executed.
* After that, open `index.html` in `frontend` project and change the `Say Hi` text to `Say Hi, <whatever name here>!`
* The `Console` view will open again with the scp commands.
* In `OpenShift Explorer` view, right-click in the `frontend` application and select `Show In -> Web Browser`
* Check if the page shows the update you made.

[Next: Debug](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/debug.md)