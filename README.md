# Gitops ArgoCD Multi-Environment

App of Apps pattern with Dev, QA, and Prod environments.

## Structure
```
apps/dev/deploy.yml      # 1 replica, latest image, ClusterIP
apps/qa/deploy.yml       # 2 replicas, latest image, ClusterIP
apps/prod/deploy.yml     # 3 replicas, versioned image, LoadBalancer
argocd/app-of-apps.yml   # Parent app — apply ONCE manually
argocd/apps/dev.yml      # Child app — Dev
argocd/apps/qa.yml       # Child app — QA
argocd/apps/prod.yml     # Child app — Prod
```

## Deploy

```bash
# Apply ONCE — ArgoCD handles everything else
kubectl apply -f argocd/app-of-apps.yml -n argocd
```

## Flow
Developer pushes → GitHub Actions builds & pushes image
→ Updates deploy.yml in Git → ArgoCD auto syncs
→ dev deploys → qa promotes → prod waits for approval
