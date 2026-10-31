# Rapport de Projet — CI/CD Lab

**Réalisé par :** Firas Khdhir  
**Projet :** Mise en place d'une chaîne CI/CD  
**Technologies :** Node.js, TypeScript, Express, GitHub Actions et Render

---

## 1. Introduction

Ce projet a pour objectif de concevoir et de mettre en œuvre une chaîne d'intégration continue (CI) et de déploiement continu (CD) pour une application web développée avec Node.js, TypeScript et Express.

La solution permet d'automatiser la vérification du code source, l'exécution des tests, la compilation de l'application et son déploiement sur la plateforme Render.

Le projet utilise également les Pull Requests, la protection de la branche principale et les déploiements temporaires pour améliorer la qualité et la fiabilité des modifications.

## 2. Architecture de l'application

L'application est développée avec Node.js, TypeScript et Express.

Elle expose trois endpoints HTTP :

| Méthode | Endpoint | Description |
|---|---|---|
| GET | `/` | Page d'accueil |
| GET | `/api/health` | Vérification de l'état de l'application |
| GET | `/api/version` | Consultation de la version de l'application |

Le serveur utilise la variable d'environnement `PORT`, avec 3000 comme valeur par défaut.

La version de l'application est définie par `APP_VERSION`. En l'absence de cette variable, la valeur utilisée est `development`.

### Structure du projet

- `src/app.ts` : définition de l'application Express et des routes.
- `src/server.ts` : démarrage du serveur HTTP.
- `test/app.test.ts` : tests automatisés.
- `.github/workflows/ci.yml` : configuration du pipeline CI.
- `captures/` : preuves visuelles des différentes étapes.
- `package.json` : dépendances et scripts npm.
- `tsconfig.json` : configuration TypeScript.

## 3. Préparation et vérification locale

Après la récupération du dépôt, les dépendances sont installées avec :

```bash
npm ci
```

La vérification complète est exécutée avec :

```bash
npm run verify
```

Cette commande regroupe trois opérations :

1. Vérification TypeScript avec `tsc --noEmit`.
2. Exécution des tests automatisés avec Node.js Test Runner.
3. Compilation de l'application avec `tsc`.

Les résultats finaux obtenus sont :

| Vérification | Résultat |
|---|---|
| TypeScript | Réussi |
| Test `/api/health` | Réussi |
| Test `/api/version` | Réussi |
| Total des tests | 2 réussis |
| Tests échoués | 0 |
| Build | Réussi |

## 4. Gestion du code source avec Git et GitHub

Le projet est hébergé dans un dépôt GitHub public :

https://github.com/firaskhdhir11-lab/cicd-lab

La branche principale utilisée est `main`.

Les modifications sont développées dans des branches dédiées, puis proposées à travers des Pull Requests.

Les principales opérations réalisées comprennent :

- Initialisation du dépôt Git.
- Création du dépôt distant GitHub.
- Création et utilisation de branches fonctionnelles.
- Commit et Push des modifications.
- Création et fusion des Pull Requests.

Cette organisation permet de séparer le développement, la vérification et l'intégration des modifications.

## 5. Intégration continue avec GitHub Actions

Le pipeline CI est défini dans :

`.github/workflows/ci.yml`

Il est déclenché lors :

- d'un Push vers `main` ;
- de l'ouverture ou de la mise à jour d'une Pull Request ciblant `main`.

### Étapes du pipeline

1. Récupération du code avec `actions/checkout`.
2. Configuration de Node.js 24 avec `actions/setup-node`.
3. Installation des dépendances avec `npm ci`.
4. Exécution de `npm run verify`.

Le contrôle de statut obligatoire est nommé :

`Verify`

Les exécutions du workflow ont confirmé le bon fonctionnement des vérifications automatiques, notamment après la fusion des Pull Requests.

## 6. Protection de la branche principale

Un GitHub Ruleset nommé `Protect main` a été créé et activé.

Les règles configurées sont :

- Ciblage de la branche `main`.
- Obligation de passer par une Pull Request.
- Nombre d'approbations requis : 0.
- Contrôle obligatoire `Verify`.
- Liste de contournement vide.

Cette configuration empêche l'intégration de modifications dans `main` lorsque le contrôle CI obligatoire échoue.

La méthode de fusion utilisée est la création d'un commit de fusion.

## 7. Déploiement continu avec Render

L'application est hébergée sur Render à travers un Web Service.

URL de production :

https://cicd-lab-ctve.onrender.com

### Configuration du service

| Paramètre | Valeur |
|---|---|
| Nom | cicd-lab |
| Branche | main |
| Runtime | Node.js |
| Instance | Free |
| Build Command | `npm ci && npm run build` |
| Start Command | `npm start` |
| Auto-Deploy | After CI Checks Pass |
| Pull Request Previews | Automatic |

Le déploiement automatique est configuré pour intervenir après la réussite des contrôles CI.

La page d'accueil et les endpoints de l'application ont été vérifiés sur le service de production.

## 8. Démonstration d'un échec CI et sa correction

Une démonstration volontaire d'échec a été réalisée afin de vérifier le comportement du pipeline CI.

### 8.1 Création de l'erreur

Un test automatisé a été ajouté pour vérifier l'endpoint :

`GET /api/version`

Le test attendait un statut HTTP 200.

Cependant, l'endpoint n'étant pas encore implémenté, le serveur retournait HTTP 404.

Les résultats étaient les suivants :

- Test `/api/health` : réussi.
- Test `/api/version` : échoué.
- Total : 1 réussite et 1 échec.

GitHub Actions a détecté cet échec et le contrôle obligatoire `Verify` a bloqué la fusion de la Pull Request.

### 8.2 Correction

La route `/api/version` a ensuite été ajoutée dans `src/app.ts`.

Elle retourne la valeur de `APP_VERSION`, avec `development` comme valeur par défaut.

Après la correction :

- Test `/api/health` : réussi.
- Test `/api/version` : réussi.
- Total : 2 tests réussis.
- Échecs : 0.

Le pipeline CI a alors réussi et la Pull Request a pu être fusionnée.

Cette démonstration confirme le rôle des tests automatisés et de la protection de branche dans la prévention des erreurs.

## 9. Déploiements temporaires des Pull Requests

La fonctionnalité Pull Request Previews de Render a été configurée sur :

`Automatic`

Une Pull Request de test a été créée pour vérifier le fonctionnement de cette fonctionnalité.

Un déploiement temporaire indépendant a été généré :

https://cicd-lab-pr-4-6xhl.onrender.com/

La page de prévisualisation affichait notamment le message :

`Preview deployment test.`

Après la fusion de la Pull Request, Render a détruit le déploiement temporaire.

Cette fonctionnalité permet de tester les modifications avant leur intégration dans la branche principale.

## 10. Principales Pull Requests réalisées

| Pull Request | Objectif | Résultat |
|---|---|---|
| #1 | Ajout du workflow GitHub Actions | Fusionnée, CI réussie |
| #2 | Mise à jour de la page d'accueil | Fusionnée, CI réussie |
| #3 | Démonstration d'un échec puis correction de l'API | Fusionnée après correction |
| #4 | Test du Preview Deployment Render | Fusionnée, preview vérifiée |

## 11. Preuves et captures d'écran

Les captures d'écran du projet sont enregistrées dans le répertoire `captures/`.

Elles documentent notamment :

- Le déploiement initial sur Render.
- La configuration de l'Auto-Deploy.
- La mise à jour de la page d'accueil.
- L'échec du contrôle CI.
- La réponse de l'API de version.
- Le fonctionnement du Preview Deployment.

Ces captures constituent des preuves visuelles des étapes réalisées.

## 12. Bilan du projet

Les principaux objectifs techniques ont été réalisés :

- Développement d'une application Express avec TypeScript.
- Mise en place des tests automatisés.
- Automatisation des vérifications avec GitHub Actions.
- Protection de la branche principale.
- Gestion des modifications par Pull Requests.
- Déploiement sur Render.
- Déploiement conditionné par la réussite des contrôles CI.
- Démonstration d'un échec CI et de sa correction.
- Vérification des déploiements temporaires.

Les vérifications locales finales ont donné deux tests réussis, sans échec, ainsi qu'une compilation réussie.

**Remarque sur la sécurité :** lors de l'installation initiale des dépendances, `npm ci` a signalé deux vulnérabilités, une de niveau modéré et une de niveau élevé. Elles restent à examiner avec `npm audit` avant de considérer l'audit de sécurité comme terminé.

## 13. Conclusion

Ce projet a permis de mettre en pratique les principes fondamentaux du DevOps à travers une chaîne CI/CD fonctionnelle.

L'association de GitHub Actions, des Pull Requests, de la protection de branche et de Render permet d'automatiser les contrôles et de sécuriser le processus de déploiement.

Le projet illustre également l'intérêt des tests automatisés et des environnements temporaires pour détecter les erreurs avant la mise en production.

---

**Réalisé par : Firas Khdhir**

Dépôt GitHub :  
https://github.com/firaskhdhir11-lab/cicd-lab

Application déployée :  
https://cicd-lab-ctve.onrender.com
