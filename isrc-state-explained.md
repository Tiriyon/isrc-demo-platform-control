# Repositories:

- bootstrap
- child management repositories (Arutzimng, callpostflow...)
- deployment repositories (contains values for helm charts)

## Bootstrap repo

Currently only delivers `root-app.yaml` which is a seed app to deploy from child management repositories:

<!-- ./root-app.yaml-->

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: bootstrap-arutzimng
  namespace: openshift-gitops
spec:
  project: gitops
  source:
    repoURL: ssh://git@<base-bitbucket-url>/gp/arutzimng.git # child repo
    targetRevision: HEAD
    path: .
  destination:
    server: https://kubernetes.default.svc
    namespace: openshift-gitops
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
---
# NOTE: I don't need this repo for now
#apiVersion: argoproj.io/v1alpha1
#kind: Application
#metadata:
#  name: bootstrap-callpostflow
#  namespace: openshift-gitops
#spec:
#  project: gitops
#  source:
#    repoURL: ssh://git@<base-bitbucket-url>/gp/callpostflow.git # another child repo
#    targetRevision: HEAD
#    path: .
#  destination:
#    server: https://kubernetes.default.svc
#    namespace: openshift-gitops
#  syncPolicy:
#    automated:
#      prune: true
#      selfHeal: true
---
# NOTE: I don't need this repo
#apiVersion: argoproj.io/v1alpha1
#kind: Application
#metadata:
#  name: bootstrap-canlyzer
#  namespace: openshift-gitops
#spec:
#  project: gitops
#  source:
#    repoURL: ssh://git@<base-bitbucket-url>/gp/canlyzer.git # another child repo
#    targetRevision: HEAD
#    path: .
#  destination:
#    server: https://kubernetes.default.svc
#    namespace: openshift-gitops
#  syncPolicy:
#    automated:
#      prune: true
#      selfHeal: true
```

This application points to the child repository and essentially laods anything that happens there `.spec.source.path: .` which is a very very large and dangerous blast radius.

The first object to be consumed is <!-- ../isrc-demo-app-catalog/root.yaml --> `root.yaml` - an application that is set in the root of the child management repository arutzhimng. This is yet another 
