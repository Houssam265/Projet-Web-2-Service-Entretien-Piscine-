# Projet-Web-2-Service-Entretien-Piscine.
# Convention de code — Plateforme Entretien de Piscines

---

## 1. Principes généraux

* Langage principal : **PHP 8+ (POO)**, architecture **MVC**.
* Base de données : **MySQL**.
* Front : HTML5 + CSS + JS (vanilla + AJAX).
* Indentation : **4 espaces**.
* Tous les accès BD via **PDO** et **requêtes préparées**.
* Chaque classe et méthode doit avoir un **DocBlock** (phpdoc).

---

## 2. Nommage

* **Classes** : `PascalCase` → `Intervenant`, `Controller`.
* **Méthodes** : `camelCase` → `createDemande()`, `verifierDisponibilite()`.
* **Variables** : `camelCase` → `$dateDebut`, `$prixHeure`.
* **Constantes** : `SCREAMING_SNAKE` → `TAUX_TVA`.
* **Fichiers PHP** : `PascalCase.php` pour classes (`Intervenant.php`), `snake_case.php` pour vues (`index.php`).
* **Tables** (DB) : `PascalCase`, Singulier: `Intervenant`, `Client`, `Service`, `Disponibilite`.
* **Colonnes** : `camelCase` → `idIntervenant`, `heureDebut`.

---

## 3. Structure de dossiers

```
/app/
  /controllers/
    Main.php
    Controller.php
  /models/
    Model.php
  /views/
    /public/
      /images/
      /js/
      /styles/
    /client/
      index.php
    /admin/
      index.php
    /Intervenant/
      index.php
    Home.php
    404.php
    /utils/
      Mailer.php
      PdfGenerator.php
    /config/
      Database.php
      Config.php
    .htaccess
    index.php
```

---

## 4. Modèle

* **Single Responsibility** : chaque modèle gère la logique métier + accès DB pour une entité.
* Toujours renvoyer types stricts (`int`, `array|null`, `bool`) si possible (PHP 8).

---

## 5. Contrôleurs

* Les contrôleurs orchestrent : valider la requête → appeler le modèle → renvoyer une vue ou JSON.
* Garder le contrôleur léger : pas de logique métier complexe dedans.

---

## 6. Vues et JS (front-end)

* Séparer logique et présentation : aucune requête SQL dans les vues.
* Toutes les interactions asynchrones via **XMLHttpRequest** (JSON).

---

## 7. Règles métiers — contraintes

* Un intervenant **doit** être connecté pour proposer un service.
* Authentification obligatoire pour effectuer une demande service (client).
* Après intervention, l’agent envoie formulaire d’évaluation -> stocker `Review`.
* Les feedbacks sont publiés **après** que les deux retours (client & intervenant) soient reçus ou après 7 jours.

---

## 8. Sécurité

* Toujours utiliser **requêtes préparées PDO**.
* Nettoyage : `htmlspecialchars()` sur sorties, `filter_var()` pour entrées.
* Stocker mots de passe avec `password_hash()` et `password_verify()`.
* Validation côté client **et** serveur.
* Limiter tentatives de connexion / lockout.
* Vérifier autorisations : `isAdmin()`, `isClient()`, `isIntervenant()` (vérifier que l’utilisateur peut accéder à une ressource).

---

## 9. Emails et notifications

* Centraliser envoi mails dans `utils/Mailer.php`.
* Templates email en HTML + texte (multipart).
* Attribuer constantes dans `config/Config.php` : `EMAIL_ADMIN`, `BASE_URL`, `SMTP_*`.
* Envoyer mail de confirmation reservation + facture PDF jointe.

---

## 10. Softwares / outils utilisés

* **Git/GitHub**.
* **Composer** si libs externes.
* **Trello** pour tâches.
* **Figma** pour maquettes.

---

## 11. Tests & qualité

* Créer tests unitaires (PHPUnit) pour la logique métier critique : disponibilité, création réservation, calcul prix.
* Linting (PHP-CS-Fixer) : règles PSR12 + notre indentation/DocBlocks.
