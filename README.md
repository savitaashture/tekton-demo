# Tekton Operator Demo for CdCon 2021

## Prerequisites:

- Install and verify nginx.
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.45.0/deploy/static/provider/cloud/deploy.yaml
```

```aidl
kubectl get pods -n ingress-nginx \
  -l app.kubernetes.io/name=ingress-nginx --watch
```

## Tekton installation

Tekton Operator installation of a previous version 0.22.0-3:
```
kubectl apply -f https://storage.googleapis.com/tekton-releases/operator/previous/v0.22.0-3/release.yaml
```

Check the status of Operator:
```
kubectl get deploy -n tekton-operator
```

Installation of Tekton Pipeline, Trigger and Dashboard with the custom resources:
```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/operator/main/config/crs/kubernetes/config/all/operator_v1alpha1_config_cr.yaml
```

Check the log of the Operator:
```
kubectl logs -f deploy/tekton-operator -n tekton-operator
```

Check the status of all Tekton components:
```
kubectl get TektonPipeline
kubectl get TektonTrigger
kubectl get TektonDashboard
```

After installing dashboard, install the ingress:
```
kubectl apply -f ingress.yaml
```

Visit http://localhost/dashboard for the dashboard portal.

Dashboard v0.15.0, Trigger v0.12.1 and Pipeline @v0.22.0.

## Deployment restore

Pick up a deployment resource of the pipeline, e.g. tekton-pipelines-controller.

Watch all the deployments:
```
kubectl get deploy -n tekton-pipelines -w
```

Remove this deployment resource with command:
```
kubectl delete deploy tekton-pipelines-controller -n tekton-pipelines
```

We expect to see the deployment resource is gone and relaunched.

## Testing Pipeline Example with Github repository

Install the example.
```
kubectl apply -f example/
```

Configure EL URL in the GitHub webhook setting

![Webhook Configuration](image/webhook.jpg)

Create a new PR for this repository.

Check locally with the command:
```
kubectl get taskruns | grep github-run-
```
to see the created TaskRun.

## Upgrade from v0.22.0-3 to v0.23.0-1

Install the operator v0.23.0-1
```
kubectl apply -f https://storage.googleapis.com/tekton-releases/operator/previous/v0.23.0-1/release.yaml
```

Check the Tekton components:
```
kubectl get TektonPipeline
kubectl get TektonTrigger
kubectl get TektonDashboard
```

Check the dashboard at http://localhost/dashboard.

Dashboard v0.16.1, Trigger v0.13.0 and Pipeline v0.23.0.

Open another new PR or update the existing PR for the repository to verify the upgraded tekton works.