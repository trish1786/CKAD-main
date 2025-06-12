## Helm

Download the shell script for the installation of the Helm package manager and run it.
```
wget  https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/kubernetes-essentials/helm.sh
```
```
bash helm.sh
```
To check the version of Helm installed
```
helm version
```

To add a Helm chart repository to your local environment. Below we are adding two Helm Chart Repositories.
```
helm repo add bitnami https://charts.bitnami.com/bitnami 
```
To list the Helm chart repositories configured in your local environment.
```
helm repo list
```

To search for Helm charts related to WordPress in the configured Helm repositories
```
helm search repo wordpress
```

To install the WordPress application using the Bitnami Helm chart with the release name my-wordpress
```
helm install my-wordpress bitnami/wordpress --set service.type=NodePort --set mariadb.primary.persistence.enabled=false
```
To list all the releases currently installed on your Kubernetes cluster
```
helm list
```
List all resources created
```
kubectl get all
```
You can install multiple helm charts from the same repo, but with unique name
```
helm install app-wordpress bitnami/wordpress --set service.type=NodePort --set mariadb.primary.persistence.enabled=false
```
To list all the releases currently installed on your Kubernetes cluster
```
helm list
```
List all resources created
```
kubectl get all
```
To uninstall the WordPress application deployed using Helm with the release name app-wordpress
```
helm uninstall app-wordpress
```

### Wordpress 
In the above steps we have already installed wordpress

Verify that the pods are running.
```
kubectl get pods
```
Now Notice that MariaDB (database) pods are part of statefulset.
```
kubectl get sts
```
```
kubectl describe sts
```
Also Notice that the front end of wordpress are part of deployment
```
kubectl get deploy
```
View the services using the below commands. 
```
kubectl get svc
```
View all the resources created by Helm
```
kubectl get all
```

Open the browser and paste the public ip of workernode : nodeport number. Observe that the WordPress site is up and running.

Add `/admin` at the end of the URL to be directed to the login page

Execute the below commands to get the user id and password. USe the same to log in to the Wordpress application that you have just deployed
```
echo Username: user
```
```
echo Password: $(kubectl get secret --namespace default my-wordpress -o jsonpath="{.data.wordpress-password}" | base64 -d)
```

To uninstall the WordPress application deployed using Helm with the release name my-wordpress
```
helm uninstall my-wordpress
```
Verify all resources are removed
```
kubectl get all
```
To remove a Helm repository from your local Helm client configuration
```
helm repo remove bitnami
```
Verify
```
helm repo list
```
To remove Helm package manager
```
sudo apt-get remove helm
```
Verify
```
helm version
```
 
 
