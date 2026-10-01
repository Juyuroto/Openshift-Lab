# Z03 — Corriger un YAML cassé

Trouver et corriger les erreurs d'un manifeste Kubernetes, puis le faire valider par le cluster sans rien créer.

## Ce qui est attendu

1. Trouver les **3 erreurs** du fichier sans l'exécuter.
2. Les corriger dans `vim`.
3. Vérifier le fichier avec `oc apply --dry-run=server -f`.

## Ce que j'ai fait

**Le fichier de départ (`broken.yaml`)**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: broken
 labels:
    app: test
spec:
  containers:
  - name: web
    image: quay.io/redhattraining/hello-world-nginx
      ports:
    - containerPort: "8080"
```

**Les 3 corrections**

| Ligne | Erreur | Correction |
|---|---|---|
| `labels:` | Indenté d'**un** espace au lieu de deux | Aligné sous `name` (2 espaces) |
| `ports:` | Trop indenté | Aligné sous `image` (même élément de liste) |
| `containerPort: "8080"` | Texte entre guillemets, alors que Kubernetes attend un **nombre** | `containerPort: 8080` |

**Le fichier corrigé**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: broken
  labels:
    app: test
spec:
  containers:
  - name: web
    image: quay.io/redhattraining/hello-world-nginx
    ports:
    - containerPort: 8080
```

**Validation par le cluster**

```bash
oc apply --dry-run=server -f broken.yaml
```

```
pod/broken created (server dry run)
```

Le cluster accepte le fichier, mais ne crée rien grâce au *dry run*.

**Ce que j'ai retenu**

- En YAML, l'indentation définit la hiérarchie : un espace de trop ou de moins casse le fichier.
- `--dry-run=server` fait valider le fichier par le cluster sans rien créer : idéal pour tester un manifeste.

## Erreurs rencontrées

| Erreur | Cause | Solution |
|---|---|---|
| `oc apply --dry-run=server -f` sans fichier | Oubli du nom du fichier après `-f` | `oc apply --dry-run=server -f broken.yaml` |