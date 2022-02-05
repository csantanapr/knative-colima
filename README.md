Knative Colima

[Colima](https://github.com/abiosoft/colima) help setup container runtimes on macOS (and Linux) with minimal setup.
It leverages the [limaVM project](https://github.com/lima-vm/lima).
Can be used as an alternative to Docker Desktop. Check out my git repo of [docker-desktop-alternatives](https://github.com/csantanapr/docker-desktop-alternatives) for other tools.

Currently in knative `kn quickstart` plugin version 1.2 you need to use 3 CPUs. colima creates the default vm with 2 CPUs.
Knative having Pod resources request higher than 2CPUs, doesn't allow some Pods to run.

See the knative quickstart issue [104](https://github.com/knative-sandbox/kn-plugin-quickstart/issues/104) for more details and latest status.

```
colima delete
colima start --cpu 3
```
For more options for the VM see `colima start --help`

Lets set some defaults config values for the `knative` minikube vm to be created using the `docker` driver to leverage colima instead of hyperkit
```
minikube config set driver docker -p knative
```

Review the current config
```
minikube config view -p knative
```
You want the output to look like this:
```
- driver: docker
```

Start a knative minikube using `kn quickstart` more information on the knative website [tutorial](https://knative.dev/docs/getting-started/)

```
kn quickstart minikube
```

Follow the instruction when prompted to run `minikube tunnel -p knative`



Lets create the knative minikube environment using the new colima VM with more CPUs
```
kn quickstart minikube
```

When your done you can delete or stop the `knative` minikube vm
To delete:
```
minikube delete -p knative
```
To stop:
```
minikube stop -p knative
```
>If you start the stopped minikube remember to run the `tunnel` command


To use kind run the following command, make sure you are using the colima VM with 3 CPUs
```
kn quickstart kind
```

When your done you can delete or stop the `knative` minikube vm
To delete:
```
kind delete cluster --name knative
```
To stop:
```
docker stop knative-control-plane
```
