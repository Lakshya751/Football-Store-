# Norwich University Football Store (work still needs to be done on this)

> **Educational Demo Project** - A class project showcasing a modern e-commerce storefront for Norwich University Football alumni.

⚠️ **Disclaimer**: This is an educational demonstration and not an official Norwich University store.

---

## 🎯 Overview

A production-style demo storefront built with modern web technologies, featuring authentic Norwich University imagery and a complete shopping experience. This project demonstrates full-stack e-commerce capabilities including product management, cart functionality, and payment processing.

### Tech Stack

- **Frontend**: Next.js 14 (App Router), TypeScript, Tailwind CSS
- **Backend**: Next.js API Route Handlers
- **Database**: Prisma ORM with SQLite (local) / PostgreSQL (production)
- **Payments**: Stripe (test mode)
- **Images**: Next.js Image Optimization with remote patterns

---

## ✨ Features

- ✅ **Modern Architecture**: Next.js 14 App Router with server components
- ✅ **Database Management**: SQLite for local development, PostgreSQL-ready for production
- ✅ **Shopping Cart**: Full cart functionality with increment/decrement, removal, and real-time totals (localStorage)
- ✅ **Checkout Flow**: Demo checkout with Stripe PaymentIntent creation and confirmation simulation
- ✅ **Order Management**: Confirmation pages with stored line items
- ✅ **Responsive Design**: Mobile-first UI with Norwich University brand colors (Maroon #8B1538, Gold #FFB81C)
- ✅ **Optimized Images**: Real NU visuals via `next/image` with whitelisted remote patterns
- ✅ **Accessibility**: Keyboard-navigable hero carousel, focus states, and reduced-motion support

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18.17 or higher
- npm 9 or higher
- Stripe test account ([sign up here](https://stripe.com))

### Installation

1. **Clone and install dependencies**
   ```bash
   git clone <your-repo-url>
   cd nu-football-nextjs
   npm install
   ```

2. **Configure environment variables**
   ```bash
   cp .env.example .env
   ```
   
   Open `.env` and add your Stripe test secret key:
   ```env
   STRIPE_SECRET_KEY=sk_test_your_key_here
   DATABASE_URL=file:./dev.db
   NEXT_PUBLIC_SITE_URL=http://localhost:3000
   ```

3. **Initialize database and seed products**
   ```bash
   npx prisma migrate dev --name init
   npm run seed
   ```

4. **Start the development server**
   ```bash
   npm run dev
   ```
   
   Open [http://localhost:3000](http://localhost:3000) in your browser.

### Quick Smoke Test (1 minute)

1. ✅ Home page loads with hero slideshow
2. ✅ Navigate to `/shop` to view products
3. ✅ Add items to cart → `/cart` displays totals and quantity controls
4. ✅ Go to `/checkout` → enter any email → click "Pay" (Stripe test mode)
5. ✅ Confirm you land on `/order/[id]` with order summary

---

## 🌐 Production Deployment (Vercel)

> **Note**: SQLite works locally but Vercel's filesystem is ephemeral. Switch to PostgreSQL for persistent data.

### Step 1: Set Up PostgreSQL

**Option A: Neon** (Recommended)
1. Create a free database at [neon.tech](https://neon.tech)
2. Copy the connection string

**Option B: Vercel Postgres**
1. Enable Postgres in your Vercel project settings
2. Copy the provided connection string

### Step 2: Update Prisma Schema

Edit `prisma/schema.prisma`:
```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

### Step 3: Migrate and Seed (Local → Production DB)

```bash
# Update your .env with production DATABASE_URL
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DB?sslmode=require"

# Run migration
npx prisma migrate dev --name init

# Seed products
npm run seed
```

### Step 4: Deploy to Vercel

1. Push your code to GitHub
2. Import project in Vercel dashboard
3. Add environment variables:
   - `STRIPE_SECRET_KEY` = `sk_test_...`
   - `DATABASE_URL` = Your PostgreSQL connection string
   - `NEXT_PUBLIC_SITE_URL` = `https://your-app.vercel.app`
4. Deploy!

---

## 📁 Project Structure

```
nu-football-nextjs/
├── app/
│   ├── page.tsx                    # Home page (Hero, Legacy section)
│   ├── shop/
│   │   └── page.tsx               # Product grid (server component)
│   ├── cart/
│   │   └── page.tsx               # Shopping cart (client component)
│   ├── checkout/
│   │   └── page.tsx               # Demo checkout flow
│   ├── order/
│   │   └── [id]/page.tsx          # Order confirmation
│   └── api/
│       ├── checkout/
│       │   └── route.ts           # Create Stripe PaymentIntent
│       └── orders/
│           └── confirm/route.ts   # Persist order to database
├── components/
│   ├── Hero.tsx                   # Hero carousel component
│   └── ProductCard.tsx            # Product display card
├── lib/
│   ├── prisma.ts                  # Prisma client singleton
│   ├── stripe.ts                  # Stripe client configuration
│   ├── types.ts                   # TypeScript type definitions
│   └── useCart.ts                 # Cart state management hook
├── prisma/
│   ├── schema.prisma              # Database schema
│   └── seed.ts                    # Database seeding script
├── public/                        # Static assets
├── .env.example                   # Environment variable template
├── next.config.mjs                # Next.js configuration
├── tailwind.config.ts             # Tailwind CSS configuration
└── package.json
```

---

## 🧰 Useful Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Create production build |
| `npm start` | Run production build |
| `npx prisma studio` | Open database browser at http://localhost:5555 |
| `npm run seed` | Seed products into database |
| `npx prisma migrate dev` | Create and run database migrations |

---

## 🧪 Stripe Integration (Test Mode)

This demo currently creates a PaymentIntent and simulates confirmation for simplicity.

### For Production Implementation:

1. **Add Stripe Elements** to `/checkout` for secure card input
2. **Implement webhook handler** at `/api/stripe/webhook`
3. **Verify payments** by listening for `payment_intent.succeeded` events
4. **Mark orders as paid** only after webhook confirmation

### Test Cards

Use these cards in test mode:
- **Success**: `4242 4242 4242 4242`
- **Decline**: `4000 0000 0000 0002`
- Use any future expiry date and any 3-digit CVC

---

## 🐛 Troubleshooting

### macOS Paths with Spaces / iCloud Desktop

If your project is in iCloud or has spaces in the path, use quotes:
```bash
cd "$HOME/Library/Mobile Documents/com~apple~CloudDocs/Desktop/Lakshya projects/nu-football-nextjs"
```

### Accidentally Initialized Git in Parent Folder

If `git status` shows unrelated files, you initialized in the wrong directory:
```bash
# Navigate to home directory
cd ~

# Remove the incorrect .git folder (careful!)
rm -rf .git

# Navigate to your project
cd "nu-football-nextjs"

# Re-initialize Git properly
git init
echo "node_modules
.DS_Store
.env
*.log
*.icloud" > .gitignore
git add .
git commit -m "Initial commit"
```

### Stripe Key Missing or Invalid

- Ensure `.env` contains a valid `sk_test_...` key
- Restart your development server: `npm run dev`
- Check for extra spaces or quotes in the `.env` file

### Images Not Loading

If you change image hosts, update `next.config.mjs`:
```javascript
images: {
  remotePatterns: [
    {
      protocol: 'https',
      hostname: 'your-new-host.com',
    },
  ],
}
```

### Database Issues

**Reset database:**
```bash
rm prisma/dev.db
npx prisma migrate dev --name init
npm run seed
```

---

## 📚 Attribution & Resources

This project uses authentic Norwich University imagery and branding:

- [Norwich University Official Site](https://www.norwich.edu)
- [Norwich Athletics - Football](https://norwichu.edu/sports/football)
- Sabine Field visuals and athletic photography
- Brand guidelines referenced in footer

**Image Rights**: If usage rights change, images should be replaced or hosted locally.

---

## 📄 License

This project is for **educational and demonstration purposes only**.

- Not affiliated with or endorsed by Norwich University
- Created as a class project prototype
- Stripe integration uses test mode only

---

## 🤝 Contributing

This is an educational project. If you're a student working on this:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📧 Contact

For questions about this project, please contact your course instructor or project maintainer.

---

**Built with ❤️ for Norwich University Alumni**
