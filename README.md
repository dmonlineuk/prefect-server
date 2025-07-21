# prefect-server

Let's try to set up the following architecture:

1. podman - see ws-podman
1. kind - see ws-kind
1. postgres container
1. prefect helm chart (official one should be right)
1. setup basic auth
1. values.yaml (describing basic auth and connecting to postgres db)

## Progress

podman is running - ms sql, prefect and postgres containers all up and accessible
kind is running - kubernetes dashboard is up and accessible
postgres is up as above. Has db prefect-db, prefect user as owner
prefect self-service is working with basic auth via 

## specifics

```bash
helm repo add prefect https://prefecthq.github.io/prefect-helm
helm repo update

kubectl create namespace prefect-server
kubectl config set-context --current --namespace=prefect

kubectl create secret generic server-auth-secret \
  -n prefect-server \
  --from-literal auth-string='admin:P4s5W0rd'
```

server-values.yaml:
```yaml
server:
  basicAuth:
    enabled: true
    existingSecret: server-auth-secret
```

```bash
helm install prefect-server prefect/prefect-server \
  -n prefect-server \
  --values server-values.yaml

kubectl -n prefect-server port-forward svc/prefect-server 4200:4200
```
