# Basic Authentication App

## Introduction
This repository contains a basic authentication application configured to run on the domain `local.basicauthtest.com` using Kubernetes and Minikube. When the user nagivates to the domain the browser asks the user username and password to log in. After logging in, the user will see an plain index with a placeholder text.

## Installation
### Requirements
- Git
- Docker
- Kubernetes
- Minikube
- `htpasswd` (for creating the authentication file)

### Running
#### Fetch files
Clone the repository:
```sh
# Clone
git clone https://github.com/yegekucuk/basicauth-kubernetes.git

# Change working directory
cd basicauth-kubernetes
```

#### Start Minikube
To start Minikube, use the following command:
```sh
# Start Minikube
minikube start
```

#### Enable Ingress
Enable the Ingress addon in Minikube:
```sh
# Enable Ingress
minikube addons enable ingress
```

If you encounter any errors, ensure that your Minikube is up to date and that you have sufficient resources allocated (e.g., CPU and memory).

#### Build the Authentication File
You can either use the provided `auth` file or create your own. To create your own `auth` file, use the following command:
```sh
# Create the auth file
htpasswd -c auth <your-username>
```

#### Create Kubernetes Secret
Use the `auth` file to create a Kubernetes secret:
```sh
# Create Kubernetes secret
kubectl create secret generic basicauth-secret --from-file=auth
```

#### Deploy the Application
Apply the deployment configuration:
```sh
# Apply deployment configuration
kubectl apply -f basicauth-deploy.yaml
```

#### Update Hosts File
Finally, update your hosts file to map the Minikube IP to `local.basicauthtest.com`.

**Windows:**
1. Open `C:\Windows\System32\drivers\etc\hosts` in a text editor with administrative privileges.
2. Add the following line, replacing `<minikube-ip>` with your Minikube IP:
    ```
    <minikube-ip> local.basicauthtest.com
    ```

**Linux:**
1. Open `/etc/hosts` in a text editor with sudo privileges.
2. Add the following line, replacing `<minikube-ip>` with your Minikube IP:
    ```
    <minikube-ip> local.basicauthtest.com
    ```

It can take a while for the image to be pulled from the DockerHub. If you want to see the status of the pod, you can run:
```sh
# With this command you can see the id of the pod.
kubectl get pods

# Then you can check the status of the pod with using the command below
kubectl describe pod <your-pod-id>
```
When the application is running, navigate to `http://local.basicauthtest.com` in your web browser to access the application.
If you use the `auth` file included in this repository to set up the application, the default username and password is:
+ `username`: ege
+ `password`: kucuk
