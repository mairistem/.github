# Conventions de Versioning et Tags

Ce document définit les conventions de versioning et de gestion des tags pour tous les projets Mairistem.

## Semantic Versioning (SemVer)

Nous suivons la spécification [Semantic Versioning 2.0.0](https://semver.org/).

### Format

```
v<MAJOR>.<MINOR>.<PATCH>[-<pre-release>]
```

| Élément | Description | Exemple |
|---------|-------------|---------|
| `MAJOR` | Changements incompatibles (breaking changes) | `2.0.0` |
| `MINOR` | Nouvelles fonctionnalités rétrocompatibles | `1.3.0` |
| `PATCH` | Corrections de bugs rétrocompatibles | `1.2.4` |
| `pre-release` | Version de pré-production (optionnel) | `1.0.0-rc.1` |

## Tags Git

### Format des tags

| Type | Format | Exemple | Cluster |
|------|--------|---------|---------|
| Dev | `v<MAJOR>.<MINOR>.<PATCH>-dev.<N>` | `v1.2.0-dev.1` | Dev |
| Qualité | `v<MAJOR>.<MINOR>.<PATCH>-qa.<N>` | `v1.2.0-qa.1` | Qualité |
| Release Candidate | `v<MAJOR>.<MINOR>.<PATCH>-rc.<N>` | `v1.2.0-rc.1` | Preprod |
| Release stable | `v<MAJOR>.<MINOR>.<PATCH>` | `v1.2.0` | Prod |

### Règles importantes

1. **Toujours préfixer avec `v`** : `v1.0.0` et non `1.0.0`
2. **Utiliser des tags annotés** : `git tag -a v1.0.0 -m "Release v1.0.0"`
3. **Ne jamais modifier un tag publié** : créer une nouvelle version

## Quand incrémenter ?

### MAJOR (x.0.0)

Incrémentez MAJOR quand vous faites des changements **incompatibles** :

- Suppression d'une API publique
- Modification du comportement d'une API existante
- Changement de schéma de base de données non rétrocompatible

```
v1.5.2 → v2.0.0
```

### MINOR (x.y.0)

Incrémentez MINOR quand vous ajoutez des **fonctionnalités rétrocompatibles** :

- Nouvelle API ou endpoint
- Nouvelle fonctionnalité utilisateur
- Dépréciation d'une fonctionnalité (sans suppression)

```
v1.5.2 → v1.6.0
```

### PATCH (x.y.z)

Incrémentez PATCH pour des **corrections rétrocompatibles** :

- Correction de bug
- Correction de faille de sécurité
- Amélioration de performance

```
v1.5.2 → v1.5.3
```

## Cycle de release et déploiement

### Workflow standard

Le versioning suit le gitflow avec les 4 clusters :

```
develop ────► qualite ────► preprod ────► main
   │            │             │            │
   ▼            ▼             ▼            ▼
  Dev        Qualité       Preprod       Prod
   │            │             │            │
   ▼            ▼             ▼            ▼
v1.2.0-dev.1  v1.2.0-qa.1  v1.2.0-rc.1  v1.2.0
```

### Détail du cycle

| Étape | Branche | Tag | Cluster | Description |
|-------|---------|-----|---------|-------------|
| 1 | `develop` | `v1.2.0-dev.N` | Dev | Intégration, tests automatisés |
| 2 | `qualite` | `v1.2.0-qa.N` | Qualité | Recette fonctionnelle |
| 3 | `preprod` | `v1.2.0-rc.N` | Preprod | Validation finale |
| 4 | `main` | `v1.2.0` | Prod | Production |

### Exemple de cycle complet

```
develop
    │
    ├──► v1.2.0-dev.1   → Déploiement cluster Dev
    │
    ├──► v1.2.0-dev.2   → Corrections, redéploiement Dev
    │
    └──► merge qualite
            │
            ├──► v1.2.0-qa.1   → Déploiement cluster Qualité
            │
            ├──► v1.2.0-qa.2   → Corrections après recette
            │
            └──► merge preprod
                    │
                    ├──► v1.2.0-rc.1   → Déploiement cluster Preprod
                    │
                    ├──► v1.2.0-rc.2   → Derniers ajustements
                    │
                    └──► merge main
                            │
                            └──► v1.2.0   → Déploiement Prod
```

### Hotfix workflow

Pour les corrections urgentes en production :

```
main (v1.2.0)
    │
    └──► hotfix/PROJ-XXX-critical-bug
            │
            ├──► v1.2.1-rc.1   → Test rapide en Preprod
            │
            └──► v1.2.1        → Déploiement Prod
                    │
                    └──► merge dans develop, qualite, preprod
```

## Création d'un tag

### Tag annoté (recommandé)

```bash
# Tag de développement
git tag -a v1.2.0-dev.1 -m "Dev release v1.2.0-dev.1"

# Tag de qualité
git tag -a v1.2.0-qa.1 -m "QA release v1.2.0-qa.1"

# Tag release candidate (preprod)
git tag -a v1.2.0-rc.1 -m "Release candidate v1.2.0-rc.1"

# Tag de production
git tag -a v1.2.0 -m "Release v1.2.0"

# Pousser le tag
git push origin v1.2.0
```

## Changelog

### Format CHANGELOG.md

Nous suivons le format [Keep a Changelog](https://keepachangelog.com/).

```markdown
# Changelog

## [Unreleased]

### Added
- Nouvelle fonctionnalité X

### Fixed
- Correction du bug Y

## [1.2.0] - 2024-01-15

### Added
- Ajout de l'authentification OAuth2 (PROJ-123)

### Fixed
- Correction du timeout de session (PROJ-200)

## [1.1.0] - 2024-01-01
...
```

### Catégories

| Catégorie | Description |
|-----------|-------------|
| `Added` | Nouvelles fonctionnalités |
| `Changed` | Modifications de fonctionnalités existantes |
| `Deprecated` | Fonctionnalités bientôt supprimées |
| `Removed` | Fonctionnalités supprimées |
| `Fixed` | Corrections de bugs |
| `Security` | Corrections de sécurité |

## Ordre de précédence des versions

```
v1.0.0-dev.1 < v1.0.0-dev.2 < v1.0.0-qa.1 < v1.0.0-qa.2 < v1.0.0-rc.1 < v1.0.0-rc.2 < v1.0.0
```

---

Pour toute question, consultez le [Guide de Contribution](../CONTRIBUTING.md).
