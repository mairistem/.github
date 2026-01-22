# Principes d'Architecture

Ce document décrit les principes d'architecture et les patterns recommandés pour les projets Mairistem.

## Table des matières

- [Vision architecturale](#vision-architecturale)
- [Principes fondamentaux](#principes-fondamentaux)
- [Architecture microservices](#architecture-microservices)
- [Patterns recommandés](#patterns-recommandés)
- [Architecture Decision Records (ADR)](#architecture-decision-records-adr)
- [Stack technique](#stack-technique)

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

Toute infrastructure doit être codifiée :

- Terraform pour le provisioning
- Helm pour Kubernetes
- Configuration versionnée

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
│              (Kong / Traefik / Nginx)                    │
└───────┬──────────────────┬───────────────────┬───────────┘
        │                  │                   │
        ▼                  ▼                   ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│    Service    │  │    Service    │  │    Service    │
│     Users     │  │    Billing    │  │   Documents   │
└───────┬───────┘  └───────┬───────┘  └───────┬───────┘
        │                  │                   │
        ▼                  ▼                   ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│   Database    │  │   Database    │  │  Object Store │
│  (PostgreSQL) │  │  (PostgreSQL) │  │     (S3)      │
└───────────────┘  └───────────────┘  └───────────────┘
```

### Communication inter-services

| Type | Usage | Technologie |
|------|-------|-------------|
| Synchrone | Requêtes temps réel | REST API, gRPC |
| Asynchrone | Events, tâches longues | RabbitMQ, Redis Pub/Sub |

### Principes microservices

1. **Single Responsibility** : Un service = un domaine métier
2. **Autonomie** : Base de données par service
3. **Découplage** : Communication via API/Events
4. **Déployabilité** : Déploiement indépendant

## Patterns recommandés

### Backend

#### Repository Pattern

Abstraction de la couche de persistance.

```typescript
// Interface
interface UserRepository {
  findById(id: string): Promise<User>;
  save(user: User): Promise<void>;
}

// Implementation
class PostgresUserRepository implements UserRepository {
  async findById(id: string): Promise<User> {
    // ...
  }
}
```

#### Service Layer

Logique métier isolée des contrôleurs.

```typescript
class UserService {
  constructor(private userRepository: UserRepository) {}

  async createUser(dto: CreateUserDto): Promise<User> {
    // Validation métier
    // Logique métier
    // Persistance
  }
}
```

#### DTO (Data Transfer Object)

Séparer les modèles API des modèles internes.

```typescript
// Request DTO
class CreateUserDto {
  email: string;
  name: string;
}

// Response DTO
class UserResponseDto {
  id: string;
  email: string;
  createdAt: Date;
}
```

### Frontend

#### Component Composition

Composants petits et réutilisables.

```tsx
// Composition plutôt qu'héritage
<Card>
  <CardHeader>
    <CardTitle>Titre</CardTitle>
  </CardHeader>
  <CardContent>
    Contenu
  </CardContent>
</Card>
```

#### Container/Presenter Pattern

Séparer logique et présentation.

```tsx
// Container (logique)
function UserListContainer() {
  const { data, isLoading } = useUsers();
  return <UserList users={data} loading={isLoading} />;
}

// Presenter (UI pure)
function UserList({ users, loading }) {
  if (loading) return <Spinner />;
  return <ul>{users.map(u => <li>{u.name}</li>)}</ul>;
}
```

#### Custom Hooks

Extraire la logique réutilisable.

```tsx
function useUsers() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchUsers().then(setUsers).finally(() => setLoading(false));
  }, []);

  return { users, loading };
}
```

### API

#### REST Conventions

| Méthode | Route | Action |
|---------|-------|--------|
| GET | `/users` | Liste des utilisateurs |
| GET | `/users/:id` | Détail d'un utilisateur |
| POST | `/users` | Créer un utilisateur |
| PUT | `/users/:id` | Remplacer un utilisateur |
| PATCH | `/users/:id` | Modifier partiellement |
| DELETE | `/users/:id` | Supprimer un utilisateur |

#### Pagination

```json
{
  "data": [...],
  "meta": {
    "page": 1,
    "perPage": 20,
    "total": 150,
    "totalPages": 8
  }
}
```

#### Error Handling

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      { "field": "email", "message": "Invalid email format" }
    ]
  }
}
```

## Architecture Decision Records (ADR)

### Format ADR

Documenter les décisions architecturales importantes.

```markdown
# ADR-001: Choix de PostgreSQL pour la base de données

## Statut
Accepté

## Contexte
Nous devons choisir une base de données pour le service Users.

## Décision
Nous utiliserons PostgreSQL.

## Raisons
- Support ACID
- Performances sur les requêtes complexes
- Expertise de l'équipe
- Écosystème mature

## Conséquences
- Formation nécessaire pour certains développeurs
- Besoin de gérer les migrations
```

### ADR existants

| ID | Titre | Statut |
|----|-------|--------|
| ADR-001 | Choix de PostgreSQL | Accepté |
| ADR-002 | Architecture microservices | Accepté |
| ADR-003 | Kubernetes pour l'orchestration | Accepté |

## Stack technique

### Backend

| Technologie | Usage |
|-------------|-------|
| **Laravel** | API REST, applications monolithiques |
| **Nest.js** | Microservices, API GraphQL |
| **PostgreSQL** | Base de données relationnelle |
| **Redis** | Cache, sessions, pub/sub |
| **RabbitMQ** | Message queue |

### Frontend

| Technologie | Usage |
|-------------|-------|
| **Next.js** | Applications web, SSR/SSG |
| **React** | Composants UI |
| **Tailwind CSS** | Styling |
| **React Query** | Data fetching, cache |

### Infrastructure

| Technologie | Usage |
|-------------|-------|
| **Kubernetes** | Orchestration containers |
| **Docker** | Containerisation |
| **Helm** | Packaging Kubernetes |
| **Terraform** | Infrastructure as Code |
| **GitHub Actions** | CI/CD |

### Observabilité

| Technologie | Usage |
|-------------|-------|
| **Prometheus** | Métriques |
| **Grafana** | Dashboards |
| **ELK Stack** | Logs centralisés |
| **Jaeger** | Tracing distribué |

## Anti-patterns à éviter

| Anti-pattern | Problème | Alternative |
|--------------|----------|-------------|
| **God class** | Classe avec trop de responsabilités | Décomposer en classes spécialisées |
| **Spaghetti code** | Code sans structure claire | Patterns et architecture en couches |
| **N+1 queries** | Requêtes DB excessives | Eager loading, batch queries |
| **Shared database** | Services couplés par la DB | Database per service |
| **Synchronous chains** | Appels synchrones en cascade | Events asynchrones |

---

Pour toute question d'architecture, contactez les Tech Leads ou ouvrez une discussion dans le channel `#architecture`.
