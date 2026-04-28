# Cyberpunk Artist Portfolio

A premium dark cyberpunk React + Vite static portfolio with a 3D animated background, neon/glow accents, glassmorphism, smooth animations, and a built-in Decap CMS admin panel for editing all content from your browser. Designed to be deployed on **Netlify** in under 5 minutes.

## Stack

- React 19 + Vite 7 + TypeScript
- Tailwind CSS v4 with a custom cyberpunk theme (CMS-editable colors)
- react-three-fiber + drei (with a CSS fallback when WebGL is unavailable)
- framer-motion for animations
- wouter for client routing
- Decap CMS for git-backed content editing at `/admin/`

## Quick start (local)

```bash
npm install
npm run dev      # http://localhost:5173
npm run build    # outputs dist/
npm run serve    # preview the production build
```

## Deploying to Netlify

The cleanest path — and the only one where the **admin / CMS works** — is a Git-connected deploy. Netlify Drop only publishes static files and cannot run the CMS commits back to a repo.

### Step 1 — Push to GitHub

1. Create an empty public (or private) repo on GitHub. **Do not** initialize it with a README.
2. From inside this folder:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```

### Step 2 — Create a Netlify site from the repo

1. Go to https://app.netlify.com → **Add new site** → **Import an existing project** → **Deploy with GitHub**.
2. Authorize Netlify to access your repo and pick it.
3. Build settings auto-fill from `netlify.toml`:
   - Build command: `npm install && npm run build`
   - Publish directory: `dist`
4. Click **Deploy site**. First build takes ~2 minutes.

### Step 3 — Enable Identity AND Git Gateway (both required)

In your Netlify site dashboard:

1. **Site configuration → Identity → Enable Identity**
2. Still on the Identity page, scroll to **Registration preferences → Invite only** (recommended).
3. Scroll to **Services → Git Gateway → Enable Git Gateway**. *(This is the step that lets the CMS write commits back to your repo. Without it, login works but saving fails.)*
4. Go to **Identity → Invite users** and invite your email.
5. Open the invitation email on the same browser. It will land you on `https://your-site.netlify.app/#invite_token=...` — the Identity widget pops up and asks you to set a password.
6. After setting your password you'll be auto-redirected to `/admin/` and signed in.

> **Important:** The site's `index.html` includes the Netlify Identity widget script — that's what makes the invite/login pop-up appear. If you ever rebuild the project from scratch, keep the `<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>` tag and the small handler that redirects to `/admin/` after login.

## Editing content

Once logged in at `/admin/`, you can edit:

- **Site Settings** — site title, logo, theme colors (primary, secondary, accent), social URLs
- **About page** — bio, profile image, skills, timeline
- **Contact page** — heading, subheading, email, social handles (Discord, Telegram, Fiverr, etc.)
- **11 Portfolio Categories** — title, description, cover image, slug, order
- **Portfolio Items** — title, category, cover image, gallery (image / video / YouTube / Vimeo), description, featured/visible flags, order

Every save commits to your GitHub repo. Netlify rebuilds the site automatically — usually live in ~30 seconds.

## Project structure

```
.
├── content/                # All editable content (CMS commits here)
│   ├── settings/           # site.json — global settings & theme
│   ├── pages/              # about.md, contact.md
│   ├── categories/         # one .md per portfolio category
│   ├── portfolio/          # one .md per portfolio item
│   ├── skills/             # skill chips for About page
│   └── timeline/           # timeline entries for About page
├── public/
│   ├── admin/              # Decap CMS UI (config.yml + index.html)
│   ├── uploads/            # CMS-uploaded images
│   ├── favicon.svg
│   └── opengraph.jpg
├── src/
│   ├── App.tsx             # Routes
│   ├── main.tsx            # App entry
│   ├── index.css           # Cyberpunk theme + global styles
│   ├── pages/              # Home, Portfolio, Category, Item, About, Contact, NotFound
│   ├── components/         # Layout, ThreeBackground, Gallery, etc.
│   ├── lib/                # content.ts (markdown loader), types.ts
│   └── hooks/
├── netlify.toml            # Netlify build & redirect config
├── vite.config.ts
├── tsconfig.json
└── package.json
```

## Theming

Theme colors live in `content/settings/site.json` under `theme.{primary, secondary, accent}` and are applied as CSS custom properties at runtime. Change them from the CMS — no redeploy of styles needed.

## License

Use freely for personal or commercial portfolios.
