# Z01 — Premier conteneur avec Podman

Lancer un premier conteneur à partir d'une image Red Hat UBI, entrer dedans et vérifier sous quel utilisateur il tourne.

## Ce qui est attendu

1. Lancer en arrière-plan un conteneur `web` depuis `registry.access.redhat.com/ubi9/httpd-24`, en publiant le port 8080.
2. Le tester avec `curl` et lire ses logs.
3. Ouvrir un shell dedans et trouver sous quel utilisateur tourne le processus.
4. Arrêter et supprimer le conteneur.

## Ce que j'ai fait

**Récupérer l'image et lancer le conteneur sur le port 8080**

```bash
podman pull registry.access.redhat.com/ubi9/httpd-24
podman run -d --name web -p 8080:8080 registry.access.redhat.com/ubi9/httpd-24
```

**Ouvrir un shell dans le conteneur et lister les processus**

```bash
podman exec -it web bash
ps aux
```

```
USER         PID ... COMMAND
default        1 ... httpd -D FOREGROUND
default       47 ... httpd -D FOREGROUND
default      226 ... /bin/bash
```

Tous les processus, y compris `httpd` (PID 1), tournent sous l'utilisateur **`default`**, pas sous `root`.

**Vérifier l'identité de l'utilisateur**

```bash
id
```

```
uid=1001(default) gid=0(root) groups=0(root)
```

**Ce que j'ai retenu**

- L'image UBI ne tourne pas en root et écoute sur 8080 : elle est donc compatible OpenShift.
- `gid=0` n'est pas un problème : seul l'UID 0 est root.

## Erreurs rencontrées

| Erreur | Cause | Solution |
|---|---|---|
| J'ai cherché un processus nommé `id` dans la sortie de `ps aux` | `id` n'est pas un processus mais une **commande**, qui affiche l'utilisateur courant | Taper `id` directement dans le shell du conteneur |
| Doute sur `gid=0(root)` : le conteneur est-il en root ? | Confusion entre l'**UID** (l'utilisateur) et le **GID** (le groupe) | Seul `uid=0` signifie root. Ici `uid=1001` : le conteneur n'est pas root |