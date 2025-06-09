

### Task 1: Blue/Green Deployment in Kubernetes 
```
vi web-blue.yaml
```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-blue
  strategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: web-blue
    spec:
      containers:
      - image: mandarct/web-blue:v1
        name: web-blue
        ports:
        - containerPort: 80
          protocol: TCP
```
```		 
kubectl apply -f web-blue.yaml
```
```
kubectl get deploy,po
```


Now create NodePort service to access application
```		 
vi svc-web.yaml
```
```yaml
apiVersion: v1
kind: Service
metadata:
  name: svc-web
spec:
  ports:
  - port: 80        # Service port(internal)
    protocol: TCP
    targetPort: 80  # Container port
    nodePort: 32123 # Range from 30000 to 32767 . This is the external port number
  selector:
    app: web-blue
  type: NodePort

```
```	 
kubectl apply -f svc-web.yaml
```
```
kubectl get svc,ep
```

#### Access you application
```
http://workernodeip:32123
```

Create another deployment (Green)
```
vi web-green.yaml
```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-green
  strategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: web-green
    spec:
      containers:
      - image: mandarct/web-green:v1
        name: web-green
        ports:
        - containerPort: 80
          protocol: TCP
```
```
kubectl apply -f web-green.yaml
```
```
kubectl get deployment,po
```


Access this application using same service that we created previously, by changing the selector in the Service yaml file.

Replace Selector `web-blue` by `web-green`
```	 
kubectl replace -f svc-web.yaml --force
```
Access you application on the port 32123

---

### Task 2: Canary Deployment in Kubernetes 

Service and deployment should have a common label.
Replace selector->matchlabel & template->labels with `type: web-app` in the yaml file of service and both the deployments and replace all

```
kubectl replace -f web-green.yaml --force
```
```
kubectl replace -f web-blue.yaml --force
```
```
kubectl replace -f svc-web.yaml --force
```

Check all obejcts
```
kubectl get po,svc,ep
```

Describe the endpoint. You would notice all 6 pods Ip added to the end-point
```
kubectl describe ep svc-web
```

Access the application on port 32123. Refresh the page a few times. Sometime it will show Blue, other time it will show green. 

### Cleanup
```
kubectl delete deploy web-green
```
```
kubectl delete deploy web-blue
```
```
kubectl delete svc svc-web
```


