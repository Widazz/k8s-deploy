# Déploiement des applications K8S

Déploiement complet du cluster Kubernetes et de ses applications

## Prérequis

- Avoir un cluster fonctionnel en **version 1.23**
- Avoir helm installé ([installer helm](https://helm.sh/docs/intro/install/))
- Avoir au moins 8Go de ram au total (cumul des noeuds)

## Installation

Commencer par cloner ce dépôt dans le répertoire de votre choix :

```bash
git clone https://github.com/Widazz/k8s-deploy --branch diiage
```

Installer ensuite la chart située dans le dossier /app/metallb puis /app/argo-cd :

```bash
cd k8s-deploy/apps/metallb
helm install metallb . --values values.yaml -n metallb --create-namespace
cd ../argo-cd
helm install argocd . --values values.yaml -n argocd --create-namespace
```

ArgoCD se déploie alors dans le namespace argocd. Vérifier que tous les pods se lancent bien :

```bash
kubectl get pods -n argocd --watch
```

Une fois l'installation d'ArgoCD finalisée, tous les pods sont bien up :
```bash
NAME                                             READY   STATUS    RESTARTS   AGE
argocd-argo-cd-app-controller-75675b6bcc-jdn7q   1/1     Running   0          114s
argocd-argo-cd-repo-server-5f45d569c7-8jbbk      1/1     Running   0          113s
argocd-argo-cd-server-6955ccd9d5-dcq8t           1/1     Running   0          114s
argocd-redis-master-0                            1/1     Running   0          111s
```

Appliquer enfin le fichier suivant pour lancer toutes les applications sur ArgoCD :
```bash
kubectl apply -f https://raw.githubusercontent.com/Widazz/k8s-deploy/diiage/application.yaml
```

## Gérer les déploiements ArgoCD

