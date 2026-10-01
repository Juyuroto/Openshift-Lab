# Z04 — Ton premier Pod en YAML

Écrire un Pod à la main en YAML, le créer sur le cluster, l'inspecter, puis observer ce qui se passe quand on le supprime.

## Ce qui est attendu

1. Écrire `pod.yaml` : un pod `hello`, label `app=hello`, image `quay.io/redhattraining/hello-world-nginx`, port 8080.
2. L'appliquer, puis afficher son état, son IP et ses logs.
3. Le supprimer : revient-il ? Pourquoi ?

## Ce que j'ai fait

**Le fichier `pod.yaml`**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello
  labels:
    app: hello
spec:
  containers:
  - name: hello
    image: quay.io/redhattraining/hello-world-nginx
    ports:
    - containerPort: 8080
```

**Créer et inspecter le pod**

```bash
oc apply -f pod.yaml
oc get pod hello -o wide
oc get pods --show-labels
oc logs hello
```

```
NAME    READY   STATUS    RESTARTS   AGE     IP             NODE
hello   1/1     Running   0          2m41s   10.128.0.180   ocp-sno

NAME    READY   STATUS    RESTARTS   AGE     LABELS
hello   1/1     Running   0          2m57s   app=hello
```

**Supprimer le pod**

```bash
oc delete pod hello
oc get pods
```

```
pod "hello" deleted
No resources found
```

Le pod **ne revient pas** : créé seul, il n'est géré par aucun contrôleur, donc personne ne le recrée.

**Ce que j'ai retenu**

- Un pod seul n'est jamais recréé : en pratique on utilise un Deployment (exercice suivant).
- `oc apply` crée l'objet s'il n'existe pas (`created`) et le met à jour sinon (`configured`).

## Erreurs rencontrées

| Erreur | Cause | Solution |
|---|---|---|
| Label `app: hello` manquant | Oubli dans `metadata` | Ajouter `labels: app: hello` sous `metadata`, puis `oc apply` |
| `oc logs hello` ne renvoie rien | Ce serveur nginx n'écrit rien tant qu'il ne reçoit pas de requête | Pas une erreur : le pod est bien `Running` et `1/1` |