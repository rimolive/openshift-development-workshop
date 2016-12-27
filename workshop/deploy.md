Deploy
======
Now we have the `frontend` application up and running, we will publish the `guestbook-service` microservice. Since it contains a custom process to build the code, we will first build it locally and then add the image to our OpenShift internal registry before deploy in our project.

Building the `guestbook-service` application
--------------------------------------------
From the Terminal, you can set all Environment Variables needed to
```
$ eval "$(vagrant service-manager env docker)"
```

```
$ docker --tlscacert=$DOCKER_CERT_PATH/ca.pem \
         --tlscert=$DOCKER_CERT_PATH/cert.pem \
         --tlskey=$DOCKER_CERT_PATH/key.pem images
```

```
$ cd $GIT_REPO/guestbook-service
$ ./builddocker.sh $REGISTRY_URL/sample-project/guestbook-service:1.0
```

```
$ docker push $REGISTRY_URL/sample-project/guestbook-service:1.0
```

Alternative option to build `guestbook-service` application
-----------------------------------------------------------
Another way to build the image is using JBoss Developer Studio. Follow the steps below to build using the IDE:

* In the  menu bar, click on `Window -> Perspective -> Open Perspective -> Other...`
* Select `Docker Tooling` and click `OK`. The new perspective will show.
* In `Docker Explorer` view, click in the `Add Connection` button.
* In the `New Docker Connection` dialog, fill the parameters with the following information:
    * `Container Development Kit` in the `Connection Name` field
    * Select `TCP Connection`
    * In the `URI` field, type `tcp://10.1.2.2:2376`
    * Check `Enable authentication`
    * Click on the `Browse...` button in the `Path` field. Select the path `$CDK_HOME/components/rhel/rhel-ose/.vagrant/machines/default/libvirt/docker` and click `OK`
    * Click on `Test Connection` button. If it shows a `Ping succeed!` message box then we are good.
    * Click `Finish`
* The `Docker Images` view will show the same images than the `docker images` command from last section.
* In the same view, click on `Build Image` button.
* In the `Docker Build` dialog, put the following information in the fields:
    * `$REGISTRY_URL/sample-project/guestbook-service` in the `Name` field
    * `$GIT_REPO/guestbook-service` in the `Directory` field
    * Click `Finish`
* A `Console` view will open with the output of the `docker build` command
* Wait for a `Successfuly built $IMAGE_HASH` message.

Now we are ready to deploy our REST Service so `frontend` application will connect to it. Follow the next lab to deploy the REST Service.

Lab: Deploying `guestbook-service` application
----------------------------------------------
* In the `OpenShift Explorer` view, right-click in the `Deploy Docker Image...` menu. A new dialog will appear.
* In the `Deploy Image to OpenShift` dialog, click on `Browse...` button next to `Image Name` field. A new dialod will appear.
* Select the image we built previously and click `OK`
* Check the `Push Image to Registry` option and click `Next >`
* Click `Next >` again
* Uncheck the `Add Route` box and click `Next >`
* Click `Finish`
* After that, a new dialog will appear listing the new objects to create. Click `OK`
* Refresh `OpenShift Explorer` view. A new application `guestbook-service` will appear and a new `Pod` is running.

Now we just deployed our REST Service! However, if you access frontend application again you will still see error messages. If you right-click in the `guestbook-service` Pod and select `Pod Log...` menu, you will find this error:
```
2016-12-27 13:04:12,266 WARN  [org.jboss.jca.core.connectionmanager.pool.strategy.OnePool] (ServerService Thread Pool -- 15) IJ000604: Throwable while attempting to get a new connection: null: javax.resource.ResourceException: IJ031084: Unable to create connection
	at org.jboss.jca.adapters.jdbc.local.LocalManagedConnectionFactory.createLocalManagedConnection(LocalManagedConnectionFactory.java:343)
	at org.jboss.jca.adapters.jdbc.local.LocalManagedConnectionFactory.getLocalManagedConnection(LocalManagedConnectionFactory.java:350)
    ...
    More stacktrace here
    ...
Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Could not create connection to database server. Attempted reconnect 3 times. Giving up.
```
Oops! This REST Service uses MySQL as the Database and we don't have any instance running. We need to deploy a MySQL application.

For the purpose of this next lab, we will use a [template](https://raw.githubusercontent.com/openshift/openshift-ansible/master/roles/openshift_examples/files/examples/v1.4/db-templates/mariadb-ephemeral-template.json) and will add in the OpenShift Web Console. Follow the next lab to deploy the MySQL instance.

Lab: Deploy the MySQL template
------------------------------
* In the `Openshift Explorer` view, right-click in the `sample-project` project and select `Show In -> Web Console`
* The web browser will open the `OpenShift Web Console` page containing the dashboard of your applications running in it.
* Click on `Add to project` option in the OpenShift bar.
* Click on `Import JSON/YAML` tab and copy/paste the contents of the [template](https://raw.githubusercontent.com/openshift/openshift-ansible/master/roles/openshift_examples/files/examples/v1.4/db-templates/mariadb-ephemeral-template.json) and click `Create`
* Make sure only the `Process the template` option is checked and cilck `Continue`
* The parameters page will show. Fill in the form with the following information:
    * `guestbook` in the `MariaDB Connection Username` field
    * `guestbook` in the `MariaDB Connection Password` field
    * `guestbook` in the `MariaDB Database Name` field
* Click `Create`

Lab: Configuring `guestbook-service` Datasource
-----------------------------------------------
* In the `Openshift Explorer` view, right-click on `guestbook-service` appliation and select `Manage Environment Variables...` menu
* In the dialog, Click on `Add...` button
* In the `Add Environment Variable` dialog, fill in with the following information:
    * `JDBC_URL` in the `Name`field
    * `jdbc:mysql://<MYSQL_SERVICE_IP>:3306/sampledb` in the `Value` field
* Click `OK`
* Do the same for `DATASOURCE_USERNAME` and `DATASOURCE_PASSWORD` env vars.

[Next: Developer Tasks](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/developer-tasks.md)