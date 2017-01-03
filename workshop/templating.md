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

However, you might want to parametrize your environment to let the user select some Database options or even add a PersistentVolume so the Database can persist its data in an external Storage. I'll leave this step to you since you can define whatever parameters you want, but I'll show you some of the parameters that might be interesting:

```json

  "parameters": [
    {
      "name": "MEMORY_LIMIT",
      "displayName": "Memory Limit",
      "description": "Maximum amount of memory the container can use.",
      "value": "512Mi",
      "required": true
    },
    {
      "name": "NAMESPACE",
      "displayName": "Namespace",
      "description": "The OpenShift Namespace where the ImageStream resides.",
      "value": "openshift"
    },
    {
      "name": "DATABASE_SERVICE_NAME",
      "displayName": "Database Service Name",
      "description": "The name of the OpenShift Service exposed for the database.",
      "value": "mariadb",
      "required": true
    },
    {
      "name": "MYSQL_USER",
      "displayName": "MariaDB Connection Username",
      "description": "Username for MariaDB user that will be used for accessing the database.",
      "generate": "expression",
      "from": "user[A-Z0-9]{3}",
      "required": true
    },
    {
      "name": "MYSQL_PASSWORD",
      "displayName": "MariaDB Connection Password",
      "description": "Password for the MariaDB connection user.",
      "generate": "expression",
      "from": "[a-zA-Z0-9]{16}",
      "required": true
    },
    {
      "name": "MYSQL_DATABASE",
      "displayName": "MariaDB Database Name",
      "description": "Name of the MariaDB database accessed.",
      "value": "sampledb",
      "required": true
    },
    {
      "name": "VOLUME_CAPACITY",
      "displayName": "Volume Capacity",
      "description": "Volume space available for data, e.g. 512Mi, 2Gi.",
      "value": "1Gi",
      "required": true
    }
]
```

These are the parameters for the `mariadb-persistent` template. Try to add these parameters in your template and deploy it in another namespace for testing.

[Next: Conclusion](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/conclusion.md)