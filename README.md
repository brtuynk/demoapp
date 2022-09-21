## Vadofone K8S Case

Springboot "Hello World" project with kubernetes and ingress using Jenkins

Shared URL: http://berat.feherconstruction.com/hello

* First of all, an Ubuntu 20.04 installed AWS EC2 is used as the environment(Size: t3.medium 2cpu & 4 RAM).

### Preparing spring-boot project

* Java, Maven installed to the server. Simple spring-boot hello world project is created.

### Preparing Jenkins Environment for the CI/CD pipeline

* Jenkins installed to the server separately. The port 8080 is used as public access: http://15.160.84.65:8080 uname:admin pwd:vodafone
* The required plugins are installed: Kubernetes, Git, etc.
* New pipeline created and the project's GitHub repo hooked with the git push actions for triggering pipeline automatically.

### Preparing Kubernetes Cluster

* Kubernetes installed via kubespray to the same server(cluster has only 1 node) to deploy spring-boot sample app as a service.
* nginx ingress controller is added to the kubernetes cluster.


### Preparing CI/CD processes

* A Dockerfile is prepared for building image.
* DockerHub is used as the image registry: https://hub.docker.com/repository/docker/beratuyanik/helloworld
* A Jenkinsfile is prepared for all of the CI/CD stages.
* A myweb.yaml named manifest file is prepared for deploying docker image on K8S.
* Login credentials are added to Jenkins for GitHub and DockerHub.
* Kubernetes cluster config is added to "Configure Clouds" settings of Jenkins and rest of the settings are set(k8s cloud details, pod templates, etc.).


## CI/CD Stages

* Since our Jenkinsfile is selected as the Script Path of Jenkins pipeline, all of the stages are prepared in Jenkinsfile.
* The stages are:

1. Checkout GitHub Source
2. Build Project (maven compile)
3. Run Tests (maven test)
4. Build Docker Image (docker build from Dockerfile)
5. Push Docker Image (docker push to the registry)
6. Deploy App on K8S (deploy service to kubernetes cluster using manifest)


## Addirional Info

* nginx ingress controller is used. Because it is managing/controlling and welcoming request traffic from outside, and then, transmits requests to the services in cluster.
* myweb service has 3 replicas for experiencing the load balancing feature.



## Some Screenshots:

* Jenkins Pipeline:
![JenkinsPipline](https://github.com/brtuynk/demoapp/blob/master/images/jenkinsPipline.png)


* GitHub Pepo Webhook:
![Webhook](https://github.com/brtuynk/demoapp/blob/master/images/webhook.png)


* Pipeline Dashboard Auto-triggering View after Git Push action:
![trigger](https://github.com/brtuynk/demoapp/blob/master/images/trigger.png)


* ```kubectl get nodes -A```, ```kubectl get svc -A```, ```kubectl get pods -A```
![k8s](https://github.com/brtuynk/demoapp/blob/master/images/k8s.png)


* The spring boot app (http://berat.feherconstruction.com/hello):
![hello](https://github.com/brtuynk/demoapp/blob/master/images/hello.png)
