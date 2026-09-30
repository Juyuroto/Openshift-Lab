# OpenShift — Préparation à la certification Red Hat (niveau 1)

Mon dépôt d'apprentissage pour préparer la certification **Red Hat Certified Technologist in OpenShift** (examen **EX180**, cours **DO180**).

On y trouve les guides de cours, les exercices corrigés et mes notes. Tout est pratiqué sur mon homelab Proxmox.

## Objectif

- Partir de zéro sur les conteneurs, le YAML et Kubernetes.
- Maîtriser le déploiement, l'exposition, la configuration, le stockage et la fiabilité des applications sur OpenShift.
- Réussir l'examen pratique EX180 (2 h, sur un cluster réel), puis viser l'EX280.

## Contenu

```
.
├── docs/
│   ├── OpenShift-Les-Bases-Niveau-Zero.pdf        # Fondamentaux : conteneurs, YAML, Kubernetes, OpenShift
│   └── OpenShift-Niveau1-DO180-EX180-Guide.pdf     # Préparation à l'examen : objectifs, 30 exercices, examen blanc
├── exercices/
│   └── zero/                                       # Explications des exercices Z01 → Z13
└── README.md
```

> **Note :** les deux PDF du dossier `docs/` ont été **entièrement générés par Claude** (Anthropic). Ce sont des supports d'étude personnels, non officiels et non affiliés à Red Hat.

## Mon lab

| Machine | Rôle |
|---|---|
| `lab-linux` (RHEL 9) | Podman, Containerfile, YAML, `vim` |
| `ocp-sno` (OpenShift 4.22, Single Node) | Kubernetes et OpenShift : tous les exercices sur cluster |

## Exercices — Les bases (niveau zéro)

Exercices du PDF *OpenShift-Les-Bases-Niveau-Zero*. Chaque lien mène à l'explication pas à pas pour réussir l'exercice.

| # | Exercice | Thème | Explication |
|---|---|---|---|
| Z01 | Premier conteneur avec Podman | Conteneurs | [Z01](exercices/zero/Z01-premier-conteneur.md) |
| Z02 | Construire ta propre image | Conteneurs | [Z02](exercices/zero/Z02-construire-image.md) |
| Z03 | Corriger un YAML cassé | YAML | [Z03](exercices/zero/Z03-yaml-casse.md) |
| Z04 | Ton premier Pod en YAML | Pods | [Z04](exercices/zero/Z04-premier-pod.md) |
| Z05 | Ton premier Deployment | Deployments | [Z05](exercices/zero/Z05-premier-deployment.md) |
| Z06 | Un Service et son nom DNS | Réseau | [Z06](exercices/zero/Z06-service.md) |
| Z07 | Une Route vers ton navigateur | Réseau | [Z07](exercices/zero/Z07-route.md) |
| Z08 | Casser et réparer les labels | Labels | [Z08](exercices/zero/Z08-labels.md) |
| Z09 | ConfigMap et Secret | Configuration | [Z09](exercices/zero/Z09-configmap-secret.md) |
| Z10 | Un volume qui survit | Stockage | [Z10](exercices/zero/Z10-volume-persistant.md) |
| Z11 | Sondes et ressources | Fiabilité | [Z11](exercices/zero/Z11-sondes-ressources.md) |
| Z12 | Mettre à jour et revenir en arrière | Mises à jour | [Z12](exercices/zero/Z12-rollout-rollback.md) |
| Z13 | L'application complète en un fichier | Synthèse | [Z13](exercices/zero/Z13-application-complete.md) |

## Progression

- [ ] Les bases : Z01 → Z13
- [ ] Guide DO180 / EX180 : E01 → E30
- [ ] Examen blanc réussi en moins de 2 h
- [ ] Certification EX180

## Ressources

- [Page officielle de l'examen EX180](https://www.redhat.com/en/services/training/ex180-red-hat-certified-technologist-in-openshift-exam)
- [Documentation OpenShift](https://docs.redhat.com/en/documentation/openshift_container_platform/)
- [Labs interactifs Red Hat](https://developers.redhat.com/learn/openshift)