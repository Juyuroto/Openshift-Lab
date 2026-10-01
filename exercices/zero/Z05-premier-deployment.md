# Z05 — Ton premier Deployment

Remplacer le pod seul par un Deployment, qui maintient automatiquement le nombre de pods voulu, et observer l'auto-réparation.

## Ce qui est attendu

1. Écrire `deploy.yaml` : Deployment `hello`, 2 réplicas, image `quay.io/redhattraining/hello-world-nginx`.
2. L'appliquer, puis lister le Deployment, le ReplicaSet et les pods.
3. Supprimer un pod et observer ce qui se passe.
4. Passer à 4 réplicas en modifiant le fichier puis en le réappliquant.

## Ce que j'ai fait

**Le fichier `deploy.yaml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
  labels:
    app: hello
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello
        image: quay.io/redhattraining/hello-world-nginx
        ports:
        - containerPort: 8080
```

**Créer le Deployment**

```bash
oc apply -f deploy.yaml
oc get deploy
```

```
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
hello   2/2     2            2           24s
```

**Supprimer un pod du Deployment**

```bash
oc delete pod hello-649cc97fb4-l95kd
oc get pods -w
```

```
NAME                     READY   STATUS    RESTARTS   AGE
hello-649cc97fb4-hqwn4   1/1     Running   0          5s
hello-649cc97fb4-pzz9x   1/1     Running   0          106s
```

Un nouveau pod (`hqwn4`, 5 s) remplace aussitôt celui supprimé. En supprimant les 2 pods en même temps, le résultat est identique : 2 nouveaux pods sont recréés.

**Ce que j'ai retenu**

- Le Deployment crée un ReplicaSet (`hello-649cc97fb4`), qui maintient en permanence le nombre de pods voulu : c'est la boucle de réconciliation.
- Pour arrêter l'application, on change l'état désiré (`oc scale deploy/hello --replicas=0`) ou on supprime le Deployment, pas les pods.

## Erreurs rencontrées

| Erreur | Cause | Solution |
|---|---|---|
| Premier YAML refusé (7 erreurs) | `kind: ReplicaSet` avec `apiVersion: v1`, `specs` au lieu de `spec`, `matchExpression` sans « s », faute de frappe dans un label, `template.spec` mal indenté, labels du template différents du sélecteur | `apiVersion: apps/v1`, `kind: Deployment`, labels identiques sous `selector.matchLabels` et `template.metadata.labels`, indentation à 2 espaces |
| `oc delete pod hello` ne change rien au Deployment | J'ai supprimé le pod seul de Z04, pas un pod du Deployment | Les pods d'un Deployment ont un nom généré (`hello-<replicaset>-<id>`) : `oc get pods` pour les voir |