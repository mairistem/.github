# Guide d'Intégration (Onboarding)

Bienvenue chez Mairistem ! Ce guide vous aidera à configurer votre environnement de développement et à comprendre nos processus.

## Table des matières

- [Jour 1 : Accès et configuration](#jour-1--accès-et-configuration)
- [Semaine 1 : Environnement de développement](#semaine-1--environnement-de-développement)
- [Semaine 2 : Découverte du code](#semaine-2--découverte-du-code)
- [Ressources essentielles](#ressources-essentielles)
- [Contacts clés](#contacts-clés)
- [FAQ Onboarding](#faq-onboarding)

## Jour 1 : Accès et configuration

### Checklist des accès

| Outil | Comment l'obtenir | Contact |
|-------|-------------------|---------|
| Email @mairistem.fr | RH / IT | it@mairistem.fr |
| Slack/Teams | Invitation automatique | - |
| GitHub (organisation) | Manager → DevOps | devops@mairistem.fr |
| JIRA | Manager → IT | it@mairistem.fr |
| Confluence | Manager → IT | it@mairistem.fr |
| VPN | IT | it@mairistem.fr |
| Environnements (dev, staging) | DevOps | devops@mairistem.fr |

### Premiers pas

1. **Configurer votre email** et calendrier
2. **Rejoindre Slack/Teams** et les channels de votre équipe
3. **Activer la 2FA** sur tous les comptes (obligatoire)
4. **Configurer le VPN** si nécessaire

## Semaine 1 : Environnement de développement

### Prérequis système

| Outil | Version | Installation |
|-------|---------|--------------|
| Git | 2.40+ | `brew install git` / `apt install git` |
| Node.js | 20 LTS | Via [nvm](https://github.com/nvm-sh/nvm) |
| PHP | 8.2+ | Via [phpenv](https://github.com/phpenv/phpenv) |
| Docker | Latest | [Docker Desktop](https://www.docker.com/products/docker-desktop/) |
| VS Code | Latest | [code.visualstudio.com](https://code.visualstudio.com/) |

### Installation Node.js (via nvm)

```bash
# Installer nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Installer Node.js
nvm install 20
nvm use 20
nvm alias default 20

# Vérifier
node --version  # v20.x.x
npm --version   # 10.x.x
```

### Installation PHP (via phpenv)

```bash
# macOS avec Homebrew
brew install php@8.2
brew install composer

# Ubuntu/Debian
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-curl php8.2-mbstring php8.2-xml php8.2-zip
sudo apt install composer

# Vérifier
php --version    # PHP 8.2.x
composer --version
```

### Configuration Git

```bash
# Identité
git config --global user.name "Prénom Nom"
git config --global user.email "prenom.nom@mairistem.fr"

# Configuration recommandée
git config --global init.defaultBranch main
git config --global pull.rebase true
git config --global fetch.prune true
git config --global core.autocrlf input  # macOS/Linux
git config --global core.autocrlf true   # Windows

# Authentification GitHub (SSH recommandé)
ssh-keygen -t ed25519 -C "prenom.nom@mairistem.fr"
cat ~/.ssh/id_ed25519.pub  # Ajouter dans GitHub → Settings → SSH Keys
```

### Configuration VS Code

Extensions recommandées :

```json
// .vscode/extensions.json
{
  "recommendations": [
    // Général
    "editorconfig.editorconfig",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "streetsidesoftware.code-spell-checker",
    "streetsidesoftware.code-spell-checker-french",

    // TypeScript / JavaScript
    "ms-vscode.vscode-typescript-next",

    // PHP / Laravel
    "bmewburn.vscode-intelephense-client",
    "shufo.vscode-blade-formatter",

    // Docker / DevOps
    "ms-azuretools.vscode-docker",
    "ms-kubernetes-tools.vscode-kubernetes-tools",

    // Git
    "eamodio.gitlens",
    "mhutchie.git-graph"
  ]
}
```

Settings recommandés :

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "[php]": {
    "editor.defaultFormatter": "bmewburn.vscode-intelephense-client"
  }
}
```

### Cloner un projet

```bash
# Structure recommandée
mkdir -p ~/dev/mairistem
cd ~/dev/mairistem

# Cloner un repository
git clone git@github.com:mairistem/nom-du-projet.git
cd nom-du-projet

# Installer les dépendances
npm install        # ou yarn / pnpm
composer install   # pour PHP

# Copier la configuration
cp .env.example .env

# Lancer l'environnement Docker (si applicable)
docker-compose up -d

# Lancer le serveur de développement
npm run dev
```

## Semaine 2 : Découverte du code

### Structure type d'un projet

```
project/
├── .github/                 # GitHub Actions, templates
├── docker/                  # Fichiers Docker
├── docs/                    # Documentation
├── src/                     # Code source
│   ├── components/         # Composants UI (frontend)
│   ├── modules/            # Modules métier (backend)
│   ├── services/           # Services
│   └── utils/              # Utilitaires
├── tests/                   # Tests
├── .env.example            # Variables d'environnement (template)
├── docker-compose.yml      # Configuration Docker
├── package.json            # Dépendances Node.js
└── README.md               # Documentation du projet
```

### Conventions à connaître

| Document | Contenu |
|----------|---------|
| [CONTRIBUTING.md](../CONTRIBUTING.md) | Comment contribuer |
| [BRANCH_NAMING.md](BRANCH_NAMING.md) | Nommage des branches |
| [COMMIT_CONVENTIONS.md](COMMIT_CONVENTIONS.md) | Messages de commit |
| [STYLEGUIDE.md](STYLEGUIDE.md) | Style de code |
| [TESTING.md](TESTING.md) | Stratégie de tests |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Principes d'architecture |

### Workflow de développement

```
1. Prendre un ticket JIRA
        ↓
2. Créer une branche
   git checkout -b feature/PROJ-123-description
        ↓
3. Développer
   - Écrire le code
   - Écrire les tests
   - Vérifier le linting
        ↓
4. Commiter
   git commit -m "feat(scope): description"
        ↓
5. Pousser
   git push -u origin feature/PROJ-123-description
        ↓
6. Créer une Pull Request
        ↓
7. Code Review
        ↓
8. Merge
```

### Premier commit

Pour votre premier commit, nous suggérons une tâche simple :
- Corriger une typo dans la documentation
- Ajouter un test manquant
- Améliorer un message d'erreur

Cela vous permettra de vous familiariser avec le workflow sans pression.

## Ressources essentielles

### Documentation interne

| Ressource | URL | Description |
|-----------|-----|-------------|
| Confluence | confluence.mairistem.fr | Documentation technique |
| JIRA | jira.mairistem.fr | Gestion de projet |
| Grafana | grafana.mairistem.fr | Monitoring |
| Swagger/API Docs | api.mairistem.fr/docs | Documentation API |

### Documentation externe

| Sujet | Ressource |
|-------|-----------|
| TypeScript | [typescriptlang.org](https://www.typescriptlang.org/docs/) |
| React | [react.dev](https://react.dev/) |
| Next.js | [nextjs.org/docs](https://nextjs.org/docs) |
| Laravel | [laravel.com/docs](https://laravel.com/docs) |
| NestJS | [docs.nestjs.com](https://docs.nestjs.com/) |
| Tailwind CSS | [tailwindcss.com/docs](https://tailwindcss.com/docs) |
| Docker | [docs.docker.com](https://docs.docker.com/) |
| Kubernetes | [kubernetes.io/docs](https://kubernetes.io/docs/) |

### Channels Slack/Teams importants

| Channel | Usage |
|---------|-------|
| `#general` | Annonces générales |
| `#dev-general` | Discussions techniques |
| `#dev-help` | Aide et questions |
| `#devops` | Infrastructure, déploiements |
| `#random` | Discussions informelles |
| `#[votre-équipe]` | Channel de votre équipe |

## Contacts clés

| Rôle | Qui contacter | Pour quoi |
|------|---------------|-----------|
| **Votre manager** | Voir organigramme | Questions générales, objectifs |
| **Tech Lead** | Voir équipe | Questions techniques, architecture |
| **DevOps** | devops@mairistem.fr | Accès, déploiements, infra |
| **IT Support** | it@mairistem.fr | Matériel, accès, VPN |
| **RH** | rh@mairistem.fr | Administratif |

## FAQ Onboarding

### Je n'ai pas accès à un repository

1. Vérifiez que vous êtes membre de l'organisation GitHub
2. Demandez l'accès à votre manager ou Tech Lead
3. Le DevOps ajoutera les permissions nécessaires

### Comment lancer le projet en local ?

1. Consultez le README.md du projet
2. Vérifiez les prérequis (Node.js, PHP, Docker)
3. Copiez `.env.example` vers `.env`
4. Lancez `docker-compose up -d` si applicable
5. Lancez `npm run dev` ou équivalent

### Comment créer ma première Pull Request ?

1. Créez une branche : `git checkout -b feature/PROJ-XXX-description`
2. Faites vos modifications
3. Commitez : `git commit -m "feat(scope): description"`
4. Poussez : `git push -u origin feature/PROJ-XXX-description`
5. Ouvrez GitHub et cliquez "Create Pull Request"
6. Remplissez le template et assignez des reviewers

### Qui peut reviewer mon code ?

- Les membres de votre équipe
- Le Tech Lead du projet
- Les personnes définies dans CODEOWNERS

### Je suis bloqué, que faire ?

1. Consultez la documentation (Confluence, README)
2. Cherchez dans les issues GitHub (ouvertes et fermées)
3. Demandez sur Slack `#dev-help`
4. Contactez votre Tech Lead ou un collègue

### Comment accéder aux environnements de test ?

1. Connectez-vous au VPN si nécessaire
2. Les URLs sont dans Confluence (section Environnements)
3. Les credentials sont dans le gestionnaire de secrets

## Checklist d'intégration

### Semaine 1

- [ ] Accès email configuré
- [ ] Slack/Teams rejoint
- [ ] GitHub accès obtenu
- [ ] JIRA/Confluence accès obtenu
- [ ] VPN configuré (si nécessaire)
- [ ] Environnement de dev installé
- [ ] Premier projet cloné et lancé

### Semaine 2

- [ ] Documentation lue (CONTRIBUTING, conventions)
- [ ] Premier commit/PR réalisé
- [ ] Participation à une code review
- [ ] Rencontre avec l'équipe

### Premier mois

- [ ] Première fonctionnalité livrée
- [ ] Familiarisation avec l'architecture
- [ ] Autonomie sur les tâches simples

---

Bienvenue dans l'équipe ! N'hésitez pas à poser des questions, tout le monde est passé par là.

Pour toute suggestion d'amélioration de ce guide, ouvrez une PR ou contactez votre Tech Lead.
