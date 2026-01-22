# Conventions de Versioning et Tags

Ce document définit les conventions de versioning et de gestion des tags pour tous les projets Mairistem.

## Semantic Versioning (SemVer)

Nous suivons la spécification [Semantic Versioning 2.0.0](https://semver.org/).

### Format

```
v<MAJOR>.<MINOR>.<PATCH>[-<pre-release>][+<build>]
```

| Élément | Description | Exemple |
|---------|-------------|---------|
| `MAJOR` | Changements incompatibles (breaking changes) | `2.0.0` |
| `MINOR` | Nouvelles fonctionnalités rétrocompatibles | `1.3.0` |
| `PATCH` | Corrections de bugs rétrocompatibles | `1.2.4` |
| `pre-release` | Version de pré-production (optionnel) | `1.0.0-beta.1` |
| `build` | Métadonnées de build (optionnel) | `1.0.0+20240115` |

## Tags Git

### Format des tags

| Type | Format | Exemple |
|------|--------|---------|
| Release stable | `v<MAJOR>.<MINOR>.<PATCH>` | `v1.2.3` |
| Alpha | `v<MAJOR>.<MINOR>.<PATCH>-alpha.<N>` | `v1.2.3-alpha.1` |
| Beta | `v<MAJOR>.<MINOR>.<PATCH>-beta.<N>` | `v1.2.3-beta.2` |
| Release Candidate | `v<MAJOR>.<MINOR>.<PATCH>-rc.<N>` | `v1.2.3-rc.1` |

### Règles importantes

1. **Toujours préfixer avec `v`** : `v1.0.0` et non `1.0.0`
2. **Utiliser des tags annotés** : `git tag -a v1.0.0 -m "Release v1.0.0"`
3. **Ne jamais modifier un tag publié** : créer une nouvelle version
4. **Ordre de pré-release** : alpha < beta < rc < stable

## Quand incrémenter ?

### MAJOR (x.0.0)

Incrémentez MAJOR quand vous faites des changements **incompatibles** :

- Suppression d'une API publique
- Modification du comportement d'une API existante
- Changement de schéma de base de données non rétrocompatible
- Changement majeur d'architecture

```
v1.5.2 → v2.0.0
```

### MINOR (x.y.0)

Incrémentez MINOR quand vous ajoutez des **fonctionnalités rétrocompatibles** :

- Nouvelle API ou endpoint
- Nouvelle fonctionnalité utilisateur
- Nouvelle option de configuration
- Dépréciation d'une fonctionnalité (sans suppression)

```
v1.5.2 → v1.6.0
```

### PATCH (x.y.z)

Incrémentez PATCH pour des **corrections rétrocompatibles** :

- Correction de bug
- Correction de faille de sécurité
- Amélioration de performance
- Correction de documentation

```
v1.5.2 → v1.5.3
```

## Cycle de release

### Workflow standard

```
develop
    │
    ├──► v1.2.0-alpha.1  (première version testable)
    │
    ├──► v1.2.0-alpha.2  (corrections)
    │
    ├──► v1.2.0-beta.1   (feature complete, tests)
    │
    ├──► v1.2.0-beta.2   (corrections)
    │
    ├──► v1.2.0-rc.1     (release candidate)
    │
    ├──► v1.2.0-rc.2     (dernières corrections)
    │
    └──► v1.2.0          (release stable)
            │
            └──► merge dans main
```

### Hotfix workflow

```
main (v1.2.0)
    │
    └──► hotfix/PROJ-XXX-critical-bug
            │
            └──► v1.2.1 (patch release)
```

## Création d'un tag

### Tag annoté (recommandé)

```bash
# Créer un tag annoté avec message
git tag -a v1.2.0 -m "Release v1.2.0

## Nouveautés
- Ajout de la fonctionnalité X
- Amélioration de Y

## Corrections
- Fix du bug Z"

# Pousser le tag
git push origin v1.2.0
```

### Tag de pré-release

```bash
# Alpha
git tag -a v1.2.0-alpha.1 -m "Alpha release v1.2.0-alpha.1"

# Beta
git tag -a v1.2.0-beta.1 -m "Beta release v1.2.0-beta.1"

# Release candidate
git tag -a v1.2.0-rc.1 -m "Release candidate v1.2.0-rc.1"
```

### Pousser tous les tags

```bash
# Pousser un tag spécifique
git push origin v1.2.0

# Pousser tous les tags (à utiliser avec précaution)
git push --tags
```

## Changelog

### Format CHANGELOG.md

Nous suivons le format [Keep a Changelog](https://keepachangelog.com/).

```markdown
# Changelog

## [Unreleased]

### Added
- Nouvelle fonctionnalité X

### Changed
- Modification du comportement Y

### Deprecated
- Fonctionnalité Z dépréciée

### Removed
- Suppression de la fonctionnalité W

### Fixed
- Correction du bug V

### Security
- Correction de la faille U

## [1.2.0] - 2024-01-15

### Added
- Ajout de l'authentification OAuth2 (PROJ-123)
- Nouveau dashboard utilisateur (PROJ-124)

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

## Exemples de versions

### Projet web classique

```
v0.1.0       # MVP initial
v0.2.0       # Ajout authentification
v0.3.0       # Ajout dashboard
v1.0.0       # Première release stable
v1.0.1       # Bugfix
v1.1.0       # Nouvelle fonctionnalité
v2.0.0       # Refonte majeure
```

### API

```
v1.0.0       # API v1 stable
v1.1.0       # Nouveaux endpoints
v1.2.0       # Nouveaux endpoints
v2.0.0       # Breaking changes (nouvelle version API)
```

### Librairie/Package

```
v0.0.1       # Version initiale (instable)
v0.1.0       # Première version utilisable
v1.0.0       # API publique stable
```

## Comparaison de versions

L'ordre de précédence est défini par SemVer :

```
1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0
```

## Outils recommandés

### Génération automatique de changelog

- [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog)
- [semantic-release](https://github.com/semantic-release/semantic-release)
- [standard-version](https://github.com/conventional-changelog/standard-version)

### Validation de version

```bash
# Script de validation
#!/bin/bash
version=$1
pattern="^v[0-9]+\.[0-9]+\.[0-9]+(-[a-z]+\.[0-9]+)?$"

if [[ $version =~ $pattern ]]; then
    echo "Version valide: $version"
else
    echo "Version invalide: $version"
    exit 1
fi
```

---

Pour toute question, consultez le [Guide de Contribution](../CONTRIBUTING.md).
