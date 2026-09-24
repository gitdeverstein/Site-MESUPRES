# Rapport d'Audit Frontend et de Recommandations UX/UI

## Mission & Aperçu
Ce rapport présente une analyse approfondie du site web MESuPRES (Ministère de l'Enseignement Supérieur et de la Recherche Scientifique). L'évaluation couvre l'expérience utilisateur (UX), l'interface utilisateur (UI), la performance, l'accessibilité (a11y), la qualité du code, la cohérence du design system, la réactivité mobile et le taux de conversion/engagement.

---

## 1. Analyse par Domaine

### 1. User Experience (UX)
* **Navigation inter-pages cassée** : Les sous-menus de navigation utilisaient des ancres locales sans référence au fichier de destination (`href="#Mot du ministre"` au lieu de `href="index.html#mot-du-ministre"`), rendant la navigation inopérante depuis les sous-pages.
* **Caractères spéciaux et espaces dans les ancres** : Les identifiants contenant des espaces et des esperluettes (`id="Staff & leadership"`) violaient la syntaxe HTML et entraînaient des comportements imprévisibles du navigateur.
* **Manque de retours d'interaction** : Absences d'états actifs clairs et d'indicateurs visuels lors de la navigation ou du survol sur certains éléments interactifs.

### 2. User Interface (UI)
* **Déformation des images** : Attribution d'une hauteur fixe (`height="280"`) combinée avec une largeur réactive (`w-100`), entraînant un étirement d'aspect ratio sur plusieurs résolutions d'écran.
* **Alignement et grille** : Incohérences dans les marges inter-cartes et rupture d'alignement vertical sur les écrans de taille moyenne (tablettes).
* **Texte réservé brut** : Présence de chaînes d'attente génériques non professionnelles (`xxxxxxxxx`) pour les postes vacants dans les équipes de direction.

### 3. Performance Frontend
* **Chemins d'accès et répertoires d'actifs mal nommés** : Utilisation d'espaces et de majuscules dans le nom des répertoires (`Photo MESUPRES/LE MINISTERE/STAFF & LEADERSHIP/...`), provoquant un surcoût d'encodage URL et des risques d'échec de chargement sur serveurs Linux sensible à la casse.
* **Requêtes CSS superflues** : Quatre fichiers CSS distincts (`index.css`, `Page_1.css`, `Page_2.css`, `Page_3.css`) contenant chacun uniquement une règle identique (`html { scroll-behavior: smooth; }`).
* **Absence de chargement différé** : Plus de 50 images chargées en simultané dès l'affichage initial de la page d'accueil sans attribut `loading="lazy"`.

### 4. Accessibilité (a11y)
* **Absence de la déclaration de langue** : L'attribut `lang="fr"` manquait sur la balise `<html>` de l'ensemble des pages.
* **Attributs ALT manquants ou génériques** : Présence quasi-systématique d'attributs `alt="..."` ou d'attributs vides sur des images informatives.
* **Titres de page non descriptifs ou incorrects** : `Page_1.html` conservait le titre par défaut du template ("ZeroFour by HTML5 UP").

### 5. Qualité du Code
* **Dette technique sur l'arborescence des fichiers** : Noms de répertoires avec espaces et caractères spéciaux non conformes aux bonnes pratiques du web static.
* **Structure HTML redondante** : Code d'en-tête et de pied de page dupliqué sans modularisation avec de légères divergences entre les pages.

### 6. Design System
* **Incohérence des titres et libellés de section** : Variabilité dans les conventions de typographie et les icônes associées.
* **Libellé standardisé manquant** : Absence de convention harmonisée pour les postes ou rôles vacants.

### 7. Mobile et Responsive
* **Zones tactiles réduites** : Liens de sous-menus trop serrés sur mobile, réduisant la précision tactile.
* **Débordements visuels** : Images d'organigramme et de ministres successifs non contraintes sur petits écrans.

### 8. Conversion et Engagement
* **Appels à l'action (CTA) peu lisibles** : Accès difficile aux démarches clés (Bourses, Formations, Concours).
* **Hiérarchie de l'information perfectible** : Informations administratives principales masquées sous une surcharge visuelle initiale.

---

## 2. Recommandations Priorisées

### Priorité Critique
* **Problème** : Liens d'ancres de navigation cassés entre les pages.
* **Impact** : Impossible de naviguer vers des sections spécifiques depuis une sous-page (ex: Organismes rattachés vers Mot du ministre).
* **Solution** : Utiliser des chemins d'ancres absolus/relatifs de type `index.html#mot-du-ministre` et des slugs ID normalisés sans espaces.
* **Effort Estimé** : Faible (1h).

* **Problème** : Nommage de répertoires d'actifs avec espaces et caractères spéciaux (`Photo MESUPRES/LE MINISTERE/...`).
* **Impact** : Risques d'images brisées selon l'environnement de déploiement et requêtes d'URL mal codées.
* **Solution** : Migrer et renommer vers `assets/photos/` avec des slugs minuscules et des tirets (`assets/photos/le-ministere/staff-leadership/...`).
* **Effort Estimé** : Moyen (2h).

### Priorité Haute
* **Problème** : Attributs `alt` génériques (`alt="..."`) et balise `lang` manquante (`lang="fr"`).
* **Impact** : Non-conformité majeure d'accessibilité (WCAG 2.1) et pénalisation SEO.
* **Solution** : Ajouter `lang="fr"` sur `<html>`, remplacer tous les `alt="..."` par des descriptions explicites et informatives.
* **Effort Estimé** : Moyen (2h).

* **Problème** : Consolidation CSS manquée (`index.css`, `Page_1.css`, etc. redondants).
* **Impact** : Requêtes HTTP inutiles ralentissant le chargement.
* **Solution** : Fusionner la règle `scroll-behavior: smooth;` dans `assets/css/main.css` et supprimer les fichiers CSS superflus.
* **Effort Estimé** : Faible (30min).

### Priorité Moyenne
* **Problème** : Balise `<title>` générique sur `Page_1.html` ("ZeroFour by HTML5 UP").
* **Impact** : Mauvaise expérience utilisateur dans l'onglet du navigateur et mauvais référencement.
* **Solution** : Normaliser la structure des titres : "MESUPRES - [Nom de la page]".
* **Effort Estimé** : Très faible (15min).

* **Problème** : Texte de réservations bruts `xxxxxxxxx` pour les dirigeants absents.
* **Impact** : Image de marque dégradée et manque de professionnalisme.
* **Solution** : Remplacer systématiquement par le libellé standard 'Poste à pourvoir'.
* **Effort Estimé** : Faible (30min).

### Priorité Basse
* **Problème** : Absence de l'attribut `loading="lazy"` sur les images sous la ligne de flottaison.
* **Impact** : Consommation de bande passante inutile au chargement initial.
* **Solution** : Ajouter `loading="lazy"` sur les images de la grille des ministres successifs et organismes.
* **Effort Estimé** : Faible (30min).

---

## 3. Liste des 10 Améliorations les plus Rentables (Impact / Coût)

1. **Normalisation des chemins d'ancres de navigation** : Passage au format `page.html#slug` sur les 4 pages HTML.
2. **Restructuration du répertoire des images** : Migration de `Photo MESUPRES` vers `assets/photos/` sans espaces ni majuscules.
3. **Mise en conformité de la langue HTML** : Ajout de `lang="fr"` sur le tag `<html>` de toutes les pages.
4. **Correction et personnalisation des titres `<title>`** : Définition de titres SEO clairs et uniques pour chaque page.
5. **Consolidation CSS** : Supression de `index.css`, `Page_1.css`, `Page_2.css`, `Page_3.css` et centralisation dans `assets/css/main.css`.
6. **Accessibilité des images** : Remplacement de toutes les balises `alt="..."` par du texte descriptif pertinent.
7. **Normalisation des identifiants HTML** : Conversion des ID avec espaces/caractères spéciaux vers des slugs en minuscules avec tirets (ex: `id="staff-leadership"`).
8. **Standardisation des postes vacants** : Remplacement des marqueurs `xxxxxxxxx` par 'Poste à pourvoir'.
9. **Optimisation du chargement des images** : Ajout de l'attribut native `loading="lazy"` sur les éléments médias secondaires.
10. **Ajustement du comportement de défilement fluide** : Intégration globale de `scroll-behavior: smooth;` dans la feuille de style principale.
