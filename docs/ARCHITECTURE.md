# Principes d'Architecture

Ce document décrit les principes d'architecture pour les projets Mairistem.

## Table des matières

- [Vision architecturale](#vision-architecturale)
- [Principes fondamentaux](#principes-fondamentaux)
- [Architecture microservices](#architecture-microservices)
- [Architecture Decision Records (ADR)](#architecture-decision-records-adr)

## Vision architecturale

Notre architecture vise à :

- **Scalabilité** : Supporter la croissance des utilisateurs et des données
- **Résilience** : Tolérer les pannes et maintenir la disponibilité
- **Maintenabilité** : Faciliter les évolutions et corrections
- **Sécurité** : Protéger les données sensibles des collectivités

## Principes fondamentaux

### 1. Séparation des responsabilités (SoC)

Chaque composant doit avoir une responsabilité unique et bien définie.

```
┌─────────────────────────────────────────────────────────┐
│                      Frontend                           │
│  (Présentation, UX, Validation côté client)            │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    API Gateway                          │
│  (Routage, Auth, Rate limiting, Logging)               │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    Microservices                        │
│  (Logique métier, Validation, Persistance)             │
└─────────────────────────────────────────────────────────┘
```

### 2. Design for failure

Concevoir les systèmes en anticipant les pannes :

- Circuit breakers
- Retry avec exponential backoff
- Graceful degradation
- Health checks

### 3. API First

Concevoir l'API avant l'implémentation :

- Spécification OpenAPI/Swagger
- Contrats d'API stables
- Versioning explicite

### 4. Infrastructure as Code (IaC)

Toute infrastructure doit être codifiée et versionnée.

## Architecture microservices

### Structure type

```
                    ┌─────────────┐
                    │   Client    │
                    │  (Browser)  │
                    └──────┬──────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│                     Load Balancer                        │
└──────────────────────────┬───────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────┐
│                      API Gateway                         │
└───────┬──────────────────┬───────────────────┬───────────┘
        │                  │                   │
        ▼                  ▼                   ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│    Service    │  │    Service    │  │    Service    │
│       A       │  │       B       │  │       C       │
└───────┬───────┘  └───────┬───────┘  └───────┬───────┘
        │                  │                   │
        ▼                  ▼                   ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│   Database    │  │   Database    │  │    Storage    │
└───────────────┘  └───────────────┘  └───────────────┘
```

### Communication inter-services

| Type | Usage |
|------|-------|
| Synchrone | Requêtes temps réel (REST API, gRPC) |
| Asynchrone | Events, tâches longues (Message queue) |

### Principes microservices

1. **Single Responsibility** : Un service = un domaine métier
2. **Autonomie** : Base de données par service
3. **Découplage** : Communication via API/Events
4. **Déployabilité** : Déploiement indépendant

## Architecture Decision Records (ADR)

Nous documentons les décisions architecturales importantes via des ADR.

### Format ADR

```markdown
# ADR-XXX: Titre de la décision

## Statut
Proposé | Accepté | Déprécié | Remplacé

## Contexte
Quel est le problème ou la situation qui nécessite une décision ?

## Décision
Quelle est la décision prise ?

## Raisons
Pourquoi cette décision a-t-elle été prise ?

## Conséquences
Quels sont les impacts de cette décision ?
```

### Où trouver les ADR

Les ADR sont stockés dans le dossier `docs/adr/` de chaque projet ou dans Confluence.

---

Pour toute question d'architecture, contactez votre référent technique ou le Tech Lead.
