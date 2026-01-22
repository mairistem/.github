# Politique de Sécurité

## Versions supportées

Nous fournissons des correctifs de sécurité pour les versions suivantes :

| Version | Support sécurité |
|---------|------------------|
| Dernière version stable | :white_check_mark: Supportée |
| Version N-1 | :white_check_mark: Supportée |
| Versions antérieures | :x: Non supportées |

## Signaler une vulnérabilité

La sécurité de nos projets est une priorité. Si vous découvrez une vulnérabilité de sécurité, nous vous encourageons à nous la signaler de manière responsable.

### Comment signaler

**Ne créez PAS d'issue publique pour les vulnérabilités de sécurité.**

Envoyez un email à : **security@mairistem.fr**

### Informations à inclure

Pour nous aider à traiter votre signalement efficacement, veuillez inclure :

1. **Description** : Description détaillée de la vulnérabilité
2. **Impact** : Impact potentiel de la vulnérabilité
3. **Reproduction** : Étapes pour reproduire le problème
4. **Environnement** : Version du logiciel, OS, configuration
5. **Preuve de concept** : Code ou captures d'écran (si applicable)
6. **Suggestions** : Corrections ou mitigations suggérées (si applicable)

### Template de signalement

```
## Résumé
[Description courte de la vulnérabilité]

## Sévérité estimée
[Critique / Haute / Moyenne / Basse]

## Produit/Service affecté
[Nom du projet, version, module]

## Description détaillée
[Description complète de la vulnérabilité]

## Étapes de reproduction
1. [Première étape]
2. [Deuxième étape]
3. [...]

## Impact
[Description de l'impact potentiel]

## Preuve de concept
[Code, captures d'écran, ou logs]

## Correction suggérée
[Si vous avez des suggestions]

## Informations de contact
[Comment vous joindre pour des questions]
```

## Notre processus

### Délais de réponse

| Étape | Délai |
|-------|-------|
| Accusé de réception | 24-48 heures ouvrées |
| Évaluation initiale | 5 jours ouvrés |
| Mise à jour de statut | Tous les 7 jours |
| Résolution (critique) | 7-14 jours |
| Résolution (haute) | 30 jours |
| Résolution (moyenne/basse) | 90 jours |

### Processus de traitement

```
1. Réception du signalement
        ↓
2. Accusé de réception (24-48h)
        ↓
3. Triage et évaluation (5 jours)
        ↓
4. Investigation et reproduction
        ↓
5. Développement du correctif
        ↓
6. Tests et validation
        ↓
7. Déploiement du correctif
        ↓
8. Communication publique
        ↓
9. Reconnaissance du chercheur
```

## Classification des vulnérabilités

Nous utilisons le système CVSS (Common Vulnerability Scoring System) pour évaluer la sévérité :

| Score CVSS | Sévérité | Description |
|------------|----------|-------------|
| 9.0 - 10.0 | Critique | Impact majeur, exploitation facile |
| 7.0 - 8.9 | Haute | Impact significatif |
| 4.0 - 6.9 | Moyenne | Impact modéré |
| 0.1 - 3.9 | Basse | Impact limité |

## Types de vulnérabilités

### Dans le scope

- Injection (SQL, XSS, Command, etc.)
- Authentification et gestion de session
- Contrôle d'accès défaillant
- Exposition de données sensibles
- Configuration de sécurité incorrecte
- Composants vulnérables
- Problèmes cryptographiques
- SSRF (Server-Side Request Forgery)
- Désérialisation non sécurisée

### Hors scope

- Attaques de type Denial of Service (DoS/DDoS)
- Ingénierie sociale
- Sécurité physique
- Vulnérabilités dans des dépendances tierces déjà publiquement connues
- Rapports de scanners automatisés sans preuve d'exploitation
- Problèmes de best practices sans impact de sécurité direct

## Divulgation responsable

Nous suivons les principes de divulgation responsable :

1. **Confidentialité** : Ne divulguez pas publiquement la vulnérabilité avant qu'elle soit corrigée
2. **Coordination** : Travaillez avec nous pour déterminer un calendrier de divulgation
3. **Délai standard** : 90 jours entre le signalement et la divulgation publique
4. **Exceptions** : En cas de risque immédiat pour les utilisateurs, nous pouvons accélérer le processus

## Reconnaissance

Nous apprécions les chercheurs en sécurité qui nous aident à améliorer nos produits.

### Hall of Fame

Les chercheurs ayant contribué à notre sécurité seront reconnus dans notre Hall of Fame (avec leur accord).

### Ce que nous offrons

- Reconnaissance publique (si souhaitée)
- Lettre de remerciement officielle
- Référence pour votre CV/portfolio

### Ce que nous attendons

- Respecter notre politique de divulgation responsable
- Ne pas accéder aux données d'autres utilisateurs
- Ne pas perturber nos services
- Fournir suffisamment d'informations pour reproduire le problème

## Bonnes pratiques de sécurité

### Pour les contributeurs

- Ne jamais commiter de secrets (mots de passe, clés API, tokens)
- Utiliser des variables d'environnement pour les configurations sensibles
- Valider et assainir toutes les entrées utilisateur
- Utiliser des requêtes préparées pour les bases de données
- Maintenir les dépendances à jour
- Suivre le principe du moindre privilège

### Outils recommandés

- **SAST** : SonarQube, Semgrep
- **DAST** : OWASP ZAP
- **Dépendances** : Dependabot, Snyk
- **Secrets** : git-secrets, truffleHog

## Contact

Pour toute question relative à la sécurité :

- **Email sécurité** : security@mairistem.fr
- **PGP Key** : [Disponible sur demande]

---

Merci de contribuer à la sécurité de nos projets.
