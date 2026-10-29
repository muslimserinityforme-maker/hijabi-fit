# Ton Bilan Santé — Hijabi Fit

Même bilan que celui de Rijal Fit (`bilan-sens-rijalfit`), pour un public
féminin : 9 questions, ≈ 2 minutes, et un résultat qui montre où la personne
en est dans ses **4 santés** : physique, mentale, sociale, spirituelle. Tout
est calculé dans le navigateur : aucune IA, aucun PDF. Le seul appel serveur
sert à enregistrer la personne dans la Google Sheet et à lui envoyer son
résultat complet par email.

## Parcours

1. **Intro** — le hadith « Ton corps a un droit sur toi. », une phrase claire.
2. **9 questions** — une par écran, 3 réponses (A/B/C = 0/1/2 points).
3. **Prénom ou surnom, email, âge (facultatif)** et case de consentement, juste
   avant le résultat.
4. **Résultat** — le profil (3 zones), les **4 barres en pourcentage** avec un
   commentaire par santé, le message de l'équipe (vidéo par profil si tu en as
   une, sinon texte), puis un seul appel à l'action : l'appel découverte offert.
   Le même retour complet part par email.

## Les 4 santés

| Santé | Questions |
|---|---|
| Physique | énergie au réveil, ventre, sommeil |
| Mentale | motivation, humeur |
| Sociale | sœurs / amies / communauté, famille |
| Spirituelle | prière, corps pendant les adorations |

Pourcentage = 100 − 80 × (points ÷ points max), arrondi à 5 (plancher à
**20 %**, jamais « 0 % »). Palier du commentaire : ≤ 25 % haut, ≤ 50 % moyen,
sinon bas. La santé la plus basse reçoit « À regarder en premier » (sauf si
toutes sont au palier haut) et c'est d'elle que parle le message du bas de page.

## Les 3 zones (score total sur 18)

| Points | Nom | Couleur |
|---|---|---|
| 0-6 | Réalignement stable | vert |
| 7-12 | Le cycle yoyo | rose |
| 13-18 | Le moment de se reprendre | rouge |

## Différences avec Rijal Fit

- Palette noir / blanc / rose `#C2185B` (variables CSS historiques
  `--kaki`/`--or`/`--beige` conservées, seules leurs valeurs ont changé).
- Aucune photo : les réponses sont en texte, les écrans pleine largeur ont un
  dégradé rose. Textes au féminin (« sœurs » au lieu de « frères »).
- Signature « L'équipe Hijabi Fit » (à préciser si tu veux une personne).

## À compléter dans `script.js` (en haut du fichier)

- `VIDEOS` : un lien YouTube/Vimeo par profil (`stable`, `yoyo`, `decrochage`).
  Vide : le message écrit s'affiche à la place.
- `WHATSAPP_URL` : un lien WhatsApp. Vide : le bouton est masqué.

## Google Sheet

`google-apps-script/Code.gs` écrit dans l'onglet **« Bilans »** (créé
automatiquement) : `Date · Prénom · Email · Âge · Statut · Score · Zone ·
Physique % · Mentale % · Sociale % · Spirituelle % · Santé à regarder en
premier · Problème n°1 · Date résultat`, une ligne par personne qui a terminé,
et envoie l'email de résultat complet (compte Gmail qui a déployé le script).

**Si tu modifies `Code.gs` :** Déployer → Gérer les déploiements → crayon →
Nouvelle version → Déployer (l'URL ne change pas).

## Structure

```
index.html                   → intro, questions, prénom/email, résultat
style.css                    → design Hijabi Fit (noir / blanc / rose)
script.js                    → questions, 4 santés, zones, enregistrement du lead
api/log-lead.js              → fonction serverless : nettoie et transmet au script Google
google-apps-script/Code.gs   → à coller dans la Google Sheet Hijabi Fit
bilan-morpho-anatomique.html → outil autonome (11 questions → exercices à proscrire)
```

## Déploiement

GitHub `muslimserinityforme-maker/hijabi-fit` (branche `main`) → Vercel, adresse
`hijabi-fit.vercel.app`. Variable Vercel : `GOOGLE_SHEETS_WEBHOOK_URL` = l'URL de
déploiement du script Google.
