# Stratégie de Tests

Ce document définit la stratégie de tests pour les projets Mairistem.

## Table des matières

- [Philosophie](#philosophie)
- [Pyramide des tests](#pyramide-des-tests)
- [Types de tests](#types-de-tests)
- [Tests Backend](#tests-backend)
- [Tests Frontend](#tests-frontend)
- [Tests E2E](#tests-e2e)
- [Couverture de code](#couverture-de-code)
- [CI/CD](#cicd)
- [Bonnes pratiques](#bonnes-pratiques)

## Philosophie

### Principes

1. **Les tests sont du code** : Même rigueur que le code de production
2. **Test-Driven Development (TDD)** : Encouragé pour les fonctionnalités complexes
3. **Tests déterministes** : Un test doit toujours produire le même résultat
4. **Tests indépendants** : Aucune dépendance entre les tests
5. **Tests rapides** : Un test unitaire < 100ms

### Quand tester ?

| Situation | Tests requis |
|-----------|--------------|
| Nouvelle fonctionnalité | Tests unitaires + intégration |
| Correction de bug | Test reproduisant le bug avant fix |
| Refactoring | Tests existants doivent passer |
| API publique | Tests de contrat |
| Logique métier critique | Couverture maximale |

## Pyramide des tests

```
                    ╱╲
                   ╱  ╲
                  ╱ E2E╲           5-10%
                 ╱______╲
                ╱        ╲
               ╱Integration╲       20-30%
              ╱____________╲
             ╱              ╲
            ╱   Unit Tests   ╲     60-70%
           ╱__________________╲
```

| Type | Quantité | Vitesse | Coût |
|------|----------|---------|------|
| E2E | Peu | Lent | Élevé |
| Intégration | Modéré | Moyen | Moyen |
| Unitaire | Beaucoup | Rapide | Faible |

## Types de tests

### Tests unitaires

- Testent une unité de code isolée (fonction, classe)
- Mockent toutes les dépendances
- Très rapides (< 100ms)

```typescript
// user.service.spec.ts
describe('UserService', () => {
  let service: UserService;
  let mockRepository: jest.Mocked<UserRepository>;

  beforeEach(() => {
    mockRepository = {
      findById: jest.fn(),
      save: jest.fn(),
    };
    service = new UserService(mockRepository);
  });

  describe('createUser', () => {
    it('should create a user with hashed password', async () => {
      const dto = { email: 'test@example.com', password: 'secret' };
      mockRepository.save.mockResolvedValue({ id: '1', ...dto });

      const result = await service.createUser(dto);

      expect(result.id).toBe('1');
      expect(mockRepository.save).toHaveBeenCalledWith(
        expect.objectContaining({ email: dto.email })
      );
    });

    it('should throw if email already exists', async () => {
      mockRepository.findByEmail.mockResolvedValue({ id: '1' });

      await expect(service.createUser({ email: 'existing@test.com' }))
        .rejects.toThrow('Email already exists');
    });
  });
});
```

### Tests d'intégration

- Testent l'interaction entre plusieurs composants
- Utilisent une vraie base de données (souvent en mémoire)
- Plus lents que les tests unitaires

```typescript
// user.integration.spec.ts
describe('User API (Integration)', () => {
  let app: INestApplication;
  let prisma: PrismaService;

  beforeAll(async () => {
    const module = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = module.createNestApplication();
    prisma = module.get(PrismaService);
    await app.init();
  });

  beforeEach(async () => {
    await prisma.user.deleteMany();
  });

  afterAll(async () => {
    await app.close();
  });

  describe('POST /users', () => {
    it('should create a user', async () => {
      const response = await request(app.getHttpServer())
        .post('/users')
        .send({ email: 'test@example.com', name: 'Test User' })
        .expect(201);

      expect(response.body).toMatchObject({
        email: 'test@example.com',
        name: 'Test User',
      });

      const userInDb = await prisma.user.findUnique({
        where: { email: 'test@example.com' },
      });
      expect(userInDb).not.toBeNull();
    });
  });
});
```

### Tests End-to-End (E2E)

- Testent le système complet du point de vue utilisateur
- Utilisent un navigateur réel ou headless
- Les plus lents et coûteux

```typescript
// login.e2e.spec.ts (Cypress)
describe('Login Flow', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('should login with valid credentials', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('password123');
    cy.get('[data-testid="login-button"]').click();

    cy.url().should('include', '/dashboard');
    cy.get('[data-testid="welcome-message"]').should('contain', 'Bienvenue');
  });

  it('should show error with invalid credentials', () => {
    cy.get('[data-testid="email-input"]').type('wrong@example.com');
    cy.get('[data-testid="password-input"]').type('wrongpassword');
    cy.get('[data-testid="login-button"]').click();

    cy.get('[data-testid="error-message"]')
      .should('be.visible')
      .and('contain', 'Identifiants invalides');
  });
});
```

## Tests Backend

### NestJS

```bash
# Structure des fichiers de test
src/
├── users/
│   ├── users.controller.ts
│   ├── users.controller.spec.ts    # Tests unitaires controller
│   ├── users.service.ts
│   ├── users.service.spec.ts       # Tests unitaires service
│   └── users.e2e-spec.ts           # Tests E2E
```

### Laravel

```bash
# Structure des fichiers de test
tests/
├── Unit/
│   ├── Services/
│   │   └── UserServiceTest.php
│   └── Models/
│       └── UserTest.php
├── Feature/
│   ├── Api/
│   │   └── UserApiTest.php
│   └── Http/
│       └── UserControllerTest.php
└── Browser/                        # Tests Dusk (E2E)
    └── LoginTest.php
```

```php
// tests/Feature/Api/UserApiTest.php
class UserApiTest extends TestCase
{
    use RefreshDatabase;

    public function test_can_create_user(): void
    {
        $response = $this->postJson('/api/users', [
            'email' => 'test@example.com',
            'name' => 'Test User',
            'password' => 'password123',
        ]);

        $response
            ->assertStatus(201)
            ->assertJsonStructure(['id', 'email', 'name', 'created_at']);

        $this->assertDatabaseHas('users', [
            'email' => 'test@example.com',
        ]);
    }

    public function test_validates_email_format(): void
    {
        $response = $this->postJson('/api/users', [
            'email' => 'invalid-email',
            'name' => 'Test',
        ]);

        $response
            ->assertStatus(422)
            ->assertJsonValidationErrors(['email']);
    }
}
```

## Tests Frontend

### React / Next.js

```bash
# Structure recommandée
src/
├── components/
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx         # Tests unitaires
│   │   └── Button.stories.tsx      # Storybook
│   └── UserCard/
│       ├── UserCard.tsx
│       └── UserCard.test.tsx
├── hooks/
│   ├── useUser.ts
│   └── useUser.test.ts
└── __tests__/                      # Tests d'intégration
    └── pages/
        └── dashboard.test.tsx
```

### Testing Library

```tsx
// Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);

    fireEvent.click(screen.getByRole('button'));

    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('is disabled when loading', () => {
    render(<Button loading>Submit</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
  });
});
```

### Tests de hooks

```tsx
// useUser.test.ts
import { renderHook, waitFor } from '@testing-library/react';
import { useUser } from './useUser';

// Mock du fetch
global.fetch = jest.fn();

describe('useUser', () => {
  it('fetches user data', async () => {
    const mockUser = { id: '1', name: 'John' };
    (fetch as jest.Mock).mockResolvedValueOnce({
      ok: true,
      json: async () => mockUser,
    });

    const { result } = renderHook(() => useUser('1'));

    expect(result.current.loading).toBe(true);

    await waitFor(() => {
      expect(result.current.loading).toBe(false);
    });

    expect(result.current.user).toEqual(mockUser);
    expect(result.current.error).toBeNull();
  });
});
```

## Tests E2E

### Cypress

```typescript
// cypress/e2e/user-management.cy.ts
describe('User Management', () => {
  beforeEach(() => {
    cy.login('admin@example.com', 'admin123'); // Custom command
  });

  it('should create a new user', () => {
    cy.visit('/admin/users');
    cy.get('[data-testid="create-user-btn"]').click();

    cy.get('[data-testid="user-form"]').within(() => {
      cy.get('input[name="email"]').type('newuser@example.com');
      cy.get('input[name="name"]').type('New User');
      cy.get('select[name="role"]').select('user');
      cy.get('button[type="submit"]').click();
    });

    cy.get('[data-testid="success-toast"]')
      .should('be.visible')
      .and('contain', 'Utilisateur créé');

    cy.get('[data-testid="users-table"]')
      .should('contain', 'newuser@example.com');
  });
});
```

### Playwright (alternative)

```typescript
// tests/user-management.spec.ts
import { test, expect } from '@playwright/test';

test.describe('User Management', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
    await page.fill('[data-testid="email"]', 'admin@example.com');
    await page.fill('[data-testid="password"]', 'admin123');
    await page.click('[data-testid="login-btn"]');
    await expect(page).toHaveURL('/dashboard');
  });

  test('should display users list', async ({ page }) => {
    await page.goto('/admin/users');
    await expect(page.locator('[data-testid="users-table"]')).toBeVisible();
    await expect(page.locator('tbody tr')).toHaveCount(10);
  });
});
```

## Couverture de code

### Objectifs

| Type de code | Couverture minimale |
|--------------|---------------------|
| Logique métier | 80% |
| Controllers/Handlers | 70% |
| Utilitaires | 90% |
| UI Components | 60% |
| Global | 70% |

### Configuration Jest

```javascript
// jest.config.js
module.exports = {
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.tsx',
  ],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 70,
      statements: 70,
    },
  },
};
```

### Rapport de couverture

```bash
# Générer le rapport
npm run test:cov

# Ouvrir le rapport HTML
open coverage/lcov-report/index.html
```

## CI/CD

### Pipeline de tests

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run test:unit
      - run: npm run test:cov
      - uses: codecov/codecov-action@v3

  integration-tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run test:integration

  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npm run test:e2e
```

## Bonnes pratiques

### Naming conventions

```typescript
// describe : Nom de l'unité testée
describe('UserService', () => {
  // describe imbriqué : Méthode ou fonctionnalité
  describe('createUser', () => {
    // it : Comportement attendu
    it('should create a user with valid data', () => {});
    it('should throw if email is invalid', () => {});
    it('should hash the password before saving', () => {});
  });
});
```

### Arrange-Act-Assert (AAA)

```typescript
it('should calculate total with discount', () => {
  // Arrange
  const items = [
    { price: 100, quantity: 2 },
    { price: 50, quantity: 1 },
  ];
  const discount = 0.1; // 10%

  // Act
  const total = calculateTotal(items, discount);

  // Assert
  expect(total).toBe(225); // (200 + 50) * 0.9
});
```

### Éviter les anti-patterns

```typescript
// ❌ Test qui dépend d'un autre test
let userId: string;

it('should create user', async () => {
  userId = await createUser();
  expect(userId).toBeDefined();
});

it('should get user', async () => {
  const user = await getUser(userId); // Dépend du test précédent
  expect(user).toBeDefined();
});

// ✅ Tests indépendants
it('should create user', async () => {
  const userId = await createUser();
  expect(userId).toBeDefined();
});

it('should get user', async () => {
  const userId = await createUser(); // Crée ses propres données
  const user = await getUser(userId);
  expect(user).toBeDefined();
});
```

### Data-testid

```tsx
// Utiliser data-testid pour les sélecteurs stables
<button data-testid="submit-button">Submit</button>

// Dans les tests
cy.get('[data-testid="submit-button"]').click();
screen.getByTestId('submit-button');
```

---

Pour toute question sur les tests, contactez l'équipe QA ou consultez le channel `#testing`.
