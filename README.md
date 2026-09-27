# Bilan Sens — Hijabi Fit

Clone adapté de `bilan-sens-rijalfit` pour un public féminin — même moteur
(scoring, jauges, graphique, silhouette, analyse complète, email/Sheet),
habillage différent : palette noir/blanc/rose, copy au féminin, **aucune
photo pour l'instant** (le site d'origine utilise des photos d'un modèle
homme — retirées ici plutôt que montrées à un public féminin).

**Aucune IA, aucun PDF.** Tout est calculé instantanément côté navigateur
(scoring du quiz + formules de composition corporelle + moteur d'analyse
déterministe), sans appel serveur autre que l'enregistrement du lead.

## Différences avec bilan-sens-rijalfit (le site hommes)

- **Palette** : noir `#1A1A1A` / blanc / rose `#C2185B` (variables CSS
  historiques `--kaki`/`--or`/`--beige` conservées dans `style.css` — seules
  leurs valeurs ont changé, pour ne pas casser tout le code qui les
  référence).
- **Aucune image de fond ni miniature de réponse** : les écrans plein-écran
  (`photo-bg`) utilisent un dégradé rose au lieu d'une photo. Si tu obtiens
  un jour un set de photos femmes équivalent à celui de Rijal Fit (une image
  de fond + 3 portraits de réponse par question, format `qN-bg.jpg` /
  `qN-a/b/c.jpg` dans `images/`), il suffit de les déposer dans `images/` et
  de rajouter les champs `bg`/`optionImages` sur chaque question dans
  `QUESTIONS` (`script.js`) — la structure du code les supporte déjà.
- **Formule de taux de graisse (Deurenberg) et seuils Faible/Moyen/Élevé**
  recalibrés pour la physiologie féminine (le corps féminin porte
  naturellement plus de masse grasse essentielle — les seuils hommes
  auraient classé "Élevé" un taux tout à fait sain pour une femme).
- **Analyse hormonale** réécrite (périménopause/œstrogènes après 40 ans, au
  lieu de la testostérone).
- **Zones** renommées : Hijabi en Éveil / Hijabi Fragilisée / Hijabi en
  Déclin.
- Toutes les questions, messages de zone et conseils d'analyse ont été
  relus et adaptés au féminin (accords, "sœurs" au lieu de "frères", etc.)
  — à relire une fois de plus si tu ajustes le texte, certains accords
  peuvent avoir été manqués.
- **Photo du coach retirée** du "Bilan de Sens personnalisé" (signature
  générique "L'équipe Hijabi Fit" en attendant que tu précises qui signe —
  toi, ta femme, ou les deux).
- Lien Calendly final **encore celui de Rijal Fit** — à remplacer par un
  lien dédié Hijabi Fit si tu en crées un.

## Mise en route

### 1. Créer la Google Sheet + le script d'envoi email (indépendant de celui de Rijal Fit)

1. Crée une **nouvelle** Google Sheet dédiée à Hijabi Fit (sheets.new) — ne
   réutilise pas celle de Rijal Fit, pour ne pas mélanger les leads.
2. Menu **Extensions → Apps Script**.
3. Supprime le code par défaut et colle le contenu de
   [`google-apps-script/Code.gs`](google-apps-script/Code.gs).
4. **Déployer → Nouveau déploiement** → type **Application web** — Exécuter
   en tant que **Moi**, Qui a accès : **Tout le monde**.
5. Autorise l'accès (c'est ton propre script).
6. Copie l'URL donnée à la fin (`https://script.google.com/macros/s/.../exec`).

Ce script envoie aussi le Bilan Sens par email à chaque prospect (`MailApp`,
depuis ton compte Gmail, sans service tiers). Même quota Gmail que pour
Rijal Fit (~100 emails/jour), partagé entre les deux sites si c'est le même
compte Google.

### 2. Créer un nouveau projet GitHub + Vercel

Ce dossier n'est **pas encore** un dépôt git ni un projet Vercel — c'est une
copie locale. Étapes :
1. Crée un nouveau dépôt GitHub (vide).
2. Depuis ce dossier : `git init`, `git remote add origin <url>`,
   `git add -A`, `git commit`, `git push`.
3. Sur vercel.com → **Add New → Project** → importe ce dépôt GitHub.
4. Dans **Settings → Environment Variables** du nouveau projet Vercel,
   ajoute `GOOGLE_SHEETS_WEBHOOK_URL` = l'URL copiée à l'étape 1.
5. Une fois déployé, mets à jour `og:url`/`og:image`/`twitter:image` dans
   `index.html` avec le vrai domaine Vercel obtenu (un `hijabi-fit.vercel.app`
   est supposé dans le code actuel, à corriger si Vercel attribue autre chose).

### 3. Tester

Même checklist que Rijal Fit : quiz complet, bio → analyse sans photo →
10 questions → porte d'entrée → résultat complet (silhouette, jauges,
analyse complète, projection), ligne dans la Sheet, email reçu, lien
Calendly.

## Structure

Identique à `bilan-sens-rijalfit` — voir son propre README pour le détail
du moteur (`QUESTIONS`/`ZONES` dans `script.js`, `api/log-lead.js`,
`google-apps-script/Code.gs`).
