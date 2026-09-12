# Stage Finder

Plateforme web de mise en relation entre étudiants et entreprises pour la recherche
de stages et d'alternances. Application Laravel 11 avec gestion fine des rôles et
permissions, recherche d'offres géolocalisée, candidatures, wishlists et
évaluations d'entreprises. Installable comme application mobile (PWA).

Projet de groupe : module développement web.

## Fonctionnalités

**Côté étudiant**
- Recherche et filtrage des offres par secteur, département, ville et compétences.
- Consultation des fiches entreprises et de leurs offres en cours.
- Candidature en ligne avec suivi de l'état de chaque dossier.
- Wishlist d'offres, ajout et retrait en un clic.
- Évaluation des entreprises après un stage.

**Côté pilote**
- Création, édition et suppression des offres et des entreprises.
- Gestion des comptes étudiants et des promotions (classes).
- Consultation des candidatures et des statistiques par offre.

**Côté administrateur**
- Gestion complète des utilisateurs, y compris les pilotes.
- Tableau de bord avec statistiques sur les offres, les entreprises et les étudiants.

## Rôles et permissions

Trois rôles sont définis via [spatie/laravel-permission](https://spatie.be/docs/laravel-permission) :
`Etudiant`, `Pilote` et `Admin`. Les permissions sont granulaires
(`create_offer`, `apply_for_offer`, `evaluate_company`, `view_offer_stats`,
`delete_student`…) et attribuées par rôle dans `database/seeders/PermissionSeeder.php`.

## Stack technique

| Couche | Technologie |
|---|---|
| Backend | PHP 8.2, Laravel 11 |
| Frontend | Blade, Tailwind CSS 3, Vite 6 |
| Base de données | MySQL / MariaDB (Eloquent, migrations, seeders) |
| Permissions | spatie/laravel-permission 6 |
| PWA | silviolleite/laravelpwa 2 |

## Installation

Prérequis : PHP 8.2+, Composer, Node.js 18+, un serveur MySQL ou MariaDB.

```bash
git clone https://github.com/Zargan3IQ/Stage-Finder.git
cd Stage-Finder

composer install
npm install

cp .env.example .env
php artisan key:generate
```

Renseignez les accès à la base dans `.env`, puis :

```bash
php artisan migrate --seed
npm run build
php artisan serve
```

L'application est disponible sur <http://localhost:8000>.

En développement, lancez `npm run dev` en parallèle pour le rechargement à chaud
des assets.

## Jeu de données de démonstration

`php artisan migrate --seed` alimente la base avec des régions, départements,
villes et codes postaux français, des secteurs d'activité, des compétences, des
promotions, ainsi que des entreprises, offres, utilisateurs, candidatures,
wishlists et évaluations fictifs. Les comptes de test et leurs mots de passe sont
définis dans `database/seeders/UserSeeder.php`.

## Modèle de données

| Modèle | Rôle |
|---|---|
| `User` | Comptes, rattachés à une classe et à un rôle |
| `Company` | Entreprises, liées à un ou plusieurs secteurs |
| `Offer` | Offres de stage, liées à une entreprise, des départements et des compétences |
| `Application` | Candidatures, avec un statut (`Status`) |
| `Evaluation` | Notes et avis laissés sur une entreprise |
| `Skill`, `Sector`, `Classe` | Référentiels |
| `Region`, `Department`, `City` | Découpage géographique |

Les tables pivot (`offers_skills`, `offers_departments`, `companies_sectors`,
`users_classes`, `wishlists`) gèrent les relations plusieurs-à-plusieurs.

## Structure du projet

```
├── app/
│   ├── Http/Controllers/   # Auth, Offer, Company, Application, Evaluation, Wishlist, Dashboard, Profile, User
│   └── Models/             # 12 modèles Eloquent
├── database/
│   ├── migrations/         # 23 migrations
│   └── seeders/            # 19 seeders, dont le référentiel géographique
├── resources/views/        # Vues Blade par domaine + pages institutionnelles
├── routes/web.php          # ~60 routes nommées
└── config/                 # Configuration Laravel, permissions et PWA
```

## Auteurs

Projet réalisé en groupe. Développement original : [benjaminbourlet/Projet-WEB](https://github.com/benjaminbourlet/Projet-WEB).
