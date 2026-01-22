# Guide de Contribution

Bienvenue ! Nous sommes ravis que vous souhaitiez contribuer aux projets Mairistem. Ce document décrit les règles et bonnes pratiques à suivre pour contribuer efficacement.

## Table des matières

- [Code de conduite](#code-de-conduite)
- [Comment contribuer](#comment-contribuer)
- [Workflow de développement](#workflow-de-développement)
- [Conventions](#conventions)
- [Pull Requests](#pull-requests)
- [Revue de code](#revue-de-code)

## Code de conduite

En participant à ce projet, vous vous engagez à respecter notre [Code de Conduite](CODE_OF_CONDUCT.md).

## Comment contribuer

### Signaler un bug

1. Vérifiez que le bug n'a pas déjà été signalé dans JIRA
2. Créez un ticket JIRA de type **Bug**
3. Fournissez un maximum de détails pour reproduire le problème

### Proposer une fonctionnalité

1. Vérifiez que la fonctionnalité n'a pas déjà été proposée
2. Créez un ticket JIRA de type **Story** ou **Feature**
3. Décrivez clairement le besoin et la solution envisagée

### Contribuer au code

1. Créez une **branche** selon nos [conventions de nommage](docs/BRANCH_NAMING.md)
2. Développez votre fonctionnalité ou correction
3. **Commit** vos changements selon nos [conventions de commits](docs/COMMIT_CONVENTIONS.md)
4. **Push** votre branche
5. Ouvrez une **Pull Request**

## Workflow de développement

### Branches principales

| Branche | Cluster | Description |
|---------|---------|-------------|
| `main` | Prod | Code en production |
| `preprod` | Preprod | Pré-production |
| `qualite` | Qualité | Recette / QA |
| `develop` | Dev | Intégration des features |

### Cycle de vie d'une contribution

```
1. Ticket JIRA créé (bug/feature)
        ↓
2. Branche créée depuis develop
        ↓
3. Développement + commits
        ↓
4. Pull Request ouverte
        ↓
5. Code Review
        ↓
6. Corrections si nécessaire
        ↓
7. Merge dans develop
        ↓
8. Déploiement : Dev → Qualité → Preprod → Prod
```

## Conventions

Nous suivons des conventions strictes pour maintenir la qualité et la cohérence du code :

| Document | Description |
|----------|-------------|
| [BRANCH_NAMING.md](docs/BRANCH_NAMING.md) | Conventions de nommage des branches |
| [VERSIONING.md](docs/VERSIONING.md) | Conventions de versioning et tags |
| [COMMIT_CONVENTIONS.md](docs/COMMIT_CONVENTIONS.md) | Conventions de messages de commit |

### Résumé rapide

#### Branches
```
feature/PROJ-123-description-courte
fix/PROJ-456-description-courte
hotfix/PROJ-789-description-courte
```

#### Commits
```
feat(scope): description courte
fix(scope): description courte
docs(scope): description courte
```

#### Tags
```
v1.0.0
v1.1.0-beta.1
v2.0.0-rc.1
```

## Pull Requests

### Avant de soumettre

- [ ] Le code compile sans erreur
- [ ] Les tests passent
- [ ] Le linter ne signale pas d'erreur
- [ ] La documentation est mise à jour si nécessaire
- [ ] Le code suit les conventions de style du projet

### Contenu de la PR

- Description des changements
- Référence au ticket JIRA
- Type de changement (feature, fix, refactor, etc.)

## Revue de code

### Pour les reviewers

- Soyez constructifs et bienveillants
- Expliquez le "pourquoi" de vos commentaires
- Distinguez les suggestions des exigences
- Utilisez les conventions de commentaires :
  - `nit:` - Détail mineur, non bloquant
  - `suggestion:` - Proposition d'amélioration
  - `question:` - Demande de clarification
  - `issue:` - Problème à corriger

### Pour les auteurs

- Répondez à tous les commentaires
- Demandez des clarifications si nécessaire
- Ne prenez pas les retours personnellement
- Mettez à jour la PR suite aux retours

## Standards de qualité

### Code

- Suivez les conventions de style du langage/framework
- Écrivez du code lisible et maintenable
- Commentez le code complexe
- Évitez la duplication (DRY - Don't Repeat Yourself)

### Tests

- Écrivez des tests pour les nouvelles fonctionnalités
- Maintenez une couverture de tests adéquate
- Les tests doivent être déterministes et reproductibles

### Documentation

- Documentez les API publiques
- Mettez à jour le README si nécessaire

## Besoin d'aide ?

Consultez le document [SUPPORT.md](SUPPORT.md) pour connaître les canaux de support disponibles.

---

Merci de contribuer à Mairistem !
