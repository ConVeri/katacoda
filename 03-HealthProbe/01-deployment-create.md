While you are reading this we are starting a simple single node Kubernetes cluster for you. Please be patient and wait until the `launch.sh` script has finished.

Now let's create a simple Deployment which defines a _liveness_ and _readiness_ probe.
The application itself is a simple REST service which just returns a freshly generated random number each time it is called.

In `deployment.yml` you find the definition for this Deployment.

Check the content of this declaration with

`bat deployment.yml`{{execute}}

Please look specifically at the `livenessProbe` and `readinessProbe` declaration which do a HTTP based and file based health check, respectively.

Now it's time to create that Pod with

`kubectl create -f deployment.yml`{{execute}}

and watch how it starts up the application pods:

`kubectl get pods -w`{{execute}}

(you can stop this with <kbd>CTRL-C</kbd>).

When the pod is up and running (status is `1/1 Running`), let's create a Service to access the application.
We are using here a `NodePort` service with our application listening on a fixed port on every node of our cluster:

`kubectl create -f service.yml`{{execute interrupt}}

The random number service can now be accessed in Katacoda with

`curl -s http://[[HOST_IP]]:31667/ | jq .`{{execute}}

or you can also reach it externally via http://[[HOST_SUBDOMAIN]]-31667-[[KATACODA_HOST]].environments.katacoda.com/

To access the health check which is used in the liveness probe, try

`curl -s http://[[HOST_IP]]:31667/actuator/health | jq .`{{execute}}
