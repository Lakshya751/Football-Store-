Norwich University Football Store - Class project for the Alumnis 

— Next.js 14 + Stripe + Prisma

A production-style demo storefront for Norwich University Football built with Next.js 14 (App Router), TypeScript, Tailwind CSS, Prisma, and Stripe (test mode).
It uses authentic Norwich imagery (Sabine Field / athletics photos) via whitelisted remote hosts.

️This is an educational demo, not an official store.

✨ Features
Next.js App Router with server components & API route handlers
Products in SQLite (local) or Postgres (production), via Prisma
Cart with increment/decrement, remove, totals (localStorage)
Checkout (demo) creates a Stripe PaymentIntent and simulates confirmation
Order confirmation page with stored line items
Responsive UI with NU brand colors (Maroon #8B1538, Gold #FFB81C)
Real NU visuals via next/image remote patterns
Accessibility: keyboard-operable hero dots, focus states, reduced-motion friendly

Stack
Frontend: Next.js 14 (App Router), TypeScript, Tailwind CSS
Backend: Next.js route handlers (/app/api/*)
Database: SQLite (local) via Prisma; switchable to Postgres for production
Payments: Stripe (test mode)
Images: next/image with remotePatterns (Norwich hosts whitelisted)


🚀 Quick Start (Local)
Prereqs: Node 18.17+, npm 9+, Stripe test account

# 1) Install deps
npm install

# 2) Configure env
cp .env.example .env
# Open .env and paste your Stripe test secret key (sk_test_...)

# 3) Create DB and seed products
npx prisma migrate dev --name init
npm run seed

# 4) Run dev server
npm run dev
# Open http://localhost:3000
Smoke test (1 minute)
Home page loads with hero slideshow
/shop lists products
Add items → /cart shows totals and quantity controls
/checkout → enter any email → Pay (Stripe test)
You should land on /order/[id] with an order summary (demo flow)


🌐 Deployment (Vercel)
SQLite is great locally, but on Vercel the filesystem is ephemeral. For persistent orders, switch to Postgres (Neon or Vercel Postgres).
A) Switch Prisma to Postgres (recommended)
Create a Postgres DB (Neon/Vercel Postgres) and copy the connection string
Edit prisma/schema.prisma:
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
Set env locally in .env:
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DB?sslmode=require"
Migrate & seed (to your hosted DB):
npx prisma migrate dev --name init
npm run seed

B) Import to Vercel
Push to GitHub → Vercel → Add New Project → Import
Add Environment Variables:
STRIPE_SECRET_KEY = your Stripe test secret (sk_test_…)
DATABASE_URL = your Postgres connection string
NEXT_PUBLIC_SITE_URL = https://your-app.vercel.app (update after first deploy if needed)
Deploy. Visit the live URL. /shop should render seeded products.
Run npm run seed locally again if you pointed to a fresh production DB and need initial products.


🧪 Stripe (Test Mode)
This demo creates a PaymentIntent and simulates confirmation to keep setup simple.
For real payments:
Add Stripe Elements on /checkout
Implement a webhook (/api/stripe/webhook) and mark orders paid only on payment_intent.succeeded


📁 Project Structure
app/
  page.tsx                # Home (Hero, Legacy)
  shop/page.tsx           # Product grid (server component)
  cart/page.tsx           # Client cart (localStorage)
  checkout/page.tsx       # Demo checkout
  order/[id]/page.tsx     # Confirmation page
  api/
    checkout/route.ts     # Create PaymentIntent
    orders/confirm/route.ts# Demo confirmation -> persist order

components/
  Hero.tsx
  ProductCard.tsx

lib/
  prisma.ts
  stripe.ts
  types.ts
  useCart.ts

prisma/
  schema.prisma
  seed.ts

public/
  favicon.ico

tailwind.config.ts
next.config.mjs


⚙️ Environment
Copy .env.example → .env:
STRIPE_SECRET_KEY=sk_test_your_key_here
DATABASE_URL=file:./dev.db
NEXT_PUBLIC_SITE_URL=http://localhost:3000


🧰 Useful Commands
npm run dev          # start Next.js locally
npm run build        # production build
npm start            # run production build
npx prisma studio    # DB browser at http://localhost:5555
npm run seed         # seed products


🐛 Troubleshooting
macOS paths with spaces / iCloud Desktop
Use quotes or backslashes, e.g.
cd "$HOME/Library/Mobile Documents/com~apple~CloudDocs/Desktop/Lakshya projects/nu-football-nextjs"
Accidentally initialized Git in a parent folder
If git status shows tons of unrelated files, you initialized at ~ or Desktop.
Remove the parent .git and re-init inside the project:
cd ~
rm -rf .git           # careful: only do this if your home dir shouldn't be a repo
cd "<path to>/nu-football-nextjs"
git init
printf "node_modules\n.DS_Store\n.env\n*.log\n*.icloud\n" >> .gitignore
git add . && git commit -m "Initial commit"
Stripe key missing/invalid
Ensure .env has a valid sk_test_... and restart npm run dev.
Images not loading
If you swap to different hosts, add them to next.config.mjs → images.remotePatterns.


📚 Attribution & Sources
Norwich University: main site & Sabine Field visuals
Norwich Athletics: football hub & photo galleries
Brand Guidelines (PDF) linked in footer
Replace or host images locally if usage rights change.


✅ License
For educational/demo use. Not affiliated with Norwich University.
It's
a prototype
