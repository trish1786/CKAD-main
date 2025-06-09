### Task : Set capabilities for a Container
With Linux capabilities, you can grant certain privileges to a process without granting all the privileges of the root user. To add or remove Linux capabilities for a Container, include the capabilities field in the securityContext section of the Container manifest.

Lets check, what happens when you don't include a capabilities field.
```
vi security-context-3.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-pod3
spec:
  containers:
  - name: sec-ctx-3
    image: gcr.io/google-samples/node-hello:1.0
```
Create the Pod:
```
kubectl apply -f security-context-3.yaml
```
Verify that the Pod's Container is running:
```
kubectl get pod security-context-pod3
```
Get a shell into the running Container:
```
kubectl exec -it security-context-pod3 -- sh
```
In your shell, list the running processes:
```
ps aux
```
Inside the pod, you can try to manipulate network-related settings, which require NET_ADMIN capability.
```
ip link set dev eth0 down
```
If you're able to execute this command without any errors, it indicates that the NET_ADMIN capability is working, allowing you to manipulate network interfaces.

You can also try to manipulate system time, which requires SYS_TIME capability.
```
date -s "2024-02-07 12:00:00"
```
If you're able to execute this command without any errors, it indicates that the SYS_TIME capability is working, allowing you to manipulate system time.
```
exit
```
Now, run a Container that is the same as the preceding container, except that it has additional capabilities set.

```
vi security-context-4.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-pod4
spec:
  containers:
  - name: sec-ctx-4
    image: gcr.io/google-samples/node-hello:1.0
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```
```
kubectl apply -f security-context-4.yaml
```
```
kubectl exec -it security-context-pod4 -- sh
```

Inside the pod, you can try to manipulate network-related settings, which require NET_ADMIN capability.
```
ip link set dev eth0 down
```
If you're able to execute this command without any errors, it indicates that the NET_ADMIN capability is working, allowing you to manipulate network interfaces.

You can also try to manipulate system time, which requires SYS_TIME capability.
```
date -s "2024-02-07 12:00:00"
```
If you're able to execute this command without any errors, it indicates that the SYS_TIME capability is working, allowing you to manipulate system time.
```
exit
```
