# CONTEXT — Landing page « Estimation » Emilio Immobilier

> Fichier de suivi du projet. À garder à jour au fil des étapes.
> Dernière mise à jour : mise en ligne + sous-domaine OK.

---

## 🎯 Objectif
Landing page dédiée à la **génération de leads vendeurs** (demandes d'estimation) pour **Emilio Immobilier**, destinée à recevoir du trafic **Google Ads**.
- Positionnement : agent **indépendant titulaire de la carte professionnelle** (loi Hoguet), estimation par un **expert humain** (pas un algorithme), secteur **Paris + Hauts-de-Seine**.
- Objectif = **qualité** des leads (pas volume).

---

## 🌐 Infra & URLs
- **Repo GitHub** : `emilio92100/estimation-landingpage` (fichier unique `index.html` à la racine).
- **Hébergement** : Vercel (projet `estimation-landingpage`), connecté à GitHub → **chaque commit redéploie automatiquement**.
- **Domaine de prod** : **https://estimation.emilio-immo.com** ✅ (CNAME créé, HTTPS actif).
  - ⚠️ Toujours lancer la pub vers ce domaine, **jamais** vers l'URL `*.vercel.app`.
- Le site principal `emilio-immo.com` reste sur **Lovable (Lovable Cloud)** — projet **séparé**, non impacté.

### Comment mettre à jour la page
Éditer `index.html` sur GitHub (crayon) ou ré-uploader → commit → Vercel redéploie tout seul sur le même domaine.

---

## 📄 La page (version en ligne = FORMULAIRE)
Deux versions existaient (formulaire multi-étapes + chat conversationnel). **La version publiée est le FORMULAIRE.**

Sections : header (logo agrandi + tél + CTA) · hero (accroche + formulaire + WhatsApp) · présentation « Votre interlocuteur » (photo, badges 10 ans + carte pro CPI/CCI, encadré loi Hoguet) · « Idées reçues / La réalité » (4 cartes indépendant vs réseau) · méthode en 3 étapes · avis · secteurs · bandeau « 20 % d'écart » · footer.

Tunnel du formulaire (6 étapes) : 1) type de bien · 2) adresse (autocomplétion Base Adresse Nationale) · 3) le bien (dimensions, situation RDC/étage, état) · 4) extérieur & annexes · 5) projet de vente (délai) · 6) contact + consentement.

Coordonnées intégrées : mobile **06 58 95 76 32** · email **arogelet@emilio-immo.com** · carte pro **CPI 9201 2020 000 045 344**.

---

## ✅ Fait
- [x] Landing page (version formulaire) conçue et itérée.
- [x] Mise en ligne sur Vercel via GitHub.
- [x] Sous-domaine **estimation.emilio-immo.com** branché et actif (HTTPS OK).

## 🔴 À FAIRE avant de lancer la pub (bloquants)
- [ ] **Brancher le formulaire pour recevoir les leads.** Aujourd'hui il n'envoie RIEN.
      → Décision prise : **solution interne Supabase dédié + email Mailjet** (indépendant de Lovable).
      → Fichiers déjà préparés : `1-table-estimation-leads.sql` + `2-fonction-estimation-lead.ts`.
      → Étapes : créer un projet Supabase dédié → créer la table (SQL) → créer la fonction Edge `estimation-lead` (Verify JWT OFF) → ajouter les secrets Mailjet → coller `SUPABASE_FN_URL` + `SUPABASE_ANON_KEY` dans `index.html`.
      → (Placeholder actuel dans le code : `VOTRE_REF_PROJET` et `COLLEZ_VOTRE_CLE_ANON_ICI`.)
- [ ] **Mentions légales + politique de confidentialité** : liens du footer à faire pointer vers les vraies pages `emilio-immo.com` (à récupérer). Doivent contenir : N° CPI, garantie financière, assurance RCP.
- [ ] **Remplacer les avis** (Nathalie R., Julien & Claire, Mehdi B. = exemples) par de **vrais avis Google**.
- [ ] **Tester** une vraie soumission une fois branché (vérifier : ligne dans Supabase + email reçu).
- [ ] **Régler le moyen de paiement** du compte Google Ads (bloqué).
- [ ] **Configurer le suivi de conversion Google Ads** (type « Site Web », déclenché à l'envoi du formulaire).

## 🟡 À faire ensuite (fort impact, pas bloquant)
- [ ] Page **admin** (HTML protégée par login) pour voir les leads façon CRM (lecture table Supabase).
- [ ] **Bandeau de chiffres/résultats** (ventes, délai moyen, % du prix) si chiffres réels dispo.
- [ ] **Mini-FAQ** (honoraires, engagement, délai, pourquoi un expert).
- [ ] Ajuster la **liste des secteurs** à la vraie zone (actuel : Paris 6/8/14/15/16/17 + Neuilly, Levallois, Boulogne, Issy, Courbevoie, Puteaux, Clamart, Suresnes, Rueil).

---

## 🚀 Campagne Google Ads (rappel du cadrage)
- Type **Recherche (Search)**, objectif **Prospects**. ⛔ PAS Performance Max / Smart.
- Ciblage géo **serré** (communes réelles), option « présence dans la zone ».
- Budget ~**20-25 €/jour**, juger après **3-4 semaines**.
- Mots-clés intention vendeur (exact/expression) : « estimation appartement [ville] », « faire estimer ma maison », « combien vaut mon appartement [quartier] »…
- Négatifs : « en ligne », « gratuite immédiate », « simulateur », « notaire », « location », « DVF », « plus-value », « emploi », marques concurrentes.
- Levier clé : **rappeler vite** (promesse « réponse sous 24 h » affichée sur la page).

---

## 🗂️ Fichiers du projet (générés)
- `index.html` — la landing (version formulaire, EN LIGNE).
- `1-table-estimation-leads.sql` — création de la table Supabase.
- `2-fonction-estimation-lead.ts` — fonction Edge (insertion + email Mailjet).
- `CONTEXT.md` — ce fichier.

*(Une version « chat » de la page existe aussi hors repo si besoin de switcher un jour.)*
