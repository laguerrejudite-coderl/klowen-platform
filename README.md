# Klowen-Bot Platform — Module Admin Panel

Ce module contient le **panneau d'administration** de la future plateforme d'hébergement Klowen-Bot.

## Contenu

### Backend (`/backend`)
- `models/User.js` — utilisateurs (rôle, statut actif/suspendu)
- `models/Bot.js` — instances de bots déployées (statut, ressources CPU/RAM/disque)
- `models/Announcement.js` — annonces diffusées aux utilisateurs
- `middleware/auth.js` + `middleware/adminOnly.js` — authentification JWT + contrôle du rôle admin
- `routes/admin.js` — toutes les routes du panneau admin :
  - `GET /api/admin/users` — liste des utilisateurs
  - `PATCH /api/admin/users/:id/suspend` / `/reactivate`
  - `DELETE /api/admin/users/:id`
  - `GET /api/admin/bots` — liste de tous les bots + ressources
  - `GET /api/admin/stats` — statistiques globales
  - `POST /api/admin/announcements` — diffuse une annonce (Socket.io temps réel)
  - `GET /api/admin/logs` et `/api/admin/logs/errors`

### Frontend (`/frontend`, Next.js + TypeScript + Tailwind)
- `app/admin/page.tsx` — vue d'ensemble (statistiques)
- `app/admin/users/page.tsx` — gestion des utilisateurs
- `app/admin/bots/page.tsx` — supervision des bots
- `app/admin/announcements/page.tsx` — création et diffusion d'annonces
- `app/admin/logs/page.tsx` — consultation des journaux/erreurs

## Installation

```bash
# Backend
cd backend
npm install
cp .env.example .env   # renseigner MONGODB_URI et JWT_SECRET
npm start

# Frontend
cd ../frontend
npm install
echo "NEXT_PUBLIC_API_URL=http://localhost:4000" > .env.local
npm run dev
```

Le panneau est ensuite accessible sur `http://localhost:3000/admin` (une fois la partie authentification connectée — voir prochaine étape).

## Sécurité déjà en place
- JWT via cookie httpOnly
- Vérification du rôle `admin` sur toutes les routes
- Helmet + rate limiting sur l'API
- Comptes suspendus bloqués à l'authentification

## Prochaines étapes
Ce module ne couvre que l'**admin panel**. Restent à construire : inscription/connexion, dashboard utilisateur, déploiement Docker par utilisateur, gestionnaire de fichiers, landing page.
