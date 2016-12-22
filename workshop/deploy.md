Deploy
======
Now we have te `frontent` application up and running, we will publish the `guestbook-service` microservice. Since it contains a custom process to build the code, we will first build it locally and then add the image to our OpenShift internal registry before deploy in our project.

Building the `guestbook-service` application
--------------------------------------------

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

[Next: Developer Tasks](https://github.com/rimolive/openshift-development-workshop/blob/master/workshop/developer-tasks.md)