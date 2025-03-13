# Directory

## Votre repository git source

Créer votre repository git `argocd-directory` sous github ou gitlab. Assurez-vous qu'il soit public.

Créer un repertoire base pour stocker les fichiers de base de votre application : 

```
argocd-directory/
└── manifests/
    ├── deployment.yaml
    ├── exposition
      ├── ingress.yaml
      ├── service.yaml
    └── rbac
      ├── role.yaml
      ├── rolebinding.yaml
```

Les fichiers sont disponibles dans le repertoire `manifests`. Regardez les

## Création de l'application ArgoCD

Inspirez vous du stanza suivant pour créer votre application et appliquer la sur le cluster :

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: httpbin
  namespace: argocd
spec:
  destination:
    namespace: httpbin
    server: https://kubernetes.default.svc
  project: default
  source:
    path: TODO
    repoURL: TODO
    targetRevision: HEAD
```

`kubectl apply -f application.yaml`

Que constatez-vous ? Quelles ressources sont appliquées ?

Vous pouvez utiliser le sync pour forcer la synchronisation par Argo.

## Gestion de la recursion

Modifiez votre Application et installez les manifestes liés à l'exposition et aux rbac. Vous pouvez vous aider de la commande `kubectl explain application.spec.source.directory`

Vous obtenez une erreur ? Essayez de la corriger !

Une fois toutes les ressources synchronisées, vous pouvez valider le bon fonctionnement de l'application en faisant un `curl localhost:8080/httpbin/headers`

## Modification du repertoire

Passer le nombre de replicas à 3 sur votre déploiement en faisant un commit. Syncez avec argo si nécessaire

Vérifiez sur le cluster le bon nombre de pods

## Include/Exclude

Modifiez votre Application pour ne pas prendre en compte le dossier `rbac`.

Vous pouvez utiliser le stanza suivant : 

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
spec:
  source:
    directory:
      exclude: 'TODO' #Optionnal
      include: 'TODO' #Optionnal
```

Modifiez une dernière fois votre Application pour installer le fichier `role.yaml`, mais pas le fichier `rolebinding.yaml`. Plusieurs solutions sont possibles.

Si les 2 directives `include` et `exclude` sont positionnées, alors l'Application inclura tous les fichiers qui correspondent au pattern d'include et qui ne correspondent pas au pattern d'exclude

## Cleanup

Supprimez votre application en passant par l'IHM, Cliquez sur le bouton `Delete`