# Reflection

<hr>

### 1. Compare the application logs before and after you exposed it as a Service.
#### Before Acessing the App
![image](https://github.com/Alvinzhafif/adprogModule11/assets/143392835/ff2ee546-1625-43e2-8020-b8d34312016a)
#### After Accessing the App
![image](https://github.com/Alvinzhafif/adprogModule11/assets/143392835/c7811a43-146e-4cc5-b6e4-506eafcae007)
Yes, it increases. Here we can see that the number of logs increases everytime we tried to access the app, specifically it increases by 2 for every access. This is caused because we are trying to log two seperate contents, one log for HTTP at port 8080 server and one other for UDP server at port 8081.

### 2. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
`-n` is defined as namespace, it is commonly used to specify the namespace where operations wants to be performed. It did not list out the pod I specifically created because it does not belong to the `kube-system` namespace, the `-n` command itself can be considered as some sort of a filter towards the display of services and pods.
