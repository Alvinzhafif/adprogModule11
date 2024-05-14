# Reflection on Hello Minikube

<hr>

### 1. Compare the application logs before and after you exposed it as a Service.
#### Before Accessing the App
![image](https://github.com/Alvinzhafif/adprogModule11/assets/143392835/ff2ee546-1625-43e2-8020-b8d34312016a)
#### After Accessing the App
![image](https://github.com/Alvinzhafif/adprogModule11/assets/143392835/c7811a43-146e-4cc5-b6e4-506eafcae007)
Yes, it increases. Here we can see that the number of logs increases every time we try to access the app, specifically it increases by 2 for every access. This is caused because we are trying to log two separate contents, one log for HTTP at port 8080 servers and one other for UDP server at port 8081. In this example it increase by 4, because I tried to access the app twice.

### 2. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
`-n` is defined as a namespace, it is commonly used to specify the namespace where operations want to be performed. It did not list out the pod I specifically created because it does not belong to the `kube-system` namespace, the `-n` command itself can be considered as some sort of a filter towards the display of services and pods.

# Reflection on Rolling Update & Kubernetes Manifest File 

<hr>

### 1. What is the difference between Rolling Update and Recreate deployment strategy?

#### Rolling Update Strategy
This strategy involves updating the application by replacing the old version with the new version. It will update one or a few instances of the update for every update.
#### Recreate Strategy
This strategy involves updating the application by shutting down all existing instances of the app and then starting the new one above it. The main point of this strategy is that there is a time when no instances are running on the deployment.
#### Key Differences
| Aspect       | Rolling Update           | Recreate |
| ------------- |-------------| -----|
| Deployment Style      | One or few instance at a time | Full stop and start |
| Downtime      | Minimal to none      |   Always during update |
| Risk Management | Lower risk, easier rollback capability      |   Higher risk, simultaneous deployment |

### 2. Deploying the Spring Petclinic REST using the Recreate deployment strategy.

1. I made changes to the `deployment.yaml` file such that it would look like the following when described through the command prompt. This is for setting the strategy to recreate
![image](https://github.com/Alvinzhafif/adprogModule11/assets/143392835/abe20ebc-9c4b-416a-b426-349cd029ee40)
2. I simulate an update by setting an image for deployment and confirming that update by using the ` kubectl rollout status deployments/spring-petclinic-rest` command. Here's the output:
![image](https://github.com/Alvinzhafif/adprogModule11/assets/143392835/f52b278d-0b3c-43ad-8037-e9eecb1d3966)
We can see here that the deployment's strategy has been changed to recreate through the age of the Pods. The new pods have the same ages for all four of them. If we were to use the RollingUpdate strategy, its replacing strategy would cause the age of the pods to be different.
### 3. Prepare different manifest files for executing Recreate deployment strategy.
Here's my new manifest file for the recreate strategy:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "4"
  creationTimestamp: "2024-05-13T06:56:18Z"
  generation: 5
  labels:
    app: spring-petclinic-rest
  name: spring-petclinic-rest
  namespace: default
  resourceVersion: "3568"
  uid: 954779d6-4213-44db-9a6c-4974f5a140c1
spec:
  progressDeadlineSeconds: 600
  replicas: 4
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: spring-petclinic-rest
  strategy:
    type: Recreate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: spring-petclinic-rest
    spec:
      containers:
      - image: docker.io/springcommunity/spring-petclinic-rest:3.2.1
        imagePullPolicy: IfNotPresent
        name: spring-petclinic-rest
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 4
  conditions:
  - lastTransitionTime: "2024-05-13T07:05:33Z"
    lastUpdateTime: "2024-05-13T07:05:33Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2024-05-13T06:56:18Z"
    lastUpdateTime: "2024-05-13T07:10:22Z"
    message: ReplicaSet "spring-petclinic-rest-54f476f68" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 5
  readyReplicas: 4
  replicas: 4
  updatedReplicas: 4
```

### 5. What do you think are the benefits of using Kubernetes manifest files? 

#### 1. Declarative Configuration
By using Manifest Files, we can describe the desired state of the app and infrastructure using a `YAML` or `JSON` file. Kubernetes will then automatically set the actual state to match what we inputted in the file. Compared to doing it manually, we need to input our own commands for setting up the app. Increasing the probability of errors and inconsistency.
#### 2. Consistency and Repeatability
Another advantage it provides is consistency and repeatability. Manifest files can be reused for deploying other environments in the same configuration. Should there be a change, it would not be difficult to just edit and reset the configuration of the manifest files. Compared to doing it manually, we need to set up each environment separately and manually, causing possible errors.
#### 3. Scalability and Management
Kubernetes Manifest Files also provide ease in deployments of large-scale projects, we can define scaling, complex configurations, and dependencies using the manifest files. When doing manual Deployment, we need to manage scaling apps manually, which can become redundant and prone to errors.
  







