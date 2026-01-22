# Support

Ce document décrit comment obtenir de l'aide pour les projets Mairistem.

## Canaux de support

### Pour les problèmes techniques

| Canal | Usage | Délai de réponse |
|-------|-------|------------------|
| **JIRA** | Bugs, incidents, demandes de fonctionnalités | Selon priorité |
| **Email** | Questions techniques, demandes d'accès | 24-48h ouvrées |
| **Slack/Teams** | Questions rapides, discussions | Temps réel |

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
- Le channel Slack/Teams du projet

### 2. Créer un ticket JIRA

Pour les bugs et demandes de fonctionnalités :

1. Accédez à JIRA : [jira.mairistem.fr](https://jira.mairistem.fr)
2. Créez un ticket dans le projet approprié
3. Utilisez le bon type : `Bug`, `Story`, `Task`
4. Remplissez tous les champs obligatoires
5. Ajoutez les labels pertinents

### 3. Priorisation

| Priorité | Description | Exemple |
|----------|-------------|---------|
| **Critique** | Service indisponible, perte de données | Production down |
| **Haute** | Fonctionnalité majeure impactée | Login impossible |
| **Moyenne** | Bug impactant mais contournable | Export PDF échoue |
| **Basse** | Amélioration, bug mineur | Typo dans l'UI |

## Slack/Teams

### Channels disponibles

| Channel | Usage |
|---------|-------|
| `#dev-general` | Discussions générales développement |
| `#dev-help` | Demandes d'aide technique |
| `#devops` | Infrastructure, CI/CD, déploiements |
| `#incidents` | Signalement et suivi d'incidents |
| `#releases` | Annonces de releases |

### Bonnes pratiques

- Utilisez les threads pour garder les conversations organisées
- Mentionnez les personnes concernées avec `@`
- Partagez les liens JIRA pour le contexte
- Évitez les messages privés pour les sujets techniques (préférez les channels)

## Escalade

Si votre problème n'est pas résolu dans les délais attendus :

```
1. Relancer sur le ticket JIRA
        ↓
2. Contacter le Tech Lead du projet
        ↓
3. Escalader au manager technique
        ↓
4. Contacter la direction technique
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
| GitLab/GitHub | github.com/mairistem | Code source |
| Grafana | grafana.mairistem.fr | Monitoring |
| Kibana | kibana.mairistem.fr | Logs |

## FAQ

### Je n'arrive pas à accéder au repository

1. Vérifiez que vous êtes membre de l'organisation GitHub
2. Contactez votre manager ou l'équipe DevOps pour les accès

### Comment obtenir des accès à un environnement ?

1. Créez un ticket JIRA de type "Access Request"
2. Indiquez l'environnement et le niveau d'accès souhaité
3. Faites valider par votre manager

### Qui contacter pour une urgence en production ?

1. Canal Slack `#incidents`
2. Astreinte DevOps (voir planning dans Confluence)
3. Email : urgence@mairistem.fr

---

Pour toute question sur ce document, contactez l'équipe technique.
