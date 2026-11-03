# Refonte du site psychoboxe.com — Audit et plan de modifications

*Audit réalisé le 27/10/2026 sur https://www.psychoboxe.com/ (version ordinateur et mobile 375 px).*

---

## 0. Point important avant tout : le site est sur Wix

Le site est construit avec **Wix** (constructeur en ligne). Conséquences :

- **Il n'y a pas de « code de base » récupérable** : Wix ne permet pas d'exporter le site en HTML. On ne pourra récupérer que le **contenu** (textes, images, PDF), pas la structure.
- Bonne nouvelle : tout le contenu est lisible en ligne, il est donc entièrement récupérable (textes copiés, ~25 PDF et images téléchargeables).
- À demander au bureau : **qui a les identifiants Wix**, **qui gère le nom de domaine** `psychoboxe.com` (chez Wix ou ailleurs ?) et **combien coûte l'abonnement Wix actuel** (cette économie peut financer l'hébergement d'un nouveau site).

### Deux options possibles

| | Option A — Rester sur Wix | Option B — Nouveau site léger (recommandé) |
|---|---|---|
| Principe | Refaire la mise en page dans Wix (éditeur mobile, plusieurs pages) | Site statique simple + interface d'édition (type Decap CMS / Pages CMS) |
| Coût | Abonnement Wix actuel (~15–30 €/mois) | Hébergement gratuit (Netlify, GitHub Pages, Cloudflare Pages) + nom de domaine (~10–15 €/an) |
| Mise à jour | Éditeur Wix (déjà connu des membres ?) | Formulaire en ligne simple : « Ajouter une actualité », « Ajouter un praticien », « Ajouter une formation » |
| Mobile | Correct si bien refait, mais lourd | Rapide et fluide par construction |
| Cookies / RGPD | Wix dépose des cookies → bandeau obligatoire | Aucun traceur → pas de bandeau nécessaire |

**Recommandation : option B**, avec des contenus « structurés » (une fiche par actualité, par formation, par praticien) pour que la mise à jour se fasse sans toucher à la mise en page.

---

## 1. Erreurs relevées sur le site actuel

### 1.1 Fautes de frappe / d'orthographe

| Où | Actuel | Correction |
|---|---|---|
| Présentation | « loi du **1et** juillet 1901 » | 1er juillet 1901 |
| Présentation | « formateurs de la **psychoboxes** » | psychoboxe |
| Présentation | « modalités d'**agréements** » / « **réglement** intérieur » | agrément / règlement |
| Conseil d'administration | « **Benjamain** NEYER » | Benjamin |
| Actualités | « Freud **sur la ring** » | Freud sur **le** ring (titre réel du livre) |
| Actualités (×2) | « Paroles de **psychoxeurs** » | psychoboxeurs |
| Actualités / Références | « Valéry **MeynadieR** », « Valéry **Meynadie** » | Meynadier |
| Interventions | « formation d'équipe à la **psychobxe** » | psychoboxe |
| Formations | « **Monpellier** » | Montpellier |
| Annuaire | « OUEST **de le** France » | de la France |
| Annuaire | « **Govran** Sophie » mais email *sophie.**gorvan**@…* | vérifier l'orthographe du nom |
| Annuaire | *elisa.**caarta**@gmail.com* | vérifier l'adresse (double « a » ?) |

### 1.2 Liens cassés ou erronés

| Où | Problème |
|---|---|
| Actualités — épisodes 1 à 4 de Valéry Meynadier (Revue Bancal) | **Page introuvable (erreur 404)** → retrouver la nouvelle adresse ou héberger le texte (avec accord) |
| Présentation — « consultable en ligne **(lien)** » | Le mot « (lien) » n'est pas un lien → pointer vers le PDF du code de déontologie (déjà présent dans Références) |
| Association **COSTE** | Le lien ouvre un **email d'EMERGENCE** au lieu de https://www.assocoste.fr/ |
| Associations EMERGENCE et COSTE | Pointent toutes deux vers **le même groupe Facebook** |
| Association de Psychoboxe AURA | Le titre pointe vers un ancien blog (glppcclyon.wordpress.com) alors que le site est psychoboxeaura.fr |
| Centre Mediapsyazur | Un des deux liens est **vide** |
| Vidéo Ghislain Pateux | Lien qui passe par **Messenger** (l.messenger.com…) avec un jeton de suivi → remplacer par le lien YouTube direct |
| Lien Revue Bancal ép. 1 | Contient un traceur Facebook (`?fbclid=…`) |
| Lien « jeu » | Pointe vers `facebook.com/#` (page vide) |
| Annuaire — Germain Marc | Adresse email **non cliquable** (seule de la liste) |
| Plusieurs liens | En `http://` au lieu de `https://` (Revue Bancal, Huffington Post, Mediapsyazur) ; le lien France Bleu redirige désormais vers *ici.fr* |

### 1.3 Contenus incohérents ou périmés

- **Président** : le bureau indique *Ghislain Pateux* président, mais « Le mot du président » est signé *Lionel Raufast, Président d'honneur*, et une vidéo le présente comme « Président de l'Institut ». → Clarifier (« Le mot du président d'honneur » ou nouveau mot du président actuel).
- « *La Psychoboxe a 45 ans* » : à mettre à jour chaque année → écrire plutôt « fondée en 19XX ».
- Livre *La boxe comme médiation thérapeutique* « **à paraître en septembre 2026** » → il est paru, à mettre au présent.
- Documentaire « sur le site **au cours du mois de juillet** » → date dépassée.
- **Marque INPI** : déposée en 2002, renouvelée en 2012 → une marque dure 10 ans, elle **expirait en décembre 2022**. Vérifier qu'elle a bien été renouvelée et mettre à jour la mention.
- Rubrique **Références** : les épisodes de Valéry Meynadier sont recopiés en double (déjà dans Actualités).
- Formation de Nîmes : le texte « Bulletin d'inscription » est suivi de deux boutons « >>> » sans distinction.
- « Voici le dernier ouvrage de Richard Hellbrunn » : le titre du livre n'apparaît pas en texte (seulement dans l'image).

### 1.4 Problèmes techniques et d'ergonomie

- **Tout le site tient sur une seule page** : sur mobile, la page fait **plus de 26 000 pixels de haut** (≈ 32 écrans à faire défiler).
- Le **menu est affiché deux fois** (en haut et au milieu de la section « Qu'est-ce que la psychoboxe ? »).
- Sur mobile, un **bandeau publicitaire Wix** « Rejoignez-nous sur l'appli » reste collé en haut de l'écran.
- L'annuaire est mis en forme avec des **espaces** pour faire des colonnes : illisible sur mobile, non triable, pas de recherche.
- **Images** : 5 sans texte alternatif, et les autres ont pour texte alternatif un nom de fichier (« Capture d'écran 2026-05-29… », « 346160992_640194864192655… ») → mauvais pour l'accessibilité et le référencement.
- Le **titre « Le Conseil d'Administration »** apparaît *après* la liste des membres dans l'ordre de lecture (lecteurs d'écran).
- Bouton d'accueil « Du nouveau dans actualités » : formulation à revoir.
- Une seule page = un seul titre Google : mauvais référencement pour « formation psychoboxe Lyon », « psychoboxe Strasbourg », etc.

---

## 2. Pages manquantes pour être en règle

| Page | Obligation | Contenu à fournir par l'association |
|---|---|---|
| **Mentions légales** | **Obligatoire** (loi LCEN, art. 6) — absente aujourd'hui | Nom de l'association, adresse du siège, n° RNA (W…) et/ou SIRET, nom du **directeur de la publication** (le président), contact, **nom et adresse de l'hébergeur** |
| **Politique de confidentialité (RGPD)** | **Obligatoire** dès qu'on collecte des données (formulaire de contact, annuaire) | Responsable du traitement, données collectées, finalité, durée de conservation, droits (accès, rectification, suppression), contact, droit de réclamation auprès de la CNIL |
| **Gestion des cookies** | Obligatoire **si** traceurs non essentiels (c'est le cas avec Wix, **aucun bandeau aujourd'hui**) | Inutile si le nouveau site n'utilise aucun traceur (option B) |
| Retrait de l'annuaire | RGPD : droit de retrait et de rectification | Les membres listés ont déjà donné leur accord ; indiquer dans la politique de confidentialité comment demander un retrait ou une correction |
| Mention sous le formulaire de contact | RGPD | Phrase courte + lien vers la politique de confidentialité |

À vérifier selon la situation de l'Institut :
- Si l'Institut est lui-même **organisme de formation** : afficher son **numéro de déclaration d'activité** (et Qualiopi le cas échéant) sur la page Formations.
- Déclaration d'accessibilité : **non obligatoire** pour une association de cette taille (recommandée mais facultative).

---

## 3. Nouvelle structure proposée (tout le contenu actuel est conservé)

```
Accueil            → mot du président, présentation courte, 3 dernières actualités, prochaines formations
L'Institut         → Qui sommes-nous, missions, Bureau, Conseil d'administration, mot du président d'honneur
La Psychoboxe      → définition, déroulé, citation de R. Hellbrunn, mention INPI, code de déontologie
Actualités         → une fiche par actualité (livres, articles, vidéos, « Paroles de psychoboxeurs », collectif 68…)
Formations         → une fiche par ville (Toulouse, Lyon, Le Mans, Strasbourg, Nîmes, Paris, Montpellier) + PDF + bulletin
Conférences        → (section vide aujourd'hui : à remplir ou masquer)
Références         → articles, presse, vidéos, PDF (sans doublons)
Interventions      → auprès de structures / auprès de personnes, liens partenaires
Annuaire           → didacticiens et praticiens, filtrables par région, avec recherche
Contact            → formulaire + signalement de pratiques non déontologiques + flyers
Pied de page       → Mentions légales · Confidentialité · Contact · Facebook
```

Inventaire de contenu à récupérer : tous les textes ci-dessus, ~15 images, ~25 PDF (`/_files/ugd/…pdf`), liens vidéo YouTube, liens externes (corrigés), liste de l'annuaire (≈ 11 didacticiens, ≈ 45 praticiens).

---

## 4. Ce qui doit être facile à mettre à jour

Avec l'option B, les membres modifient via un formulaire en ligne, sans toucher au code :

| Contenu | Champs du formulaire |
|---|---|
| Actualité | titre, date, texte, image, lien(s) |
| Formation | ville, année, description, PDF, lien d'inscription |
| Praticien / didacticien | nom, prénom, statut, région, ville, email |
| Membres du bureau / CA | nom, fonction |
| Référence | type (article/presse/vidéo), titre, auteur, lien ou PDF |

Les actualités et formations passées peuvent être archivées automatiquement selon leur date.

---

## 5. Plan d'action par étapes

1. **Décisions du bureau** — choisir option A ou B, désigner 1 ou 2 personnes chargées des mises à jour, récupérer les accès Wix et du nom de domaine.
2. **Collecte des infos légales** — n° RNA/SIRET, adresse du siège, directeur de publication, statut organisme de formation, état de la marque INPI.
3. **Corrections immédiates sur le site actuel** (possible tout de suite dans Wix, sans attendre la refonte) — fautes de la section 1.1, liens de la section 1.2, mentions légales.
4. **Vérification de l'annuaire** — corriger les coordonnées douteuses relevées en 1.1.
5. **Récupération du contenu** — textes, images, PDF, liens (inventaire section 3).
6. **Maquette mobile d'abord** — validation de l'apparence par le bureau.
7. **Construction du site** + interface de mise à jour + pages légales.
8. **Relecture** par 2–3 membres sur téléphone et ordinateur.
9. **Bascule du nom de domaine** `psychoboxe.com` vers le nouveau site, redirection des anciens liens, puis résiliation de Wix.
10. **Petit guide** (1 page) « Comment ajouter une actualité / une formation / un praticien ».

---

## Questions à poser au bureau

- Qui possède les accès Wix et où est géré le nom de domaine ?
- Quel est le coût actuel de Wix ?
- N° RNA / SIRET et adresse du siège ?
- L'Institut est-il déclaré organisme de formation ?
- La marque « Psychoboxe » a-t-elle été renouvelée en 2022 ?
- Qui est aujourd'hui président : Ghislain Pateux ou Lionel Raufast ? Qui signe « Le mot du président » ?
- La section « Conférences » (vide) doit-elle être conservée ?
