# Prerequisites

- Kubernetes cluster (e.g., Minikube)
- Argo CD installed in the cluster
- kubectl configured to access the cluster
- Argo CD CLI installed locally

## Getting Started

1. **Clone the Repository**

   ```sh
   git clone https://github.com/tedckh/my-argocd-app.git
   cd my-argocd-app
   ```

2. **Create Argo CD Application**

   ```sh
   argocd app create argocd-demo-app \
     --repo https://github.com/tedckh/my-argocd-app.git \
     --path k8s \
     --dest-server https://kubernetes.default.svc \
     --dest-namespace argocd-demo-app
   ```

3. **Sync the Application**

   ```sh
   argocd app sync argocd-demo-app
   ```

4. **Access the Application**

   ```sh
   kubectl port-forward svc/argocd-demo-app -n argocd-demo-app 8081:80
   ```

   Then open your browser and go to: <http://localhost:8081>

## Application Components

### Namespace

- Name: `argocd-demo-app`
- Purpose: Isolates the application resources

### Deployment

- Name: `argocd-demo-app`
- Image: `nginx:latest`
- Replicas: 1
- Purpose: Runs the Nginx web server

### Service

- Name: `argocd-demo-app`
- Type: ClusterIP
- Port: 80
- Purpose: Exposes the Nginx server internally

## Monitoring and Management

### Check Application Status

```sh
argocd app get argocd-demo-app
```

### View Application Resources

```sh
kubectl get all -n argocd-demo-app
```

### View Application Logs

```sh
kubectl logs -n argocd-demo-app -l app=argocd-demo-app
```

## GitOps Workflow

1. Make changes to the Kubernetes manifests in the `k8s/` directory
2. Commit and push changes to the repository
3. Argo CD automatically detects changes and syncs the application
4. Monitor the sync status using `argocd app get argocd-demo-app`

## Troubleshooting

### Common Issues

1. **Application Out of Sync**
   - Check if the repository URL is correct
   - Verify the path to Kubernetes manifests
   - Ensure the namespace exists

2. **Pod Not Starting**
   - Check pod logs: `kubectl logs -n argocd-demo-app <pod-name>`
   - Verify image pull policy and image name

3. **Service Not Accessible**
   - Verify service configuration
   - Check if port forwarding is working
   - Ensure the pod is running

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.
EOF
