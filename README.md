# Argo CD Demo Application

This repository contains a simple web application deployed using Argo CD, demonstrating GitOps principles and automated deployments.

## Overview

The application consists of:

- A simple web page served by Nginx
- Custom HTML content stored in a ConfigMap
- Automated deployment using Argo CD

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Argo CD CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

## Repository Structure

```
my-argocd-app/
├── k8s/
│   ├── configmap.yaml    # Custom HTML content
│   ├── deployment.yaml   # Nginx deployment
│   ├── service.yaml      # Service configuration
│   └── namespace.yaml    # Namespace definition
└── README.md
```

## Getting Started

1. **Start Minikube**

   ```bash
   minikube start
   ```

2. **Install Argo CD**

   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

3. **Access Argo CD UI**

   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```

   Then visit <https://localhost:8080> in your browser.

4. **Login to Argo CD**

   ```bash
   # Get the initial admin password
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```

   Username: `admin`
   Password: (from the command above)

5. **Create Argo CD Application**

   ```bash
   argocd app create argocd-demo-app \
     --repo https://github.com/your-username/my-argocd-app.git \
     --path k8s \
     --dest-server https://kubernetes.default.svc \
     --dest-namespace argocd-demo-app
   ```

6. **Enable Auto-Sync**

   ```bash
   argocd app set argocd-demo-app --sync-policy automated
   ```

7. **Access the Application**

   ```bash
   kubectl port-forward svc/argocd-demo-app -n argocd-demo-app 8081:80
   ```

   Then visit <http://localhost:8081> in your browser.

## Making Changes

1. **Update the HTML Content**
   - Edit `k8s/configmap.yaml`
   - Commit and push changes to Git
   - Argo CD will automatically sync the changes

2. **Update the Deployment**
   - Edit `k8s/deployment.yaml`
   - Commit and push changes to Git
   - Argo CD will automatically sync the changes

## Troubleshooting

1. **Check Application Status**

   ```bash
   argocd app get argocd-demo-app
   ```

2. **Check Pod Status**

   ```bash
   kubectl get pods -n argocd-demo-app
   ```

3. **Check ConfigMap**

   ```bash
   kubectl get configmap -n argocd-demo-app
   ```

4. **View Pod Logs**

   ```bash
   kubectl logs -n argocd-demo-app -l app=argocd-demo-app
   ```

## Cleanup

1. **Delete the Application**

   ```bash
   argocd app delete argocd-demo-app
   ```

2. **Delete the Namespace**

   ```bash
   kubectl delete namespace argocd-demo-app
   ```

3. **Stop Minikube**

   ```bash
   minikube stop
   ```

## Notes

- The application uses a NodePort service (port 8081) for external access
- Custom HTML content is stored in a ConfigMap and mounted to the Nginx container
- Auto-sync is enabled for automated deployments
- No need to run `minikube tunnel` for this setup

## Contributing

Feel free to submit issues and enhancement requests!
