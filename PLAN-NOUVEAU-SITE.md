# Nouveau site Psychoboxe — Plan technique (site + base de données + espace admin)

*Suite de [PLAN-REFONTE.md](PLAN-REFONTE.md). Ce document décrit **comment** construire le nouveau site. Maquette visuelle : [index.html](index.html) — en ligne sur https://lippido.github.io/psychoboxe/.*

---

## 1. Objectif

Un site rapide et fluide sur téléphone, dont **tout le contenu vient d'une base de données**. Un membre autorisé se connecte à un **espace admin**, remplit un formulaire (nouvelle actualité, nouvelle formation, nouveau praticien…) et le site public se met à jour tout seul, sans toucher au code.

```
Visiteur  ──►  Site public (pages rapides, lisibles sur mobile)
                      ▲ lit
                      │
               Base de données  ◄── écrit ──  Espace admin (connexion par compte)
```

---

## 2. Choix techniques recommandés

| Besoin | Choix | Pourquoi |
|---|---|---|
| Base de données + comptes + fichiers | **Supabase** (région **UE**, Francfort/Paris) | Base PostgreSQL, connexion des comptes, stockage des PDF/images, offre gratuite suffisante, données en Europe (RGPD) |
| Site public + espace admin | **Astro** (rendu à la demande) + un peu de JavaScript pour les formulaires | Pages très légères et rapides sur mobile, bon référencement Google, un seul projet pour le public et l'admin |
| Hébergement | **Cloudflare Pages** (ou Netlify) | Gratuit, HTTPS automatique, déploiement à chaque modification du code |
| Nom de domaine | `psychoboxe.com` conservé | ~10–15 €/an, à rattacher au nouvel hébergement |
| Code source | **GitHub** (dépôt de l'association) | Historique, sauvegarde, possibilité de passer la main à un autre bénévole |

**Alternative plus simple si personne ne veut gérer de code** : rester sur Wix et utiliser Wix CMS (collections + membres). Moins flexible, abonnement payant, pas de code récupérable — voir PLAN-REFONTE.md §0.

**Coût estimé** : domaine ≈ 10–15 €/an, le reste à 0 € tant que l'on reste sous les limites gratuites (largement le cas pour ce site).

> ⚠️ Point d'attention : sur l'offre gratuite de Supabase, un projet **sans aucune visite pendant 7 jours est mis en pause**. Le site public étant consulté régulièrement ça ne devrait pas arriver, mais prévoir une sonde de surveillance gratuite (UptimeRobot) qui ouvre une page chaque jour.

---

## 3. Structure de la base de données

| Table | Contenu | Champs principaux |
|---|---|---|
| `actualites` | Actualités et nouveautés | titre, date, texte, image, lien(s), épinglée (oui/non), publiée (oui/non) |
| `formations` | Une formation par ville | ville, année, titre, description, PDF, lien d'inscription, date de début, visible jusqu'au |
| `membres_bureau` | Bureau + conseil d'administration | nom, fonction, ordre d'affichage, statut (bureau / CA / président d'honneur) |
| `praticiens` | Annuaire : didacticiens et praticiens | nom, prénom, statut, région, ville, email, actif |
| `references` | Articles, presse, vidéos, PDF | type, titre, auteur, lien ou fichier, date |
| `interventions` | Interventions auprès de structures / de personnes | catégorie, titre, texte |
| `partenaires` | Associations partenaires | nom, lien, email, description |
| `pages_texte` | Textes de pages peu changeantes | identifiant (`presentation`, `qu-est-ce-que-la-psychoboxe`, `mot-du-president`, `mentions-legales`, `confidentialite`…), titre, contenu |
| `messages_contact` | Messages envoyés via le formulaire | nom, email, message, date, traité (oui/non) |
| `admins` | Comptes administrateurs (aucun compte visiteur) | lien avec le compte de connexion, nom |

Les fichiers (PDF, images) sont stockés dans le **stockage de fichiers** Supabase, la base ne conserve que leur adresse.

**Règles de sécurité de la base (Row Level Security)** — le point le plus important :
- Un visiteur anonyme peut **lire uniquement** les lignes publiées/visibles, et **jamais** `messages_contact` ni `admins`.
- Seul un administrateur connecté peut ajouter / modifier / supprimer, gérer les comptes admin et lire les messages de contact.
- Le formulaire de contact peut **insérer** un message mais pas relire les autres.

---

## 4. Espace admin

**Adresse** : `psychoboxe.com/admin` (non lié depuis le menu public, non indexé par Google).

**Connexion** — réservée aux administrateurs. Les visiteurs n'ont **aucun compte à créer** : tout le site public se consulte librement.
- Compte par **e-mail + mot de passe**, avec **double authentification** (application sur téléphone).
- **Aucune inscription libre** : un admin invite un nouvel admin (invitation par e-mail).
- Mot de passe oublié par e-mail, session qui expire automatiquement.
- Un seul rôle, `admin` : ajouter / modifier / supprimer tout le contenu, textes légaux, comptes admin, lecture des messages.

**Écrans** (un par table de la section 3, tous sur le même modèle)
1. **Tableau de bord** : raccourcis « Nouvelle actualité », « Nouvelle formation », « Nouveau praticien », messages non lus.
2. **Liste** : recherche, tri, bouton « Modifier », « Masquer », « Supprimer » (avec confirmation).
3. **Formulaire** : champs clairs, aperçu avant publication, envoi d'images/PDF par glisser-déposer, bouton « Enregistrer en brouillon » / « Publier ».
4. **Annuaire** : import d'un tableau (fichier CSV) pour reprendre la liste actuelle.
5. **Comptes admin** : inviter, désactiver.

**Journal des modifications** : chaque ajout/modification enregistre *qui* et *quand*, pour retrouver une erreur.

---

## 5. Pages du site public

Reprend la structure de PLAN-REFONTE.md §3, alimentée par la base :

| Page | Source des données |
|---|---|
| Accueil | `pages_texte` (mot du président) + 3 dernières `actualites` + prochaines `formations` |
| L'Institut | `pages_texte` (présentation) + `membres_bureau` |
| La Psychoboxe | `pages_texte` (définition, citation, mention INPI, déontologie) |
| Actualités | `actualites` (liste + une page par actualité) |
| Formations | `formations` (une carte par ville) |
| Références | `references`, filtrables par type |
| Interventions | `interventions` + `partenaires` |
| Annuaire | `praticiens` : filtre par région, recherche par nom/ville, regroupé didacticiens / praticiens |
| Contact | formulaire → `messages_contact` + signalement de pratiques non déontologiques |
| Mentions légales, Confidentialité | `pages_texte` (modifiables par un admin) |

Exigences communes :
- **Mobile d'abord** : menu repliable, texte lisible, boutons assez grands pour le pouce, annuaire en cartes (pas en colonnes d'espaces).
- Pas de bandeau publicitaire, pas de traceur → **pas de bandeau cookies nécessaire** (à conserver : n'ajouter ni Google Analytics, ni pixel Facebook, ni vidéo YouTube intégrée sans mode « sans cookie »).
- Vidéos YouTube : intégrées via `youtube-nocookie.com` ou simple lien/vignette.
- Images redimensionnées automatiquement, texte alternatif obligatoire dans le formulaire admin.
- Titre et description propres à chaque page (référencement), plan du site (`sitemap.xml`).
- Contraste et taille de texte conformes (accessibilité de base).

---

## 6. Protection des données (RGPD)

- Base hébergée en **UE**.
- **Annuaire** : les membres listés ont déjà donné leur accord pour la publication de leur nom et de leur e-mail ; un praticien peut demander son retrait ou une correction → case « actif » décochée.
- **Messages de contact** : conservés 12 mois puis supprimés (tâche automatique ou rappel manuel annuel).
- **Sauvegarde** : export hebdomadaire de la base (script ou fonction Supabase) stocké chez l'association ; le code reste sur GitHub.
- **Politique de confidentialité** et **mentions légales** rédigées avec les informations demandées dans PLAN-REFONTE.md §2 (RNA/SIRET, siège, directeur de publication, hébergeurs : Cloudflare et Supabase).
- Aucun mot de passe ni clé secrète dans le code public ; clés stockées dans les réglages de l'hébergeur.

---

## 7. Étapes de réalisation

| # | Étape | Résultat | Dépend de |
|---|---|---|---|
| 0 | Décisions du bureau (voir §8) + création des comptes GitHub, Supabase, Cloudflare au nom de l'association | Accès centralisés, pas liés à une seule personne | — |
| 1 | Récupération du contenu actuel (textes, images, PDF, annuaire) en fichiers propres | Dossier de contenu corrigé (fautes et liens cassés de PLAN-REFONTE §1) | 0 |
| 2 | Création de la base : tables, règles de sécurité, comptes admin | Base vide et protégée | 0 |
| 3 | Import des données existantes (annuaire, bureau, actualités, formations, références) | Base remplie | 1, 2 |
| 4 | Squelette du site public : en-tête, menu mobile, pied de page, pages vides | Maquette navigable sur téléphone | 0 |
| 5 | Pages publiques branchées sur la base | Site public complet | 3, 4 |
| 6 | Espace admin : connexion, tableau de bord, formulaires, envoi de fichiers | Les admins peuvent publier seuls | 2, 5 |
| 7 | Formulaire de contact + pages légales + double authentification | Site conforme | 5, 6 |
| 8 | Tests : mobile (iPhone, Android), anciens téléphones, test de sécurité (un visiteur ne peut ni écrire ni lire les données privées), vitesse | Liste de défauts corrigés | 7 |
| 9 | Relecture par 2–3 membres, formation des futurs admins (guide d'1 page + démonstration) | Équipe autonome | 8 |
| 10 | Mise en ligne : bascule de `psychoboxe.com`, redirection des anciens liens, surveillance, résiliation de Wix | Site en production | 9 |

Ordre conseillé : le site public (étapes 4–5) peut être validé visuellement **avant** de construire l'admin, pour arrêter tôt si l'apparence ne convient pas.

---

## 8. Décisions et informations à obtenir du bureau

1. Valider l'approche **Supabase + Astro + Cloudflare** (ou choisir l'alternative Wix CMS).
2. **Qui seront les admins** (2 personnes minimum, pour ne pas dépendre d'une seule) ?
3. Qui crée et détient les comptes GitHub / Supabase / Cloudflare / domaine (au nom de l'association, avec une adresse e-mail partagée type `contact@…`) ?
4. Les informations légales (RNA/SIRET, siège, directeur de publication, organisme de formation).
5. Qui arbitre le contenu du site (relecture avant publication, ou chaque admin publie directement) ?
6. Faut-il un espace réservé aux membres (documents internes, comptes rendus d'A.G.) ? Non prévu ici ; à ajouter en option.

---

## 9. Hors périmètre (à ne pas faire dans la première version)

Boutique ou paiement en ligne, inscription en ligne aux formations avec règlement, newsletter, espace membres avec connexion pour les praticiens, version en plusieurs langues, application mobile. Tous ajoutables plus tard sans refaire le site.
