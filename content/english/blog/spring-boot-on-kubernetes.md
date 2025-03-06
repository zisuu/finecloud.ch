---
title: "Spring Boot on Kubernetes"
meta_title: "Spring Boot on Kubernetes"
description: "We will use Docker Desktop to provide us a Test Kubernetes Environment. Open Docker Desktop Settings, go to Tab Kubernetes. Select Enable Kubernetes, then Apply & Restart. Now you should be able to see docker-desktop listed, if you run kubectl get nodes."
date: 2023-05-03T20:46:00
categories: ["Software Development"]
author: "David"
tags: ["container", "docker", "java", "kubernetes", "software development", "spring", "spring-framework"]
draft: false
---

### Enable Kubernetes in Docker Desktop

We will use Docker Desktop to provide us a Test Kubernetes Environment.

Open Docker Desktop Settings, go to Tab "Kubernetes". Select "Enable Kubernetes", then "Apply & Restart".

Now you should be able to see docker-desktop listed, if you run `kubectl get nodes`.

### Create Deployment

We will use the Docker Image we created in the last Post. Instead of directly deploying the Application we want to create a deployment.yml file:

```sh
kubectl create deployment myapp --image name/myapp --dry-run=client -o=yaml > deployment.yml
```

The content looks like this:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: myapp
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: myapp
    spec:
      containers:
      - image: name/myapp
        name: myapp
        resources: {}
status: {}
```

Now we can apply this deployment with: kubectl apply -f deployment.yml.

**Create Service**

With the command:
`kubectl create service clusterip myapp --tcp=8080:8080 --dry-run=client -o=yaml > service.yml`

We can create the service definition file. And then apply it with:

`kubectl apply -f service.yml`

The content of the file:

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: myapp
  name: myapp
spec:
  ports:
  - name: 8080-8080
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: myapp
  type: ClusterIP
status:
  loadBalancer: {}
```

With kubectl get all we can now see that the service has been created.

**Port Forwarding**

To be able to access the App, we need to create a Port Forwarding like so:

1. Get your local IP address, e.g.: ipconfig getifaddr en0
2. Configure port forwarding with: kubectl port-forward service/myapp 8080:8080
3. Check if it works with: curl localhost:8080/actuator/health
4. This should return: {"status":"UP","groups":["liveness","readiness"]}

**Terminate Service and Deployment**

If you want to stop a Service or a Deployment you can use these commands:

```bash
kubectl delete service myapp
kubectl delete deployment myapp
```

**Exposing Services**

If you want to expose a Service permanently and not only with Port Forwarding you can go with this:

1. Replace type: ClusterIP in the service.yml file with type: NodePort
2. Reapply the deployment and service
3. Check what dynamic Port the service has been exposed on: kubectl get all
4. Check access with curl: curl localhost:31610/actuator/health

**Accessing Logs**
One option is to check the logs on the docker containers directly with:

1. `docker ps -a`
2. `docker logs -f <containername>`

But in a Kubernetes context you probably can't access the docker logs directly, or docker is not used at all, that's why you need to go with this:

1. `kubectl get all`
2. `kubectl logs -f <podname>`

**Setting Environment Variables**
Example use case: overwrite log levels.

1. Change your deployment.yml and add the env part to it:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: myapp
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: myapp
    spec:
      containers:
      - image: jhyyhpp/kbe-rest-brewery
        name: kbe-rest-brewery
        resources: {}
        env:
        - name: LOGGING_LEVEL_MYAPP
          value: INFO
status: {}
```
2. `kubectl apply -f deployment.yml`
3. `kubectl get all`
4. `kubectl logs -f <podname>`

**Readiness Probe**

A readiness probe is an essential feature in Kubernetes that ensures the proper functioning of a deployed application. Here are some reasons why you should use a readiness probe:

1. Prevents traffic to unhealthy pods: Kubernetes uses readiness probes to determine if a pod is ready to receive traffic or not. If a pod is not ready, Kubernetes will not route traffic to that pod. This ensures that traffic is only sent to healthy pods, preventing downtime and improving the overall availability of the application.
2. Allows for graceful scaling: When new pods are added to a deployment or replica set, Kubernetes uses readiness probes to determine when the new pods are ready to receive traffic. This allows for a more graceful scaling experience, as traffic is only routed to new pods once they are ready to handle it.
3. Helps with rolling updates: Kubernetes uses readiness probes to determine when a new version of an application is ready to receive traffic. This allows for rolling updates to be performed without causing downtime or disruption to users.
4. Provides insight into application health: Readiness probes can be used to provide insight into the health of an application. By monitoring the results of readiness probes, you can determine if your application is healthy and identify any issues that need to be addressed.

Overall, readiness probes are a crucial feature in Kubernetes that help ensure the proper functioning of your application and improve its availability.

**Liveness Probe**
A liveness probe is another important feature in Kubernetes that ensures the health of a deployed application. Here are some reasons why you should use a liveness probe:

1. Restarts unhealthy pods: Kubernetes uses liveness probes to determine if a pod is healthy or not. If a pod fails the liveness probe, Kubernetes will automatically restart the pod, ensuring that the application remains available.
2. Prevents failed requests: Liveness probes help prevent failed requests by ensuring that only healthy pods are serving traffic. If a pod is not healthy, Kubernetes will not route traffic to that pod, preventing failed requests and improving the overall availability of the application.
3. Identifies application failures: Liveness probes can be used to identify application failures and help diagnose issues. By monitoring the results of liveness probes, you can determine if your application is healthy and identify any issues that need to be addressed.
4. Supports self-healing: By automatically restarting unhealthy pods, liveness probes support self-healing in Kubernetes. This ensures that your application remains available even in the face of failures.

Overall, liveness probes are a crucial feature in Kubernetes that help ensure the health of your application and improve its availability. By using liveness probes, you can ensure that your application remains available, even in the face of failures, and identify and address any issues that may arise.

To add the readiness and liveness probes we need to add the following content to our deployment.yml file:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: myapp
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: myapp
    spec:
      containers:
      - image: jhyyhpp/kbe-rest-brewery
        name: kbe-rest-brewery
        resources: {}
        env:
        - name: LOGGING_LEVEL_CH_FINECLOUD_SFGRESTBREWERY
          value: INFO
        - name: MANAGEMENT_ENDPOINTS_HEALTH_PROBES_ENABLED
          value: "true"
        - name: MANAGEMENT_HEALTH_READINESSSTATE_ENABLED
          value: "true"
        - name: MANAGEMENT_HEALTH_LIVENESSSTATE_ENABLED
          value: "true"
        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: 8080
        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: 8080
status: {}
```

**Graceful Shutdown**
Graceful shutdown is an important feature to consider when deploying an application on Kubernetes. Here are some reasons why you should use a graceful shutdown:

1. Minimizes downtime: Graceful shutdown allows your application to shut down in a controlled manner, ensuring that any in-flight requests are completed before the application is terminated. This helps to minimize downtime and improve the overall availability of the application.
2. Avoids data loss: During a graceful shutdown, your application has the opportunity to save any data that needs to be persisted before shutting down. This helps to avoid data loss and ensures that your application can be restarted without losing any important data.
3. Prevents disruption to users: A graceful shutdown ensures that your application is shut down in a way that is transparent to users. By completing any in-flight requests and avoiding abrupt terminations, you can prevent disruption to users and provide a better user experience.
4. Supports scaling: When scaling down your application, a graceful shutdown ensures that any remaining requests are completed before the pod is terminated. This allows for more efficient scaling and helps to prevent any lost or interrupted requests.

Overall, a graceful shutdown is an important feature to consider when deploying an application on Kubernetes. It helps to minimize downtime, avoid data loss, prevent disruption to users, and support efficient scaling. By using a graceful shutdown, you can ensure that your application is shut down in a way that is safe, controlled, and reliable.

To configure graceful shutdown we need to add the following content to our deployment.yml file:

```yaml
- name: SERVER_SHUTDOWN
  value: "graceful"
lifecycle:
  preStop:
    exec:
      command: ["/bin/sh", "-c", "sleep 10"]
```

