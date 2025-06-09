
#### Step 1: Verify Cluster and Current Namespace
1. First, confirm that your `kubectl` is configured correctly and you can access your cluster.
   ```
   sudo su
   ```
   ```bash
   kubectl get nodes
   ```

3. Check the available namespaces.
   ```bash
   kubectl get ns
   ```
   By default, the namespace is `default`.

---

#### Step 2: Create Namespaces
1. **Create a namespace called `dev`**.
   ```bash
   kubectl create namespace dev
   ```

2. **Create another namespace called `prod`**.
   ```bash
   kubectl create namespace prod
   ```

3. Verify the namespaces you just created.
   ```bash
   kubectl get namespaces
   ```

---

#### Step 3: Deploy Applications in Different Namespaces

1. **Deploy a sample Nginx application in the `dev` namespace**:
   ```bash
   kubectl run nginx-dev --image=nginx --namespace=dev
   ```

2. **Expose the Nginx deployment as a service in the `dev` namespace**:
   ```bash
   kubectl expose pod nginx-dev --port=80 --name=nginx-service --namespace=dev
   ```

3. **Deploy the same Nginx application in the `prod` namespace**:
   ```bash
   kubectl run nginx-prod --image=nginx --namespace=prod
   ```

4. **Expose the Nginx deployment as a service in the `prod` namespace**:
   ```bash
   kubectl expose pod nginx-prod --port=80 --name=nginx-service --namespace=prod
   ```

---

#### Step 4: List and Access Resources in Namespaces

1. **List all pods in the `dev` & `prod` namespace**:
   ```bash
   kubectl get pods --namespace=dev
   ```
    ```bash
   kubectl get pods --namespace=prod
   ```

2. **List all services in the `dev` & `prod` namespace**:
   ```bash
   kubectl get services --namespace=prod
   ```
    ```bash
   kubectl get services --namespace=dev
   ```

3. **Access the `dev` namespace Nginx service**:
   ```bash
   kubectl port-forward service/nginx-service 8080:80 --namespace=dev
   ```
   Visit `http://localhost:8080` in your browser to access the Nginx app.

4. **Access the `prod` namespace Nginx service** (in a new terminal window):
   ```bash
   kubectl port-forward service/nginx-service 8081:80 --namespace=prod
   ```
   Visit `http://localhost:8081` to access the Nginx app in the `prod` namespace.

---

#### Step 5: Delete Resources
1. **Delete the `dev` namespace**:
   ```bash
   kubectl delete namespace dev
   ```

2. **Delete the `prod` namespace**:
   ```bash
   kubectl delete namespace prod
   ```
#### Verify Deletion
```
kubectl get ns
```
---
