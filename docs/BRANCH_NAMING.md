# Conventions de Nommage des Branches

Ce document définit les conventions de nommage des branches Git pour tous les projets Mairistem.

## Format général

```
<type>/<ticket>-<description>
```

| Élément | Description | Exemple |
|---------|-------------|---------|
| `type` | Type de branche (voir ci-dessous) | `feature`, `fix`, `hotfix` |
| `ticket` | Référence JIRA | `PROJ-123` |
| `description` | Description courte en kebab-case | `user-authentication` |

## Types de branches

### Branches principales

| Branche | Description | Protection |
|---------|-------------|------------|
| `main` | Code en production | Protégée, merge via PR uniquement |
| `develop` | Branche d'intégration | Protégée, merge via PR uniquement |

### Branches de travail

| Type | Usage | Exemple |
|------|-------|---------|
| `feature/` | Nouvelle fonctionnalité | `feature/PROJ-123-user-login` |
| `fix/` | Correction de bug | `fix/PROJ-456-null-pointer-exception` |
| `hotfix/` | Correction urgente en production | `hotfix/PROJ-789-security-patch` |
| `refactor/` | Refactoring sans changement fonctionnel | `refactor/PROJ-101-clean-api-service` |
| `docs/` | Documentation uniquement | `docs/PROJ-102-api-documentation` |
| `test/` | Ajout ou modification de tests | `test/PROJ-103-unit-tests-auth` |
| `chore/` | Tâches de maintenance | `chore/PROJ-104-upgrade-dependencies` |

### Branches de release

| Type | Usage | Exemple |
|------|-------|---------|
| `release/` | Préparation d'une release | `release/1.2.0` |

## Règles de nommage

### À faire

- Utiliser le **kebab-case** (minuscules avec tirets)
- Toujours inclure la **référence JIRA**
- Garder la description **courte** (3-5 mots max)
- Utiliser des mots **descriptifs** et **significatifs**

### À éviter

- Les majuscules (sauf pour le ticket JIRA)
- Les underscores `_`
- Les caractères spéciaux
- Les descriptions trop longues
- Les noms génériques (`feature/fix`, `feature/update`)

## Exemples

### Exemples valides

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

# Releases
release/1.0.0
release/2.1.0-beta
```

### Exemples invalides

```bash
# Pas de ticket JIRA
feature/add-login                    # ❌ Manque le ticket

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

## Workflow Git Flow

```
main ─────────────────────────────────────────────► (production)
  │                                        ▲
  │                                        │ merge
  ▼                                        │
develop ──────────────────────────────► release/1.0.0
  │         ▲         ▲         ▲
  │         │         │         │
  ▼         │         │         │
feature/   fix/    refactor/   docs/
```

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

### Sans ticket JIRA

Dans de rares cas où il n'y a pas de ticket JIRA (ex: maintenance interne), utilisez :

```bash
chore/no-ticket-upgrade-nodejs-20
docs/no-ticket-fix-typos
```

### Branches personnelles / expérimentales

Pour des expérimentations personnelles :

```bash
experiment/<username>-<description>
spike/<username>-<description>
```

Exemple : `experiment/jdupont-test-new-cache-strategy`

## Outils et automatisation

### Hooks Git

Vous pouvez configurer un hook pre-commit pour valider le nom de branche :

```bash
#!/bin/bash
# .git/hooks/pre-push

branch=$(git rev-parse --abbrev-ref HEAD)
pattern="^(feature|fix|hotfix|refactor|docs|test|chore|release)\/[A-Z]+-[0-9]+-[a-z0-9-]+$|^(main|develop)$|^release\/[0-9]+\.[0-9]+\.[0-9]+(-[a-z]+\.[0-9]+)?$"

if [[ ! $branch =~ $pattern ]]; then
    echo "Erreur: Le nom de branche '$branch' ne respecte pas les conventions."
    echo "Format attendu: type/PROJ-XXX-description"
    exit 1
fi
```

---

Pour toute question, consultez le [Guide de Contribution](../CONTRIBUTING.md).
