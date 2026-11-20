# TP3 – Jenkins ⇆ Docker : Build → Test → Push → Deploy

## 1. Présentation du projet

Ce projet est réalisé dans le cadre du TP3 de la matière **Introduction au DevOps**, en 3ème année Licence en Informatique de Gestion à l'Institut Supérieur de Gestion de Sousse (ISG).

L'objectif est de mettre en place une chaîne CI/CD permettant d'automatiser les différentes étapes du cycle de vie d'une application web simple :

- **Build** : construction d'une image Docker à partir d'un Dockerfile.
- **Test** : vérification du bon fonctionnement de l'application.
- **Push** : publication de l'image Docker sur Docker Hub.
- **Deploy** : déploiement de l'application avec Docker Compose.

L'application web utilisée est une page HTML/CSS statique servie par Nginx.

## 2. Technologies et outils utilisés

- **Git & GitHub** : gestion du code source et versionnement.
- **Jenkins** : automatisation du pipeline CI/CD.
- **Docker** : conteneurisation de l'application.
- **Docker Hub** : stockage et distribution de l'image Docker.
- **Docker Compose** : gestion et déploiement du conteneur.
- **Nginx Alpine** : serveur web léger.
- **Visual Studio Code** : environnement de développement.
- **Windows + WSL2** : environnement d'exécution Docker.

## 3. Structure du projet

```text
TP3-Jenkins-Docker/
│
├── index.html
├── style.css
├── Dockerfile
├── Jenkinsfile
├── docker-compose.yml
└── screenshots/
```

## 4. Description des fichiers

| Fichier | Description |
|---|---|
| `index.html` | Structure de la page web. |
| `style.css` | Mise en forme de l'application. |
| `Dockerfile` | Instructions pour construire l'image Docker. |
| `Jenkinsfile` | Définition du pipeline CI/CD Jenkins. |
| `docker-compose.yml` | Configuration du déploiement avec Docker Compose. |
| `screenshots/` | Captures d'écran des différentes étapes réalisées. |

## 5. Réalisation du projet

### Étape 1 – Préparation du dépôt GitHub

Création d'un dépôt GitHub pour héberger le code source du projet.

Dépôt : [TP3-Jenkins-Docker](https://github.com/ayedamina04-stack/TP3-Jenkins-Docker)

Les fichiers du projet sont versionnés avec Git et hébergés sur la branche `main`.

### Étape 2 – Conteneurisation avec Docker

Création du fichier `Dockerfile` pour construire une image basée sur Nginx Alpine.

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
COPY style.css /usr/share/nginx/html/style.css

EXPOSE 80
```

Cette configuration permet de servir l'application web à travers un serveur Nginx exécuté dans un conteneur Docker.

### Étape 3 – Configuration du pipeline Jenkins

Création d'un fichier `Jenkinsfile` pour automatiser les étapes du pipeline.

Le pipeline comprend les étapes suivantes :

1. **Checkout** : récupération du code source depuis GitHub.
2. **Docker Build** : construction de l'image Docker.
3. **Smoke Test** : lancement temporaire d'un conteneur et vérification de la réponse HTTP.
4. **Push (Docker Hub)** : authentification et publication de l'image sur Docker Hub.

Les identifiants Docker Hub sont configurés dans Jenkins à travers les Credentials, avec l'identifiant `dockerhub-creds`.

L'image publiée est :

`amina47/monapp:latest`

### Étape 4 – Exécution et validation du pipeline

Le pipeline Jenkins a été exécuté avec succès.

La construction de l'image, le test de fonctionnement et la publication sur Docker Hub ont été validés.

Le résultat final du pipeline est :

**Finished: SUCCESS**

### Étape 5 – Publication sur Docker Hub

Après la construction et les tests, l'image Docker est publiée sur Docker Hub afin de pouvoir être récupérée et utilisée pour le déploiement.

Image Docker :

`amina47/monapp:latest`

### Étape 6 – Déploiement avec Docker Compose

Création du fichier `docker-compose.yml` :

```yaml
services:
  web:
    image: amina47/monapp:latest
    ports:
      - "8084:80"
    restart: unless-stopped
```

Explication :

- `image` : image Docker récupérée depuis Docker Hub.
- `ports` : redirection du port 8084 de la machine hôte vers le port 80 du conteneur.
- `restart: unless-stopped` : redémarrage automatique du conteneur sauf s'il est arrêté manuellement.

Les commandes suivantes ont été exécutées :

```bash
docker compose config
docker compose pull
docker compose up -d
docker ps
```

Le conteneur a été créé et démarré avec succès.

L'application est accessible à l'adresse :

**http://localhost:8084**

### Étape 7 – Vérification finale

L'application a été testée dans le navigateur et affiche correctement le message :

**Bienvenue sur mon application web !**

*C'est un projet simple pour démontrer l'intégration continue avec Jenkins.*

La commande `docker ps` confirme également que le conteneur est actif
