# Guide de Style de Code

Ce document définit les conventions de style de code pour les projets Mairistem.

## Table des matières

- [Principes généraux](#principes-généraux)
- [TypeScript / JavaScript](#typescript--javascript)
- [PHP / Laravel](#php--laravel)
- [React / Next.js](#react--nextjs)
- [CSS / Tailwind](#css--tailwind)
- [SQL](#sql)
- [Git](#git)
- [Outils de formatage](#outils-de-formatage)

## Principes généraux

### Lisibilité

- Le code est lu plus souvent qu'il n'est écrit
- Privilégier la clarté à la concision
- Un fichier = une responsabilité
- Nommer explicitement (éviter les abréviations cryptiques)

### Cohérence

- Suivre les conventions du projet existant
- Utiliser les outils de formatage automatique
- Respecter les patterns établis

### Documentation

- Commenter le "pourquoi", pas le "quoi"
- Documenter les API publiques
- Maintenir le README à jour

## TypeScript / JavaScript

### Configuration

```json
// tsconfig.json (extrait)
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

### Nommage

| Type | Convention | Exemple |
|------|------------|---------|
| Variables | camelCase | `userName`, `isActive` |
| Constantes | SCREAMING_SNAKE_CASE | `MAX_RETRIES`, `API_URL` |
| Fonctions | camelCase | `getUserById()`, `formatDate()` |
| Classes | PascalCase | `UserService`, `HttpClient` |
| Interfaces | PascalCase (sans préfixe I) | `User`, `ApiResponse` |
| Types | PascalCase | `UserId`, `RequestConfig` |
| Enums | PascalCase | `UserRole`, `HttpStatus` |
| Fichiers | kebab-case | `user-service.ts`, `api-client.ts` |

### Types

```typescript
// Préférer les interfaces pour les objets
interface User {
  id: string;
  email: string;
  createdAt: Date;
}

// Types pour les unions et utilitaires
type UserId = string;
type UserRole = 'admin' | 'user' | 'guest';

// Éviter any, utiliser unknown si nécessaire
function parseJson(json: string): unknown {
  return JSON.parse(json);
}

// Typer explicitement les retours de fonction
function getUser(id: string): Promise<User | null> {
  // ...
}
```

### Fonctions

```typescript
// Fonctions courtes et focalisées
function calculateTotal(items: Item[]): number {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// Paramètres optionnels avec valeurs par défaut
function fetchUsers(page = 1, limit = 20): Promise<User[]> {
  // ...
}

// Destructuring pour les objets complexes
function createUser({ email, name, role = 'user' }: CreateUserDto): User {
  // ...
}

// Arrow functions pour les callbacks
const activeUsers = users.filter(user => user.isActive);
```

### Async/Await

```typescript
// Préférer async/await aux .then()
async function fetchUserWithPosts(userId: string): Promise<UserWithPosts> {
  const user = await userService.findById(userId);
  const posts = await postService.findByUserId(userId);
  return { ...user, posts };
}

// Gestion des erreurs explicite
async function safelyFetchUser(id: string): Promise<User | null> {
  try {
    return await userService.findById(id);
  } catch (error) {
    logger.error('Failed to fetch user', { id, error });
    return null;
  }
}
```

### Imports

```typescript
// Ordre des imports
// 1. Modules Node.js
import { readFile } from 'fs/promises';

// 2. Dépendances externes
import { Injectable } from '@nestjs/common';
import { z } from 'zod';

// 3. Imports internes absolus
import { UserService } from '@/services/user.service';
import { User } from '@/models/user';

// 4. Imports relatifs
import { formatDate } from './utils';
import type { Config } from './types';
```

## PHP / Laravel

### PSR Standards

Nous suivons les standards PSR :
- PSR-1 : Basic Coding Standard
- PSR-4 : Autoloading
- PSR-12 : Extended Coding Style

### Nommage

| Type | Convention | Exemple |
|------|------------|---------|
| Variables | camelCase | `$userName`, `$isActive` |
| Constantes | SCREAMING_SNAKE_CASE | `MAX_RETRIES` |
| Fonctions | camelCase | `getUserById()` |
| Classes | PascalCase | `UserService` |
| Méthodes | camelCase | `findByEmail()` |
| Propriétés | camelCase | `$createdAt` |
| Tables DB | snake_case pluriel | `users`, `user_roles` |
| Colonnes DB | snake_case | `created_at`, `user_id` |

### Laravel Conventions

```php
// Controllers : singulier + Controller
class UserController extends Controller
{
    // Actions RESTful
    public function index() { }    // GET /users
    public function show($id) { }  // GET /users/{id}
    public function store() { }    // POST /users
    public function update($id) { } // PUT /users/{id}
    public function destroy($id) { } // DELETE /users/{id}
}

// Models : singulier, PascalCase
class User extends Model
{
    // Relations
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }
}

// Requests : verbe + ressource + Request
class StoreUserRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'email' => ['required', 'email', 'unique:users'],
            'name' => ['required', 'string', 'max:255'],
        ];
    }
}
```

### Services

```php
// Services pour la logique métier
class UserService
{
    public function __construct(
        private UserRepository $repository,
        private EventDispatcher $events,
    ) {}

    public function createUser(array $data): User
    {
        $user = $this->repository->create($data);
        $this->events->dispatch(new UserCreated($user));
        return $user;
    }
}
```

## React / Next.js

### Structure des composants

```tsx
// Ordre dans un composant
// 1. Imports
import { useState, useEffect } from 'react';
import { Button } from '@/components/ui/button';

// 2. Types
interface UserCardProps {
  user: User;
  onEdit?: (user: User) => void;
}

// 3. Composant
export function UserCard({ user, onEdit }: UserCardProps) {
  // 3a. Hooks
  const [isExpanded, setIsExpanded] = useState(false);

  // 3b. Handlers
  const handleToggle = () => setIsExpanded(!isExpanded);

  // 3c. Render
  return (
    <div className="rounded-lg border p-4">
      <h3>{user.name}</h3>
      {isExpanded && <p>{user.bio}</p>}
      <Button onClick={handleToggle}>
        {isExpanded ? 'Réduire' : 'Voir plus'}
      </Button>
    </div>
  );
}
```

### Nommage des composants

| Type | Convention | Exemple |
|------|------------|---------|
| Composants | PascalCase | `UserCard`, `NavBar` |
| Hooks | camelCase avec use | `useUser`, `useAuth` |
| Fichiers composants | PascalCase ou kebab-case | `UserCard.tsx` ou `user-card.tsx` |
| Event handlers | handle + Event | `handleClick`, `handleSubmit` |
| Boolean props | is/has/should | `isLoading`, `hasError` |

### Hooks personnalisés

```tsx
// Préfixer par "use"
function useUser(id: string) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetchUser(id)
      .then(setUser)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [id]);

  return { user, loading, error };
}
```

### Bonnes pratiques

```tsx
// Éviter les inline functions dans le JSX pour les listes
// ❌ Mauvais
{users.map(user => (
  <UserCard key={user.id} onClick={() => handleSelect(user)} />
))}

// ✅ Bon
const handleUserSelect = useCallback((user: User) => {
  // ...
}, []);

{users.map(user => (
  <UserCard key={user.id} user={user} onSelect={handleUserSelect} />
))}
```

## CSS / Tailwind

### Classes Tailwind

```tsx
// Ordre des classes (recommandé)
// 1. Layout (display, position, flex/grid)
// 2. Sizing (width, height)
// 3. Spacing (margin, padding)
// 4. Typography (font, text)
// 5. Visual (background, border, shadow)
// 6. States (hover, focus)

<div className="flex items-center justify-between w-full p-4 text-sm font-medium bg-white border rounded-lg hover:bg-gray-50">
  Content
</div>
```

### Composants réutilisables

```tsx
// Utiliser cva ou class-variance-authority pour les variants
import { cva, type VariantProps } from 'class-variance-authority';

const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md font-medium',
  {
    variants: {
      variant: {
        primary: 'bg-blue-600 text-white hover:bg-blue-700',
        secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
        destructive: 'bg-red-600 text-white hover:bg-red-700',
      },
      size: {
        sm: 'h-8 px-3 text-sm',
        md: 'h-10 px-4 text-base',
        lg: 'h-12 px-6 text-lg',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
);
```

## SQL

### Conventions

```sql
-- Mots-clés en MAJUSCULES
SELECT id, email, created_at
FROM users
WHERE is_active = true
ORDER BY created_at DESC;

-- Tables en snake_case pluriel
CREATE TABLE user_roles (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    role_name VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Index nommés explicitement
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_user_roles_user_id ON user_roles(user_id);
```

## Git

### Messages de commit

Voir [COMMIT_CONVENTIONS.md](COMMIT_CONVENTIONS.md).

### Branches

Voir [BRANCH_NAMING.md](BRANCH_NAMING.md).

## Outils de formatage

### Configuration ESLint

```json
// .eslintrc.json
{
  "extends": [
    "next/core-web-vitals",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/explicit-function-return-type": "warn"
  }
}
```

### Configuration Prettier

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100
}
```

### Configuration PHP CS Fixer

```php
// .php-cs-fixer.php
return (new PhpCsFixer\Config())
    ->setRules([
        '@PSR12' => true,
        'array_syntax' => ['syntax' => 'short'],
        'ordered_imports' => ['sort_algorithm' => 'alpha'],
    ]);
```

### Scripts npm recommandés

```json
// package.json
{
  "scripts": {
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "typecheck": "tsc --noEmit"
  }
}
```

---

Pour toute question sur le style de code, consultez le channel `#dev-standards` ou ouvrez une discussion.
