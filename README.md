# 📰 Blog Strapi - TP CMS Headless

Blog moderne développé avec **Strapi v5** (backend headless CMS) et **SvelteKit** (frontend).

---

## 🚀 Objectif du TP

Découvrir et pratiquer l’usage d’un **CMS headless** combiné à un framework frontend moderne.  
Ce projet consiste à créer un blog collaboratif où :

- ✍️ **Auteurs (role Author)** : rédigent, modifient et publient leurs propres articles.
- 👀 **Visiteurs (role Public)** : consultent les articles publiés et peuvent laisser des commentaires.

---

## ⚙️ Installation

### Backend (Strapi)

- Se placer dans le dossier **backend**
- Installer les dépendances avec `npm install`
- Lancer le serveur avec `npm run develop`

### Frontend (SvelteKit)

- Se placer dans le dossier **frontend**
- Installer les dépendances avec `npm install`
- Lancer le serveur avec `npm run dev`

### Configuration

- Copier le fichier **.env.example** en **.env**
- Ajuster les variables si nécessaire

---

## 🌐 Accès

- **Interface d’administration Strapi** → [http://localhost:1338/admin](http://localhost:1338/admin)
- **Frontend du blog** → [http://localhost:5173](http://localhost:5173)

---

## 🛠 Modélisation Strapi

### Content Type **Article**

- Titre _(String, requis)_
- Slug _(UID unique basé sur le titre)_
- Contenu _(Rich Text)_
- Image de couverture _(Media, image unique)_
- Auteur _(Relation many-to-one avec User)_

### Content Type **Comment**

- Contenu du commentaire _(Text, requis)_
- Nom de l’auteur _(String, requis)_
- Date de publication _(DateTime, basé sur createdAt)_
- Relation avec l’article concerné _(many-to-one avec Article)_

---

## 🔐 Rôles & Permissions

### Visiteur (Public)

- Peut consulter les articles publiés
- Peut poster des commentaires

### Auteur (Author)

- Peut créer des articles
- Peut modifier et supprimer **uniquement ses propres articles**
- Peut poster des commentaires

---

## 🛡 Restrictions d’édition

Une **policy personnalisée** empêche un auteur de modifier ou supprimer un article qui ne lui appartient pas.  
Cette vérification est appliquée sur les routes **PUT** et **DELETE** des articles.

---

## 📡 Endpoints REST principaux

- **Authentification** : connexion via email et mot de passe
- **Articles** :
  - Récupérer les articles publiés avec leur image et leur auteur
  - Récupérer un article précis via son slug avec ses commentaires
- **Commentaires** : création d’un commentaire lié à un article

---

## 🎨 Frontend (SvelteKit)

### Pages réalisées

- **Accueil**

  - Liste des articles publiés sous forme de grille (image, titre, auteur, date)
  - Chaque carte redirige vers la page de l’article

- **Page Article**

  - Affiche image de couverture, titre, auteur, date, contenu
  - Liste des commentaires associés
  - Formulaire d’ajout de commentaire (nom + contenu)

---

## ✅ Fonctionnalités

- Gestion des **articles** et **commentaires**
- API REST basée sur **Strapi v5**
- Interface **responsive** et adaptée aux mobiles
- Permissions et rôles configurés
- UI claire et moderne

---

## 📚 Documentation utile

- [Quick Start Strapi v5](https://docs.strapi.io/cms/quick-start)
- [Content-type Builder](https://docs.strapi.io/cms/features/content-type-builder)
- [Plugin Users & Permissions](https://docs.strapi.io/dev-docs/plugins/users-permissions)
- [Policies](https://docs.strapi.io/cms/backend-customization/policies)
- [Document Service API](https://docs.strapi.io/cms/api/document-service)
- [Référence API REST](https://docs.strapi.io/cms/api/rest)
