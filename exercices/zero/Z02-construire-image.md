# Z02 — Construire ta propre image

Créer une image personnalisée à partir d'une image Red Hat UBI, avec un `Containerfile`, puis la lancer.

## Ce qui est attendu

1. Créer un fichier `index.html` contenant « Bonjour depuis mon image ».
2. Écrire un `Containerfile` basé sur `ubi9/httpd-24` qui copie ce fichier dans `/var/www/html/`.
3. Construire l'image `monsite:1.0`, la lancer et vérifier avec `curl`.

## Ce que j'ai fait

**Écrire le `index.html`**

```bash
<!DOCTYPE html><html><body><h1>Bonjour depuis mon image</h1></body></html>
```

**Écrire le `Containerfile`**

```dockerfile
FROM registry.access.redhat.com/ubi9/httpd-24
COPY index.html /var/www/html/
```

**Construire l'image**

```bash
podman build -t monsite:1.0 .
podman images
```

**Lancer le conteneur en publiant le port 8080 et tester**

```bash
podman run -d --name monsite -p 8080:8080 monsite:1.0
curl localhost:8080
```

```
Bonjour depuis mon image
```

**Ce que j'ai retenu**

- `FROM` part d'une image existante, `COPY` y ajoute mes fichiers : l'image hérite du port 8080 et de l'utilisateur non-root de la base.
- Le port se publie au lancement avec `-p <port VM>:<port conteneur>`, pas dans le `Containerfile`.

## Erreurs rencontrées

| Erreur | Cause | Solution |
|---|---|---|
| Je pensais devoir déclarer un port dans le `Containerfile` | Confusion entre `EXPOSE` (simple documentation) et `-p` (publication réelle du port) | L'image de base écoute déjà sur 8080 ; publier le port au lancement avec `-p 8080:8080` |