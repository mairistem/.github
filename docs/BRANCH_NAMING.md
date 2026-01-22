# Conventions de Nommage des Branches

Ce document définit les conventions de nommage des branches Git pour tous les projets Mairistem.

## Format général

```
<type>/<ticket>-<description>
```

| Élément | Description | Exemple |
|---------|-------------|---------|
| `type` | Type de branche (voir ci-dessous) | `feature`, `fix`, `hotfix` |
| `ticket` | Référence JIRA (si applicable) | `PROJ-123` |
| `description` | Description courte en kebab-case | `user-authentication` |

## Types de branches

### Branches principales

| Branche | Description | Déploiement | Protection |
|---------|-------------|-------------|------------|
| `main` | Code en production | Cluster **Prod** | Protégée, merge via PR uniquement |
| `preprod` | Pré-production | Cluster **Preprod** | Protégée, merge via PR uniquement |
| `qualite` | Recette / QA | Cluster **Qualité** | Protégée, merge via PR uniquement |
| `develop` | Branche d'intégration | Cluster **Dev** | Protégée, merge via PR uniquement |

### Branches de travail

| Type | Usage | Exemple |
|------|-------|---------|
| `feature/` | Nouvelle fonctionnalité | `feature/PROJ-123-user-login` |
| `fix/` | Correction de bug | `fix/PROJ-456-null-pointer-exception` |
| `hotfix/` | Correction urgente en production | `hotfix/PROJ-789-security-patch` |
| `refactor/` | Refactoring sans changement fonctionnel | `refactor/PROJ-101-clean-api-service` |
| `docs/` | Documentation uniquement | `docs/PROJ-102-api-documentation` |
| `test/` | Ajout ou modification de tests | `test/PROJ-103-unit-tests-auth` |
| `chore/` | Tâches de maintenance | `chore/upgrade-dependencies` |

### Branches de release

| Type | Usage | Exemple |
|------|-------|---------|
| `release/` | Préparation d'une release | `release/1.2.0` |

## Workflow Git Flow et déploiement

```
                                    Clusters
                                    ────────
feature/ ──┐
fix/      ─┼──► develop ──► qualite ──► preprod ──► main
refactor/ ─┘        │          │           │          │
                    ▼          ▼           ▼          ▼
                   Dev      Qualité     Preprod     Prod
```

### Cycle de déploiement

| Étape | Branche | Cluster | Description |
|-------|---------|---------|-------------|
| 1 | `develop` | Dev | Intégration continue, tests automatisés |
| 2 | `qualite` | Qualité | Recette fonctionnelle, tests QA |
| 3 | `preprod` | Preprod | Validation finale, tests de charge |
| 4 | `main` | Prod | Production |

## Règles de nommage

### À faire

- Utiliser le **kebab-case** (minuscules avec tirets)
- Inclure la **référence JIRA** quand un ticket existe
- Garder la description **courte** (3-5 mots max)
- Utiliser des mots **descriptifs** et **significatifs**

### À éviter

- Les majuscules (sauf pour le ticket JIRA)
- Les underscores `_`
- Les caractères spéciaux
- Les descriptions trop longues
- Les noms génériques (`feature/fix`, `feature/update`)

## Exemples

### Avec ticket JIRA

```bash
# Features
feature/PROJ-123-add-user-authentication
feature/PROJ-124-implement-dashboard-charts
feature/PROJ-125-export-pdf-report

# Bug fixes
fix/PROJ-200-login-timeout-error
fix/PROJ-201-missing-validation
fix/PROJ-202-incorrect-date-format

# Hotfixes
hotfix/PROJ-300-critical-sql-injection
hotfix/PROJ-301-session-hijacking

# Refactoring
refactor/PROJ-400-simplify-api-calls
refactor/PROJ-401-extract-common-utils

# Documentation
docs/PROJ-500-update-readme
docs/PROJ-501-api-swagger-specs
```

### Sans ticket JIRA

Pour les tâches de maintenance ou documentation sans ticket :

```bash
chore/upgrade-dependencies
chore/update-nodejs-20
docs/fix-readme-typos
refactor/clean-unused-imports
```

### Releases

```bash
release/1.0.0
release/2.1.0-beta
```

### Exemples invalides

```bash
# Mauvais format
Feature/PROJ-123-Login               # ❌ Majuscules
feature/PROJ_123_login               # ❌ Underscores
feature/PROJ-123_add_user_auth       # ❌ Mix tirets et underscores

# Trop long
feature/PROJ-123-add-complete-user-authentication-with-oauth2-and-social-login  # ❌

# Trop vague
feature/PROJ-123-fix                 # ❌ Pas descriptif
feature/PROJ-123-update              # ❌ Pas descriptif
```

## Workflow Git

### Création d'une branche

```bash
# Se positionner sur develop
git checkout develop
git pull origin develop

# Créer la branche de travail
git checkout -b feature/PROJ-123-user-authentication
```

### Mise à jour depuis develop

```bash
# Mettre à jour develop
git checkout develop
git pull origin develop

# Rebaser la branche de travail
git checkout feature/PROJ-123-user-authentication
git rebase develop
```

### Finalisation

```bash
# Pousser la branche
git push -u origin feature/PROJ-123-user-authentication

# Ouvrir une Pull Request vers develop
```

## Cas particuliers

### Branches personnelles / expérimentales

Pour des expérimentations personnelles :

```bash
experiment/<username>-<description>
spike/<username>-<description>
```

Exemple : `experiment/jdupont-test-new-cache-strategy`

---

Pour toute question, consultez le [Guide de Contribution](../CONTRIBUTING.md).
