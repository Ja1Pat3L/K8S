# K8S

Kubernetes/Minkube Basic Commands

1. Start Minikube: minikube start --driver docker
2. Check Status: Minikube status
3. Get minikube nodes: kubectl get node
4. Apply file: kubectl apply -f webapp.yaml
5. Get all deployments: kubectl get all 
6. Get configmap: kubectl get configmap
7. Get secret: kubectl get secret
8. Get pods: kubectl get pods
9. kubectl --help
10.kubectl get --help
11. kubectl describe service webapp-service(for specific component)
12. Logs: kubectl logs webapp-deployment-65d4754f9d-5zvbp [after logs is the pod name]
13 for service command to get the port for web access: kubectl get svc
14. to get minikube ip for web access:  minikube ip
OR 15. kubectl get node -o wide
16. minikube service webapp-service
using this as minikube ip not working.





Basic Stuff:

1. Deployment for running the pods and services for providing an address to those pods

2.Selector:
               matchlabel:
                app:mongo (you can name it however you want as it is a key value pair)
For Eg: myKey:myValue

3. All the pods with this label belong to  name: mongo-deployment
  labels:
    app: nginx
this deployment

4.  replicas:3
means how many pods you want to create

5. Scale Database in kubernetes: we should use statefulSet and not a deployment

6. Service
We need selector to select pods to forward the request to

7. targetPort should be same as  containerPort as thats where the application in the pod is accessiblea and thats where the service should forward the request to. Service port attribute sets the port of service and  can be different but common practice to keep it same.

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
  labels:
    app: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongodb
        image: mongo:5.0
        ports:
        - containerPort: 27017
---
# You can have multiple YAML configurations within 1 file
apiVersion: v1
kind: Service
metadata:
  name: mongo-service
spec:
  selector:
    app: mongo
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
8. Make it external service we define type using nodeport(external service type) default is ClusterIP(an internal service)




Problems

1. echo -n mongouser | base64 
Error
base64 : The term 'base64' is not recognized as the name of a cmdlet, 
function, script file, or operable program. Check the spelling of the name,    
or if a path was included, verify that the path is correct and try again.      
At line:1 char:21
+ echo -n mongouser | base64
+                     ~~~~~~
    + CategoryInfo          : ObjectNotFound: (base64:String) [], CommandNotF  
   oundException
    + FullyQualifiedErrorId : CommandNotFoundException

2. Why are we using configMapKeyRef instead of secretMapKeyRef
Ans: Maybe DB-URL or endpoint doesn't need secret.

