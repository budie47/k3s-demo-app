# k3s demo app

A small nginx web page deployed to the k3s lab cluster by Argo CD (GitOps).
The page shows which pod served the request, so refreshing shows load balancing across replicas.

**URL:** http://demo.192.168.185.10.sslip.io

## Layout

```
k8s/                      # Argo CD syncs this folder (kustomize)
  app/index.html          # the web page
  app/default.conf        # nginx config
  deployment.yaml         # 2 replicas, nginx-unprivileged
  service.yaml, ingress.yaml, namespace.yaml
argocd/application.yaml   # the Argo CD Application (applied once)
```

## Make a change

Edit `k8s/app/index.html` (e.g. change `v1` to `v2`), commit and push to `main`.
Argo CD polls the repo every ~3 minutes (or click **Refresh** in the UI) and rolls the pods out;
the ConfigMap name hash changes with the content, which triggers the rolling update.

## Register with Argo CD

```bash
kubectl apply -f argocd/application.yaml
```
