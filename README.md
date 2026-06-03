# 🎬 Home of Clips

A production-ready web application for browsing curated movie dialogue and slow-motion clip collections, backed by Google Drive folders.

**Tech Stack:** React 18 · Vite · TypeScript · Tailwind CSS · Supabase · Netlify

---

## ✨ Features

| Feature | Details |
|---|---|
| 🔍 Public search | Fast debounced search by movie title |
| 🌗 Dark / Light mode | Persisted in localStorage |
| 🎞 Movie pages | Poster, metadata, two Google Drive buttons |
| 🔐 Admin auth | Supabase Auth with approved-email whitelist |
| 🗂 Admin dashboard | Add / Edit / Delete movies in a responsive table |
| 📱 Mobile-first | Responsive across all screen sizes |
| ⚡ Netlify-ready | `netlify.toml` SPA redirects included |

---

## 🚀 Quick Start

### 1. Clone & install

```bash
git clone <your-repo-url>
cd home-of-clips
npm install
```

### 2. Set up Supabase

1. Create a project at [supabase.com](https://supabase.com)
2. Go to **SQL Editor → New Query**
3. Paste and run the contents of `supabase/migrations/001_initial_schema.sql`
4. Add your admin email to `admin_users`:
   ```sql
   insert into public.admin_users (email) values ('your@email.com');
   ```
5. Create the corresponding auth account: **Authentication → Users → Invite user**

### 3. Configure environment variables

```bash
cp .env.example .env
```

Fill in `.env`:
```env
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

Get these from: **Supabase Dashboard → Settings → API**

### 4. Run locally

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173)

---

## 🌐 Deploy to Netlify

### Option A — Netlify UI (recommended)

1. Push your repo to GitHub / GitLab
2. Go to [app.netlify.com](https://app.netlify.com) → **Add new site → Import from Git**
3. Build settings:
   - **Build command:** `npm run build`
   - **Publish directory:** `dist`
4. Under **Site settings → Environment variables**, add:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
5. Deploy — the `netlify.toml` handles SPA routing automatically

### Option B — Netlify CLI

```bash
npm install -g netlify-cli
netlify login
netlify init
netlify env:set VITE_SUPABASE_URL "https://..."
netlify env:set VITE_SUPABASE_ANON_KEY "..."
netlify deploy --prod
```

---

## 📁 Project Structure

```
home-of-clips/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   ├── admin/
│   │   │   ├── MovieForm.tsx        # Add / edit form
│   │   │   └── ProtectedRoute.tsx   # Auth guard
│   │   └── ui/
│   │       ├── Button.tsx
│   │       ├── Modal.tsx
│   │       ├── MovieCard.tsx
│   │       ├── Navbar.tsx
│   │       ├── SearchBar.tsx
│   │       └── Toast.tsx
│   ├── contexts/
│   │   ├── AuthContext.tsx           # Supabase auth state
│   │   └── ThemeContext.tsx          # Dark/light mode
│   ├── hooks/
│   │   ├── useDebounce.ts
│   │   └── useMovies.ts             # CRUD operations
│   ├── lib/
│   │   └── supabase.ts              # Supabase client
│   ├── pages/
│   │   ├── admin/
│   │   │   ├── DashboardPage.tsx
│   │   │   └── LoginPage.tsx
│   │   ├── public/
│   │   │   ├── HomePage.tsx
│   │   │   └── MoviePage.tsx
│   │   └── NotFoundPage.tsx
│   ├── types/
│   │   └── index.ts
│   ├── App.tsx
│   ├── index.css
│   └── main.tsx
├── supabase/
│   └── migrations/
│       └── 001_initial_schema.sql
├── .env.example
├── netlify.toml
├── tailwind.config.js
├── tsconfig.json
└── vite.config.ts
```

---

## 🗄 Database Schema

### `movies`
| Column | Type | Description |
|---|---|---|
| `id` | uuid (PK) | Auto-generated |
| `title` | text | Movie title |
| `poster_url` | text | Direct image URL |
| `release_year` | integer | e.g. `2010` |
| `language` | text | e.g. `English` |
| `dialogues_folder_url` | text | Google Drive folder link |
| `slowmo_folder_url` | text | Google Drive folder link |
| `created_at` | timestamptz | Auto-set |

### `admin_users`
| Column | Type | Description |
|---|---|---|
| `id` | uuid (PK) | Auto-generated |
| `email` | text (unique) | Approved admin email |
| `created_at` | timestamptz | Auto-set |

### RLS Policies
- **movies** → SELECT: public. INSERT/UPDATE/DELETE: approved admins only.
- **admin_users** → SELECT: authenticated users (to check their own admin status).
- A `is_admin()` SQL function powers the admin check.

---

## 🔑 Admin Workflow

1. Add the admin email to `admin_users` table via SQL
2. Create the Supabase Auth account (invite or sign-up)
3. Visit `/admin/login` and sign in
4. Manage movies at `/admin`

> Only emails pre-approved in `admin_users` can access the dashboard — signing up without being whitelisted is rejected.

---

## 🎨 Design System

- **Display font:** Bebas Neue (cinematic headlines)
- **Body font:** DM Sans (clean, readable)
- **Mono font:** JetBrains Mono (badges, labels)
- **Accent color:** Brand orange (`#f97316`)
- **Dark palette:** Cinema blacks (`#080808` → `#3d3d3d`)

---

## 📜 License

MIT — free to use and modify.
