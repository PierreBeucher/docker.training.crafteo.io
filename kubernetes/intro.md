# Kubernetes introduction


K3S est une implémentation Kubernetes "lightweight" s'installant facilement. Elle permet de déployer des clusters avec un ou plusieurs noeuds en quelques minutes.

## Installer un petit cluster Kubernetes avec K3S

Se référer à [la documentation d'installation](https://rancher.com/docs/k3s/latest/en/quick-start/) ou lancer la commande:

```
# Install k3s
curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE=0644 sh -
```

Vérifier le fonctionnement de la CLI `kubectl`

```
kubectl version
kubectl get nodes
```

## Déploiement de l'application sur le cluster

Créons un Namespace `vote` pour y déployer l'application selon les mmanifests présent dans `resources/k8s`:

```
kubectl create ns vote
kubectl apply -f resources/k8s
```

Vérifier l'existence des pods:

```
kubectl -n vote get pods
```

L'application est exposé sur la node via:

```
# Vote 
pierre.training.crafteo.io:31000

# Result
pierre.training.crafteo.io:31001
```
