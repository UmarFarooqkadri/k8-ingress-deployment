## Create a Kind Cluster using below command

    cat <<EOF | kind create cluster --config=-
    kind: Cluster
    apiVersion: kind.x-k8s.io/v1alpha4
    nodes:
    - role: control-plane
      kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "ingress-ready=true"
      extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP
    EOF


## Install Nginx 

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Create deployment
    kubectl apply -f bootstrap-deployment.yaml

## Create Service
    kubectl apply -f bootstrap-service.yaml

## Create Ingress
    kubectl apply -f bootstrap-ingress.yaml
## Access using below URL

http://localhost/bootstrap/


**References:** 

https://kind.sigs.k8s.io/docs/user/ingress/