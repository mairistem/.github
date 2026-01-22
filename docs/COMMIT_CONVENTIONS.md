# Conventions de Commits

Ce document définit les conventions de messages de commit pour tous les projets Mairistem, basées sur [Conventional Commits](https://www.conventionalcommits.org/).

## Format

```
<type>(<scope>): <description>

[body]

[footer(s)]
```

### Structure

| Partie | Obligatoire | Description |
|--------|-------------|-------------|
| `type` | Oui | Type de changement |
| `scope` | Non | Portée du changement |
| `description` | Oui | Description courte |
| `body` | Non | Description détaillée |
| `footer` | Non | Métadonnées (breaking changes, références) |

## Types de commits

| Type | Description | Exemple |
|------|-------------|---------|
| `feat` | Nouvelle fonctionnalité | `feat(auth): add OAuth2 login` |
| `fix` | Correction de bug | `fix(api): resolve timeout issue` |
| `docs` | Documentation | `docs(readme): update installation steps` |
| `style` | Formatage (pas de changement de code) | `style(css): fix indentation` |
| `refactor` | Refactoring (pas de fix ni feat) | `refactor(utils): simplify date parsing` |
| `perf` | Amélioration de performance | `perf(query): optimize database queries` |
| `test` | Ajout ou modification de tests | `test(auth): add unit tests for login` |
| `build` | Build system, dépendances | `build(deps): upgrade lodash to 4.17.21` |
| `ci` | Configuration CI/CD | `ci(github): add deploy workflow` |
| `chore` | Tâches de maintenance | `chore: update .gitignore` |
| `revert` | Annulation d'un commit | `revert: feat(auth): add OAuth2 login` |

## Scope

Le scope indique la partie du code affectée. Il est optionnel mais recommandé.

### Exemples de scopes

| Projet | Scopes possibles |
|--------|------------------|
| API | `auth`, `users`, `api`, `db`, `middleware` |
| Frontend | `ui`, `components`, `pages`, `store`, `hooks` |
| Infra | `docker`, `k8s`, `helm`, `ci`, `terraform` |

## Description

La description doit :

- Être en **anglais** (termes techniques)
- Commencer par une **minuscule**
- Utiliser l'**impératif** présent ("add" et non "added" ou "adds")
- Ne pas dépasser **72 caractères**
- Ne pas se terminer par un point

### Verbes courants

| Verbe | Usage |
|-------|-------|
| `add` | Ajouter une fonctionnalité/fichier |
| `remove` | Supprimer |
| `update` | Mettre à jour |
| `fix` | Corriger |
| `refactor` | Restructurer |
| `rename` | Renommer |
| `move` | Déplacer |
| `improve` | Améliorer |
| `simplify` | Simplifier |
| `implement` | Implémenter |

## Body (corps)

Le body est optionnel et permet de :
- Expliquer le **contexte** du changement
- Décrire le **pourquoi** (pas le comment)
- Mentionner les **effets de bord**

```
feat(auth): add two-factor authentication

Implement TOTP-based 2FA to enhance account security.
Users can now enable 2FA from their profile settings.

This change requires the google-authenticator library.
```

## Footer

### Références JIRA

```
feat(dashboard): add export functionality

Refs: PROJ-123
```

### Breaking Changes

Pour les changements incompatibles, utilisez `BREAKING CHANGE:` dans le footer :

```
feat(api): change authentication endpoint

BREAKING CHANGE: The /auth/login endpoint now requires
a JSON body instead of form data. All API clients must
be updated.

Refs: PROJ-456
```

Ou utilisez `!` après le type :

```
feat(api)!: change authentication endpoint
```

### Co-auteurs

```
feat(ui): redesign homepage

Co-authored-by: Jean Dupont <jean.dupont@mairistem.fr>
Co-authored-by: Marie Martin <marie.martin@mairistem.fr>
```

## Exemples complets

### Commit simple

```
feat(users): add email verification
```

### Commit avec body

```
fix(api): resolve memory leak in connection pool

The connection pool was not properly releasing connections
after timeout, causing memory to grow indefinitely.

Added explicit cleanup in the error handler.

Refs: PROJ-789
```

### Commit avec breaking change

```
feat(api)!: migrate to v2 authentication

BREAKING CHANGE: API v1 authentication endpoints are removed.
All clients must migrate to v2 endpoints:
- POST /v1/auth/login → POST /v2/auth/signin
- POST /v1/auth/logout → POST /v2/auth/signout

Migration guide: docs/migration-v2.md

Refs: PROJ-1000
```

### Commit de revert

```
revert: feat(users): add email verification

This reverts commit abc1234.

The email service is not ready for production.

Refs: PROJ-800
```

## Bonnes pratiques

### À faire

- Un commit = un changement logique
- Messages clairs et descriptifs
- Référencer les tickets JIRA
- Utiliser le body pour les changements complexes

### À éviter

```bash
# Trop vague
git commit -m "fix bug"                    # ❌
git commit -m "update code"                # ❌
git commit -m "changes"                    # ❌

# Mauvais format
git commit -m "Fixed the login bug"        # ❌ (passé)
git commit -m "Feat: Add login"            # ❌ (majuscule)
git commit -m "feat(auth) add login"       # ❌ (manque ":")

# Trop long
git commit -m "feat(auth): add complete user authentication system with OAuth2, SAML, and local login support including password reset and email verification"  # ❌
```

### Exemples corrects

```bash
# Bons exemples
git commit -m "feat(auth): add OAuth2 support"
git commit -m "fix(api): resolve null pointer in user service"
git commit -m "docs(readme): add installation instructions"
git commit -m "refactor(utils): extract date formatting logic"
git commit -m "test(auth): add integration tests for login flow"
```

## Outils

### Commitlint

Configuration `.commitlintrc.json` :

```json
{
  "extends": ["@commitlint/config-conventional"],
  "rules": {
    "type-enum": [2, "always", [
      "feat", "fix", "docs", "style", "refactor",
      "perf", "test", "build", "ci", "chore", "revert"
    ]],
    "subject-case": [2, "always", "lower-case"],
    "subject-max-length": [2, "always", 72]
  }
}
```

### Husky (Git hooks)

```bash
# Installation
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional

# Configuration
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

### Commitizen

Pour des commits interactifs :

```bash
# Installation
npm install --save-dev commitizen cz-conventional-changelog

# Utilisation
npx cz
# ou
git cz
```

## Relation avec le versioning

Les types de commits influencent le [versioning](VERSIONING.md) :

| Type | Impact version |
|------|----------------|
| `feat` | MINOR (1.x.0) |
| `fix` | PATCH (1.0.x) |
| `BREAKING CHANGE` | MAJOR (x.0.0) |
| Autres | Aucun impact direct |

---

Pour toute question, consultez le [Guide de Contribution](../CONTRIBUTING.md).
