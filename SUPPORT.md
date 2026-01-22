# Support

Ce document décrit comment obtenir de l'aide pour les projets Mairistem.

## Canaux de support

### Pour les problèmes techniques

| Canal | Usage | Délai de réponse |
|-------|-------|------------------|
| **Teams** | Questions rapides, discussions, escalade | Temps réel |
| **Email** | Questions techniques, demandes d'accès | 24-48h ouvrées |
| **JIRA** | Bugs, incidents, demandes de fonctionnalités | Selon priorité |

### Contacts

| Type | Contact |
|------|---------|
| Support technique | support@mairistem.fr |
| Équipe DevOps | devops@mairistem.fr |
| Sécurité | security@mairistem.fr |

## Comment signaler un problème

### 1. Vérifier la documentation

Avant de signaler un problème, consultez :
- La documentation du projet (README, Wiki)
- Les issues existantes (ouvertes et fermées)
- Le channel Teams du projet

### 2. Contacter votre référent technique

Pour les questions techniques ou blocages :
- **Teams** : Contactez directement votre référent technique
- **Email** : Envoyez un email à votre référent technique

### 3. Créer un ticket JIRA

Pour les bugs, incidents et demandes de fonctionnalités :

1. Accédez à JIRA : [jira.mairistem.fr](https://jira.mairistem.fr)
2. Créez un ticket dans le projet approprié
3. Utilisez le bon type : `Bug`, `Story`, `Task`
4. Remplissez tous les champs obligatoires
5. Ajoutez les labels pertinents

### Priorisation des tickets

| Priorité | Description | Exemple |
|----------|-------------|---------|
| **Critique** | Service indisponible, perte de données | Production down |
| **Haute** | Fonctionnalité majeure impactée | Login impossible |
| **Moyenne** | Bug impactant mais contournable | Export PDF échoue |
| **Basse** | Amélioration, bug mineur | Typo dans l'UI |

## Microsoft Teams

### Channels disponibles

| Channel | Usage |
|---------|-------|
| `Dev - Général` | Discussions générales développement |
| `Dev - Aide` | Demandes d'aide technique |
| `DevOps` | Infrastructure, CI/CD, déploiements |
| `Incidents` | Signalement et suivi d'incidents |
| `Releases` | Annonces de releases |

### Bonnes pratiques

- Utilisez les threads pour garder les conversations organisées
- Mentionnez les personnes concernées avec `@`
- Partagez les liens JIRA pour le contexte
- Privilégiez les channels publics pour les sujets techniques

## Escalade

Si vous êtes bloqué ou si votre problème n'est pas résolu :

### Option 1 : Contacter votre référent technique

- **Par Teams** : Message direct ou appel
- **Par Email** : Email avec contexte détaillé

### Option 2 : Créer un ticket JIRA

Si le problème nécessite un suivi formel :

1. Créez un ticket JIRA avec le type approprié
2. Décrivez le contexte et le blocage
3. Assignez ou mentionnez votre référent technique
4. Ajoutez le label `escalade` si urgent

### Chaîne d'escalade

```
1. Référent technique (Teams / Email / JIRA)
        ↓
2. Tech Lead du projet
        ↓
3. Manager technique
        ↓
4. Direction technique
```

## Ressources utiles

### Documentation interne

- [Confluence](https://confluence.mairistem.fr) - Documentation technique
- [Wiki GitHub](../../wiki) - Documentation projet

### Outils

| Outil | URL | Usage |
|-------|-----|-------|
| JIRA | jira.mairistem.fr | Gestion de tickets |
| Confluence | confluence.mairistem.fr | Documentation |
| GitHub | github.com/mairistem | Code source |
| Grafana | grafana.mairistem.fr | Monitoring |
| Kibana | kibana.mairistem.fr | Logs |

## FAQ

### Je n'arrive pas à accéder au repository

1. Vérifiez que vous êtes membre de l'organisation GitHub
2. Contactez votre référent technique ou l'équipe DevOps

### Comment obtenir des accès à un environnement ?

1. Créez un ticket JIRA de type "Access Request"
2. Indiquez l'environnement et le niveau d'accès souhaité
3. Faites valider par votre manager

### Qui contacter pour une urgence en production ?

1. Channel Teams `Incidents`
2. Astreinte DevOps (voir planning dans Confluence)
3. Email : urgence@mairistem.fr

---

Pour toute question sur ce document, contactez votre référent technique.
