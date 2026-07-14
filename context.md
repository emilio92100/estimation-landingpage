# CONTEXT — Landing page « Estimation » Emilio Immobilier

> Fichier de suivi du projet. À garder à jour au fil des étapes.
> **Dernière mise à jour :** chaîne complète opérationnelle (landing + base + admin).

---

## 🎯 Objectif
Landing page dédiée à la **génération de leads vendeurs** (demandes d'estimation) pour **Emilio Immobilier**, destinée à recevoir du trafic **Google Ads**.
- Positionnement : agent **indépendant titulaire de la carte professionnelle** (loi Hoguet), estimation par un **expert humain**, secteur **Paris + Hauts-de-Seine**.
- Objectif = **qualité** des leads (pas volume).

---

## 🌐 Infra & URLs

| Élément | Valeur |
|---|---|
| **Landing (public)** | https://estimation.emilio-immo.com ✅ |
| **Admin (privé)** | https://estimation.emilio-immo.com/admin.html · mot de passe : `emilio2020` |
| **Repo GitHub** | `emilio92100/estimation-landingpage` (`index.html` + `admin.html` + `context.md`) |
| **Hébergement** | Vercel (projet `estimation-landingpage`), connecté à GitHub → **chaque commit redéploie automatiquement** |
| **Supabase** | Projet **`emilio-estimations`** (org Analymo, région eu-west-1) · Project ID : `hwiznahlbeisxkkskhfb` |
| **DNS** | Domaine chez OVH, sous-domaine `estimation` en CNAME vers Vercel ✅ |

⚠️ **Toujours lancer la pub vers `estimation.emilio-immo.com`**, jamais vers l'URL `*.vercel.app`.
Le site principal `emilio-immo.com` reste sur **Lovable (Lovable Cloud)** — projet séparé, non impacté. (Lovable Cloud = backend géré par Lovable, migration nécessaire si sortie un jour.)

### Comment mettre à jour
Éditer / ré-uploader le fichier sur GitHub → commit → Vercel redéploie tout seul (~1 min). Penser au rechargement forcé (Ctrl+Maj+R) si le cache s'accroche.

---

## 🔄 Architecture (comment ça marche)

```
Visiteur → estimation.emilio-immo.com (Vercel)
              ↓ formulaire envoyé
       Edge Function "estimation-lead" (Supabase)
              ↓
       Table estimation_leads (Supabase)
              ↑ lecture / update / delete
       Edge Function "admin-leads" (protégée par mot de passe)
              ↑
       admin.html → Alexandre
```

**Supabase — ce qui est en place :**
- Table `estimation_leads` (+ colonnes `status` et `notes`)
- Edge Function **`estimation-lead`** — reçoit le formulaire, insère le lead (Verify JWT OFF)
- Edge Function **`admin-leads`** — actions `list` / `update` / `delete`, protégée (Verify JWT OFF)
- Secret **`ADMIN_PASSWORD`** = `emilio2020`
- ❌ Pas d'email (Mailjet non configuré, choix assumé) → tout se consulte via l'admin

---

## 📄 La landing (version FORMULAIRE)
Deux versions avaient été construites (formulaire multi-étapes + chat conversationnel). **La version publiée est le FORMULAIRE.**

Sections : header (logo, tél, CTA) · hero (accroche + formulaire + bouton WhatsApp) · présentation « Votre interlocuteur » (photo, badges 10 ans + carte pro CPI/CCI, encadré loi Hoguet) · « Idées reçues / La réalité » (4 cartes) · méthode en 3 étapes · avis · secteurs · bandeau « 20 % d'écart » · footer.

**Tunnel du formulaire (6 étapes) :**
1. Type de bien
2. Adresse (**obligatoire**, autocomplétion Base Adresse Nationale)
3. Le bien (dimensions, situation RDC/étage, état)
4. Extérieur & annexes
5. Projet de vente (délai)
6. Contact + consentement

**Validations :** bouton grisé tant que l'étape est incomplète · prénom requis · téléphone **10 chiffres** · email **valide** · case cochée.
**À l'envoi :** loader en 3 étapes (~4 s) puis confirmation + récapitulatif illustré.

**Coordonnées intégrées :** mobile **06 58 95 76 32** · email **arogelet@emilio-immo.com** · carte pro **CPI 9201 2020 000 045 344** (délivrée par la CCI de Paris).

---

## 🖥️ L'admin (`admin.html`)
Tableau de bord : sidebar avec filtres par statut (compteurs), 4 cartes de stats, recherche, cartes de demande, et **panneau de détail** qui s'ouvre au clic.

- **Statuts :** Nouveau → À rappeler → Rappelé → RDV pris → Perdu
- **Ouverture par défaut sur l'onglet « Nouveau »**
- Détail : bouton **Appeler**, **Email**, **WhatsApp** (message pré-rempli), infos complètes du bien, changement de statut, **notes avec bouton Enregistrer**, suppression (définitive, avec confirmation)
- Conseil : préférer le statut **« Perdu »** à la suppression (un vendeur peut revenir dans 3-6 mois)

---

## ✅ FAIT
- [x] Landing conçue, itérée, mise en ligne
- [x] Sous-domaine `estimation.emilio-immo.com` + HTTPS
- [x] Supabase : table + fonction `estimation-lead`
- [x] **Formulaire branché → les leads arrivent en base** (testé ✅)
- [x] Autocomplétion d'adresse fonctionnelle
- [x] Panel admin complet + fonction `admin-leads` + mot de passe

## 🔴 À FAIRE avant de lancer la pub (bloquants)
- [ ] **Mentions légales + politique de confidentialité** — les liens du footer pointent vers des URL supposées (`emilio-immo.com/mentions-legales` et `/politique-de-confidentialite`). **À vérifier / créer.** Doivent contenir : N° CPI, garantie financière, assurance RCP. *Exigé par Google Ads pour la collecte de leads.*
- [ ] **Remplacer les avis** — Nathalie R. / Julien & Claire / Mehdi B. sont des **exemples inventés**. À remplacer par de vrais avis Google avant publicité.
- [ ] **Google Ads : régler le moyen de paiement** (compte bloqué)
- [ ] **Configurer le suivi de conversion** Google Ads (type « Site Web », déclenché à l'envoi du formulaire). Sans ça = dépense à l'aveugle (erreur de 2022).

## 🟡 À faire ensuite (pas bloquant)
- [ ] Ajuster la **liste des secteurs** à la vraie zone (actuel : Paris 6/8/14/15/16/17 + Neuilly, Levallois, Boulogne, Issy, Courbevoie, Puteaux, Clamart, Suresnes, Rueil)
- [ ] **Bandeau de chiffres/résultats** (ventes, délai moyen, % du prix) si chiffres réels disponibles — gros levier de conversion
- [ ] **Mini-FAQ** (honoraires, engagement, délai, pourquoi un expert)
- [ ] Optionnel : export CSV des leads · alerte email/notif · mot de passe admin plus fort

---

## 🚀 Campagne Google Ads (cadrage validé)
- Type **Recherche (Search)**, objectif **Prospects**. ⛔ PAS Performance Max / Smart (erreur de 2022 : campagne intelligente + aucun tracking = 185 € perdus).
- **Ciblage géo serré** sur les communes réelles, option « présence dans la zone ».
- **Budget** ~20-25 €/jour · juger après **3-4 semaines**.
- **Enchères** : « Maximiser les clics » avec plafond CPC au début → « Maximiser les conversions » une fois le tracking actif.
- **Mots-clés** (exact/expression) : « estimation appartement [ville] », « faire estimer ma maison [ville] », « combien vaut mon appartement [quartier] », « prix vente appartement [rue] »…
- **Négatifs** : en ligne · gratuite immédiate · simulateur · calcul · notaire · location/louer · DVF · plus-value · emploi/salaire · marques concurrentes.
- **Annonces** alignées avec la page + extensions (liens annexes, accroches, **extension d'appel**).
- **Levier n°1 : rappeler vite** (la page promet « réponse sous 24 h »).
- Repères : CPC réel attendu 3-8 € (vs 0,23 € en 2022 = signe d'un mauvais ciblage) · objectif CPL < 60-100 € · un mandat parisien ≫ budget mensuel.

---

## 🗂️ Fichiers du projet
- `index.html` — la landing (EN LIGNE)
- `admin.html` — le panel admin (EN LIGNE)
- `1-table-estimation-leads.sql` — création de la table *(exécuté ✅)*
- `2-fonction-estimation-lead.ts` — fonction de réception des leads *(déployée ✅)*
- `3-ajout-statut.sql` — colonnes status + notes *(exécuté ✅)*
- `4-fonction-admin-leads.ts` — fonction admin list/update/delete *(déployée ✅)*
- `CONTEXT.md` — ce fichier

*(Une version « chat » de la landing existe hors repo si besoin de switcher un jour.)*
