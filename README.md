# Canada Immigration Accompagnement

Plateforme professionnelle d’accompagnement en immigration canadienne.

**Design premium • Sécurité RLS • Multi-étapes • Espace client + Admin**

---

## État actuel du projet

### Déjà réalisé ✅

| Élément | Statut |
|---------|--------|
| Architecture Next.js 15 + TypeScript + Tailwind | ✅ |
| Schéma PostgreSQL complet + RLS strict | ✅ |
| Types TypeScript + labels de statuts | ✅ |
| Clients Supabase (browser + server) | ✅ |
| Composants UI (Button, Input, Select, Textarea, PasswordStrength) | ✅ |
| Header & Footer premium responsive | ✅ |
| Page d’accueil complète (hero, programmes, étapes, FAQ, CTA) | ✅ |
| **Inscription** (10 champs + solidité MDP + CGU + validation) | ✅ |
| **Connexion** (redirection admin / client) | ✅ |
| **Choix du programme** (7 catégories avec critères) | ✅ |
| **Formulaire multi-étapes A→E** + barre de progression + brouillon + récapitulatif + page de confirmation verte | ✅ |
| **Espace client** (dossiers, notifications, actions rapides, déconnexion) | ✅ |
| **Tableau de bord admin** (stats, liste filtrable, recherche) | ✅ |
| Variables d’environnement d’exemple | ✅ |

### À compléter

1. Pages détail dossier (client + admin) avec changement de statut + motif de rejet
2. Système de messagerie client ↔ admin
3. Upload sécurisé de documents (Supabase Storage)
4. Middleware de protection des routes
5. Pages légales (mentions, confidentialité, CGU)
6. Pré-remplissage du profil dans le formulaire de candidature
7. Tests d’accès et de permissions
8. Génération du numéro de dossier via fonction SQL côté serveur

---

## Stack technique

- **Frontend** : Next.js 15 (App Router) + React 19 + TypeScript
- **Style** : Tailwind CSS v4 + design system rouge canadien `#c8102e` / bleu nuit `#0a2540`
- **Backend / Auth / DB** : Supabase (PostgreSQL + Auth + Storage + RLS)
- **Icônes** : Lucide React
- **Hébergement recommandé** : Vercel + Supabase

---

## Installation locale

```bash
cd immigration-canada

npm install
npm install @supabase/supabase-js @supabase/ssr lucide-react clsx tailwind-merge date-fns zod react-hook-form @hookform/resolvers --legacy-peer-deps

cp .env.example .env.local
# → Renseigner NEXT_PUBLIC_SUPABASE_URL et NEXT_PUBLIC_SUPABASE_ANON_KEY

npm run dev
```

Ouvrez http://localhost:3000

---

## Configuration Supabase (obligatoire)

1. Créez un projet sur https://supabase.com
2. Dans **SQL Editor**, exécutez le fichier  
   `supabase/migrations/001_initial_schema.sql`
3. Activez **Email** dans Authentication → Providers
4. Créez un bucket **privé** nommé `documents` dans Storage
5. Copiez l’URL et la clé `anon` dans `.env.local`

### Création du compte administrateur @Jenpaul2

```sql
-- Après inscription normale avec votre email :
UPDATE profiles
SET role = 'admin'
WHERE email = 'votre-email-admin@exemple.com';
```

Activez idéalement la MFA sur ce compte.

---

## Flux utilisateur

1. **Accueil** → Commencer ma demande
2. **Inscription** → Création compte + profil
3. **Choix du programme** (permanente, travail, études, visite, familial, affaires, autre)
4. **Formulaire multi-étapes** :
   - A : Informations personnelles
   - B : Individuel ou familial (1 à 13 personnes)
   - C : Profil (études, langues, budget…)
   - D : Description libre du projet
   - E : Récapitulatif + soumission
5. **Confirmation** avec numéro de dossier `CA-YYYY-XXXXXX`
6. **Espace client** : suivi des dossiers + notifications
7. **Admin** : vue d’ensemble + gestion des dossiers

---

## Sécurité déjà en place

- Mots de passe hachés par Supabase Auth
- Row Level Security sur **toutes** les tables
- Un client ne voit que ses propres données
- Notes admin invisibles aux clients
- Journal d’audit prévu
- Service role key uniquement côté serveur
- Aucun secret dans le code source

---

## Structure des fichiers principaux

```
src/
├── app/
│   ├── page.tsx                    # Accueil
│   ├── auth/login/page.tsx
│   ├── auth/register/page.tsx
│   ├── client/
│   │   ├── page.tsx                # Dashboard client
│   │   ├── choix-programme/page.tsx
│   │   └── candidature/page.tsx    # Formulaire multi-étapes
│   └── admin/page.tsx              # Dashboard admin
├── components/
│   ├── layout/Header.tsx + Footer.tsx
│   └── ui/ (Button, Input, Select, Textarea, PasswordStrength)
├── lib/supabase/ + utils.ts
├── types/database.ts
supabase/migrations/001_initial_schema.sql
```

---

## Prochaines actions recommandées

1. Configurer Supabase et tester l’inscription + soumission d’un dossier.
2. Demander la page détail dossier admin (changement de statut + motif).
3. Implémenter la messagerie.
4. Ajouter le middleware de protection.

---

**Avertissement** : Plateforme privée d’accompagnement.  
Ce n’est **pas** un site officiel du gouvernement du Canada.  
Aucune garantie de résultat n’est possible.
