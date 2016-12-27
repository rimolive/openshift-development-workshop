Debug
=====
In JBoss Developer Studio, you can debug remotely your application using the OpenShift plugin. Follow the next steps to proceed with `frontend` code debug:

Lab: Debugging `frontend` application
-------------------------------------
* Right-click in the `frontend` server adapter in `Servers` view and select `Restart in Debug` menu.
* It will recreate the pod with debugging features.
* Open `frontend.js` file and put a breakpoint in line 47 (`res.send('I am ok');`)
* Access `http://frontend-sample-project.rhel-cdk.10.1.2.2.xip.io/api/health` endpoint. The `Debug` perspective will open and the request is locked in the breakpoint.
* Press `F8` to keep running the request thread.

[Next: Templating](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/templating.md)