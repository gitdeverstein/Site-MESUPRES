# Rapport d'Audit et d'Amélioration Frontend - MESUPRES Web

**Auteur :** Senior Frontend Engineer & UX Reviewer
**Projet :** Site Web MESUPRES (Ministère de l'Enseignement Supérieur et de la Recherche Scientifique)
**Date :** Octobre 2023

---

## 1. Analyse Détaillée par Domaine

### 1. User Experience (UX)
* **Navigation inter-pages cassée :** Les menus de navigation de chaque page pointent vers des ancres locales sans inclure le chemin du fichier parent (ex: `<a href="#Mot du ministre">` au lieu de `index.html#Mot-du-ministre`). Cela empêche l'utilisateur d'accéder aux sous-sections depuis n'importe quelle page.
* **Espaces et caractères spéciaux dans les ancres :** Les ancres contiennent des espaces et des caractères accentués (ex: `#Staff & leadership`, `#Rattachés au ministre`), provoquant des comportements erratiques sur la plupart des navigateurs modernes.
* **Absence d'états actifs clairs sur les sous-menus :** L'utilisateur ne sait pas exactement où il se trouve lorsqu'il navigue au sein d'une longue page multi-sections.
* **Contenus manquants / Placeholders :** De nombreux postes ministériels affichent un nom générique ou incomplet ("Poste à pourvoir").

### 2. User Interface (UI)
* **Incohérences visuelles et Hiérarchie :** Répétition excessive de la balise `<div id="content">` sur une même page (`index.html`), ce qui casse la structure du conteneur principal et altère les marges et l'alignement vertical.
* **Problèmes d'encodage et chemins d'images :** Le dossier d'images principal est nommé `Photo MESUPRES` avec des sous-dossiers comme `LE MINISTERE/STAFF & LEADERSHIP`. Les espaces et `&` dans les URLs provoquent des risques de liens brisés (HTTP 404 / URL Encoding issues).
* **Consistance visuelle des titres :** Différences de typographie et de taille sur les titres de niveau 2 (`<h2>`) entre `index.html` et `Page_3.html`.

### 3. Frontend Performance
* **Fichiers CSS redondants :** Quatre fichiers CSS distincts (`index.css`, `Page_1.css`, `Page_2.css`, `Page_3.css`) contiennent la même unique règle CSS (`html { scroll-behavior: smooth; }`), générant 4 requêtes HTTP superflues.
* **Images non optimisées :** Aucune image ne possède l'attribut `loading="lazy"`. L'ensemble des 50+ photos HD (ex: portraits des ministres successifs) est chargé au rendu initial.
* **Chargement d'assets inutilisés :** Script Dropotron et bibliothèques FontAwesome incluses sur toutes les pages sans optimisation du rendu.

### 4. Accessibility (A11y)
* **Absence d'attribut `lang` :** La balise `<html>` n'a pas la déclaration de langue `lang="fr"`, empêchant les lecteurs d'écran de prononcer correctement le texte français.
* **Absence d'attributs `alt` significatifs :** Plus de 80 images disposent d'un attribut `alt` vide, d'une valeur sans signification (`alt="..."`, `alt="logo2"`) ou manquante.
* **Structure de balisage non conforme :** Plusieurs éléments interactifs manquent de rôles ARIA et d'états d'accentuation (`:focus`) visibles au clavier.
* **Titres de page non descriptifs :** `Page_1.html` possède la balise `<title>ZeroFour by HTML5 UP</title>` au lieu d'un titre représentatif.

### 5. Code Quality & Maintainability
* **Incohérence des identifiants (IDs) :** Utilisation d'identifiants dupliqués (`id="content"` présent 3 fois dans `index.html`) et d'IDs non valides (`id="..."`).
* **Architecture CSS fragmentée :** Inclusion d'un fichier CSS spécifique par page au lieu d'intégrer les règles transversales dans `assets/css/main.css`.
* **Nommage des fichiers HTML :** Utilisation des noms génériques `Page_1.html`, `Page_2.html`, `Page_3.html` au lieu de slugs sémantiques.

### 6. Design System Consistency
* **Typographie et espacements :** Marges incohérentes entre les sections d'organismes rattachés et les listes de ministres.
* **Boutons & CTA :** Les liens d'action manquent d'unification visuelle (boutons primaires vs secondaires non formalisés).

### 7. Mobile & Responsive Design
* **Menus déroulants sur mobile :** Le menu Dropotron n'adapte pas correctement ses sous-éléments au toucher sur petit écran, causant des superpositions.
* **Zones de clic trop réduites :** Les liens du menu de navigation secondaire ont une hauteur de ligne inférieure à 44px sur mobile.

### 8. Conversion & Engagement Rates
* **Absence d'appels à l'action (CTA) clairs :** La page "Formation" liste des offres sans boutons d'action explicites ("Postuler", "En savoir plus").
* **Hiérarchie de l'information :** Le mot du ministre et les coordonnées essentielles sont relégués sous de volumineux blocs de texte.

---

## 2. Plan d'Action Recommandé par Priorité

### Priority: Critical
1. **Navigation & Anchors Cassées**
   * **Problème :** Les ancres du menu principal ne fonctionnent pas depuis les sous-pages et contiennent des caractères spéciaux (`#Staff & leadership`).
   * **Impact :** Expérience utilisateur bloquante, navigation impossible inter-pages.
   * **Solution :** Utiliser des URLs relatives absolues avec ancres nettoyées (ex: `index.html#Staff-leadership`, `Page_1.html#Rattaches-au-ministre`).
   * **Effort Estimé :** 1 heure.

2. **Normalisation des Chemins d'Assets (Images)**
   * **Problème :** Dossiers et fichiers contenant des espaces et caractères spéciaux (`Photo MESUPRES/LE MINISTERE/STAFF & LEADERSHIP`).
   * **Impact :** Risque fort d'images cassées (404) sur divers serveurs web / navigateurs.
   * **Solution :** Renommer le dossier en `Photo_MESUPRES` et assainir la hiérarchie en slugs minuscules/majuscules sans espaces.
   * **Effort Estimé :** 1 heure.

### Priority: High
3. **Accessibilité : Déclaration de Langue et Titres SEO/A11y**
   * **Problème :** Balise `<html>` sans `lang="fr"` et `<title>` par défaut (`ZeroFour by HTML5 UP`).
   * **Impact :** Mauvais référencement (SEO) et incompatibilité avec la synthèse vocale (accessibilité).
   * **Solution :** Ajouter `lang="fr"` sur chaque page et fournir un `<title>` unique et explicite (`MESUPRES - Organismes Rattachés`).
   * **Effort Estimé :** 30 minutes.

4. **Accessibilité : Textes Alternatifs des Images (`alt`)**
   * **Problème :** Attributs `alt="..."` ou absents sur plus de 80 images.
   * **Impact :** Non-conformité WCAG 2.1 AA.
   * **Solution :** Ajouter des descriptions précises pour chaque image (ex: `alt="Portrait du Ministre de l'Enseignement Supérieur"`).
   * **Effort Estimé :** 1.5 heures.

### Priority: Medium
5. **Consolidation CSS et Suppression des Fichiers Dupliqués**
   * **Problème :** Fichiers `index.css`, `Page_1.css`, `Page_2.css`, `Page_3.css` redondants.
   * **Impact :** Requêtes HTTP superflues et dette technique.
   * **Solution :** Déplacer `scroll-behavior: smooth;` dans `assets/css/main.css` et supprimer les fichiers CSS individuels.
   * **Effort Estimé :** 30 minutes.

6. **Optimisation des Performances : Lazy Loading des Images**
   * **Problème :** Chargement simultané de toutes les images dès l'ouverture de la page.
   * **Impact :** Consommation de bande passante inutile et temps de chargement perçu élevé sur mobile.
   * **Solution :** Ajouter l'attribut native `loading="lazy"` sur l'ensemble des images sous la ligne de flottaison.
   * **Effort Estimé :** 45 minutes.

7. **Nettoyage de la Structure DOM et des Identifiants Invalides**
   * **Problème :** IDs dupliqués (`id="content"`) et IDs invalides (`id="..."`).
   * **Impact :** Non-conformité aux normes HTML5 et dysfonctionnement potentiel des scripts JS.
   * **Solution :** Nettoyer le balisage HTML pour avoir des IDs uniques et valides.
   * **Effort Estimé :** 1 heure.

### Priority: Low
8. **Boutons CTA & Engagement sur la Page Formation**
   * **Problème :** Absence de boutons clairs pour candidater ou télécharger les dossiers.
   * **Impact :** Taux de conversion et d'engagement faible pour les étudiants/chercheurs.
   * **Solution :** Ajouter des boutons CTA "Candidater" ou "En savoir plus" à chaque offre.
   * **Effort Estimé :** 1 heure.

---

## 3. Liste des 10 Améliorations les Plus Rentables (Impact / Coût)

1. **Correction des ancres de navigation inter-pages** (`index.html#Ancre`).
2. **Renommage du dossier d'images `Photo MESUPRES` vers `Photo_MESUPRES`** et nettoyage des sous-dossiers.
3. **Mise en place de `lang="fr"`** sur les balises `<html>` de toutes les pages.
4. **Correction des balises `<title>`** pour chaque page du site.
5. **Consolidation des styles CSS** dans `assets/css/main.css` et suppression de `index.css` à `Page_3.css`.
6. **Ajout des attributs `alt` significatifs** sur toutes les images du ministère et des ministres successifs.
7. **Activation du `loading="lazy"`** sur toutes les images sous la ligne de flottaison.
8. **Suppression des IDs HTML dupliqués et invalides** (`id="..."`).
9. **Amélioration du contraste et des zones tactiles** pour le menu de navigation mobile.
10. **Valorisation des appels à l'action (CTA)** sur les offres de bourses, concours et formations.
