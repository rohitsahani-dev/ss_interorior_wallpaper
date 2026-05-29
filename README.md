# SS_Interiors & WallPapers

Production-ready full-stack wallpaper eCommerce and wallpaper installation service website for **SS_Interiors & WallPapers** in Biratnagar, Nepal.

## Features

- Next.js 15 App Router, TypeScript, Tailwind CSS, shadcn-style UI components
- Prisma ORM with PostgreSQL for Neon or Supabase
- Auth.js credentials authentication with bcrypt-hashed admin passwords
- Role-protected admin dashboard with middleware protection
- Wallpaper CRUD with Cloudinary image upload validation
- Interior room preview images for every wallpaper
- Live NPR pricing with installation cost at **NPR 600 per roll**
- Cart, wishlist, recently viewed wallpapers, related wallpapers, COD checkout
- Orders restricted to Biratnagar only
- Contact form, WhatsApp floating button, Google Maps embed and directions
- Secure API routes with Zod validation, origin checks, rate limiting, sanitized input, and Prisma queries
- SEO metadata, OpenGraph, sitemap, robots.txt, and LocalBusiness/Product structured data
- Dark mode, loading states, empty states, and error boundaries

## Tech Stack

- Next.js 15, React 19, TypeScript
- Tailwind CSS, Radix UI primitives, lucide-react, Framer Motion
- Prisma, PostgreSQL
- NextAuth/Auth.js, bcryptjs
- Cloudinary
- Zustand
- Zod, React Hook Form
- Vercel

## Environment Setup

Copy `.env.example` to `.env` and fill production values:

```bash
cp .env.example .env
```

Required values:

- `DATABASE_URL`: PostgreSQL connection string from Neon or Supabase
- `AUTH_SECRET`: strong random secret, at least 32 bytes
- `NEXTAUTH_URL`: local or production URL
- `ADMIN_EMAIL`, `ADMIN_PASSWORD`, `ADMIN_NAME`: seeded admin account
- `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`: upload credentials
- `NEXT_PUBLIC_SITE_URL`: deployed domain
- `NEXT_PUBLIC_BUSINESS_PHONE`, `NEXT_PUBLIC_WHATSAPP_NUMBER`, `NEXT_PUBLIC_BUSINESS_EMAIL`
- `NEXT_PUBLIC_GOOGLE_MAPS_EMBED_URL`, `NEXT_PUBLIC_GOOGLE_MAPS_DIRECTIONS_URL`: set the exact shop location before launch

## Local Development

```bash
npm install
npm run prisma:generate
npm run prisma:migrate
npm run db:seed
npm run dev
```

Open `http://localhost:3000`.

Admin login:

- URL: `/admin/login`
- Email/password: values from `ADMIN_EMAIL` and `ADMIN_PASSWORD`

## Database

Prisma models:

- `User`: admin/customer account data with role
- `Wallpaper`: product details, stock, category, featured flag, view count
- `WallpaperImage`: main, gallery, and interior preview images
- `Order`: COD customer order with Biratnagar delivery data and status
- `OrderItem`: quantity, price snapshot, installation selection, line totals
- `ContactMessage`: saved contact form leads

Run migrations:

```bash
npm run prisma:migrate
```

Seed demo wallpapers and admin:

```bash
npm run db:seed
```

## Security Notes

Implemented safeguards:

- Auth.js session handling and CSRF support for auth flows
- bcrypt password hashing
- Admin middleware route protection
- Server-side role checks for admin APIs
- Zod validation on all write routes
- Input sanitization for stored text
- Prisma parameterized queries for SQL injection prevention
- Same-origin checks on write APIs
- Rate limiting with Upstash Redis when configured, in-memory fallback for development
- Secure headers and CSP in `next.config.ts`
- Cloudinary file type and file size validation
- No secrets committed; all credentials are environment variables

For production, configure Upstash Redis values to make rate limiting durable across Vercel serverless instances.

## Deployment on Vercel

1. Push the repository to GitHub.
2. Import it into Vercel.
3. Add all environment variables from `.env.example`.
4. Set `NEXT_PUBLIC_SITE_URL` and `NEXTAUTH_URL` to the Vercel domain or custom domain.
5. Run the initial migration against the production database:

```bash
npm run prisma:generate
npm run prisma:migrate
npm run db:seed
```

6. Deploy.

Before launch, replace placeholder phone/email values and configure the exact Google Maps embed/directions URLs for the shop.

## Useful Commands

```bash
npm run dev
npm run build
npm run lint
npm run typecheck
npm run prisma:studio
```

## Business Logic

Installation is calculated per wallpaper roll:

```text
Wallpaper subtotal = wallpaper price × quantity
Installation subtotal = 600 NPR × quantity when installation is selected
Grand total = wallpaper subtotal + installation subtotal
```

The checkout API recalculates totals from database prices and stock, so client-side totals cannot be tampered with.
"# SS_Interiors-WallPapers" 
