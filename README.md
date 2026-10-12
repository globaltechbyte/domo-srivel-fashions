# Srivel Fashion House — Women's Fashion E-commerce Demo

A client-presentation-ready, fully interactive boutique storefront (React + Vite + TypeScript + Tailwind v4 + Motion + Zustand + React Router).
No backend, database, payment gateway or paid API. Everything persists in the browser's localStorage.

## Run it

```bash
npm install
npm run dev        # http://localhost:5173
npm run build      # type-check + production build into /dist
npm run preview    # serve the production build locally
npm run lint       # oxlint
npm run typecheck  # tsc -b
npm test           # vitest (tracking, URL safety, lookup, masking, order store, gallery privacy/consent, media rules)
```

## Deploy to Vercel

1. Push the folder to GitHub (or run `npx vercel` in this directory).
2. Framework preset: **Vite**. Build command `npm run build`, output directory `dist`.
3. `vercel.json` (included) rewrites all routes to `index.html` so deep links such as `/product/...` and `/admin-demo` work.

## Where to edit things

| What | Where |
| --- | --- |
| Brand name, email, city, Instagram handle, WhatsApp number, shipping rules, announcement text | `src/config.ts` |
| Wordmark text | `Wordmark` in `src/components/ui/primitives.tsx` and `<title>` in `index.html` |
| Colours, fonts, spacing tokens | `@theme` block in `src/styles/index.css` |
| Products, categories, collections, sizes | `src/data/products.ts` |
| Homepage copy, testimonials, static pages (About, Shipping…) | `src/data/content.ts` |
| Sample admin orders | `src/data/orders.ts` |
| Instagram profile URL and handle | `BRAND.instagramUrl` / `instagramHandle` in `src/config.ts` |
| Demo gallery seed items (all flagged `isDemo`) | `src/data/social.ts` |
| Upload size limits | `MEDIA_LIMITS` in `src/lib/media.ts` |

**WhatsApp:** `WHATSAPP_NUMBER` is the placeholder `910000000000`. Replace with your real number (country code, digits only).

**Photography:** the demo ships with generated, on-brand illustrations (`art:` image tokens) so it never shows a broken image and carries no
licensing risk. To use real photos, put them in `public/products/` and list the paths/URLs in a product's `images` array. If a photo URL fails to load,
the card falls back to an illustration automatically. Admin → Edit product also has a "Photo URL" field.

## Implemented

- Cinematic home (hero, categories, new arrivals, featured collection, bestsellers carousel, brand story, sample testimonials, Instagram grid, visual-only newsletter, footer) and branded first-load intro
- Catalogue with URL-driven filters (category, price range, size, colour, search), sorting, load-more, skeletons, empty state, mobile filter drawer
- Product page: gallery (swipe on mobile, hover/click zoom on desktop), colour/size selection (size required), size guide, stock states, quantity, Add to Cart, Buy via WhatsApp, sticky mobile buy bar, related products
- Cart drawer + cart page with correct totals, shipping rule, persistence, animated badge
- Checkout with Indian mobile/PIN validation; WhatsApp pre-filled message or simulated COD; demo order confirmation; orders saved locally
- `/admin-demo`: dashboard, product CRUD with per-size/colour stock, orders with status and COD payment updates, reset demo data
- **Order tracking** (`/track-order`): order ID + registered mobile lookup against the demo order store, animated vertical timeline
  (Pending Confirmation → Confirmed → Packed → Shipped → Delivered, Cancelled shown separately), items, payment, courier info and a safe external
  courier link. Personal data is masked on this page. A clearly marked **demo test order** (`AUR-DEMO-TEST01`, mobile `9000000000`) is included.
  Linked from the header, footer, mobile menu and order confirmation page.
- **Admin order workflow**: one-step-at-a-time transitions (cancel allowed only before shipping, Delivered/Cancelled are final), courier name /
  tracking number / tracking URL, and a timestamped status history. Admin and tracking read the same store; changes appear in other tabs of the same browser.
- **Styled on Instagram** (homepage): editorial grid with a feature tile, hover zoom, play icons on videos/reels, lazy-loaded media, in-page preview for uploads
  and a safe "Follow Us on Instagram" link. Instagram post/reel URLs are validated and normalised; cards link to the original content.
- **Loved by Our Customers** (homepage): masonry delivery-photo gallery with first name/initials, city, product, optional customer-written feedback and a fullscreen
  lightbox (arrow keys, swipe, focus trap). Customer photos appear publicly **only with recorded consent and a completed privacy check**; placeholders are labelled "Demo example".
- **Gallery admin** (`/admin-demo/instagram`, `/admin-demo/customers`): add/edit/delete, Instagram URL, photo/video upload, thumbnail, caption, display order (with up/down),
  active/visible switches, consent and privacy confirmations, text scans that block phone numbers, PIN codes, emails, links and tracking codes.
- Motion throughout, honouring `prefers-reduced-motion`; wishlist; search overlay; toasts; error boundary and 404

## Demo limitations

- Orders, cart, wishlist and admin edits live only in the visitor's browser; nothing is sent to a server, so no business receives COD orders.
- WhatsApp orders only start a chat: the customer must press send, and nothing is confirmed automatically.
- The admin area has no authentication (by design, for presentation) and uses mock data.
- Placing an order does not decrement stock.
- **Gallery uploads are browser-local.** Metadata is in localStorage; uploaded photos/videos are in this browser's IndexedDB (images ≤ 5 MB, resized to 1600 px with
  metadata stripped; videos ≤ 25 MB). They do **not** sync across devices or browsers, visitors elsewhere won't see them, and clearing site data deletes them. For a real site use a CDN and image URLs.
- Instagram does not give this demo post previews, so a card for a pasted Instagram link needs a thumbnail (or uploaded media) from the admin. Nothing is fetched from Instagram.
- Only the first 9 active Instagram items and 12 published customer photos are shown on the homepage.
- Order tracking is simulated. There is **no courier API integration, no real-time tracking, no cross-device sync** and no real fulfilment: statuses change only when
  someone clicks in the demo admin, and orders exist only in the browser that created them. See [docs/PRODUCTION.md](docs/PRODUCTION.md) for what a real
  deployment needs (server database, secure order verification, rate limiting, shared state, admin auth).
- Testimonials, policies and contact details are labelled placeholders to be replaced.
- Product images are generated illustrations, not photographs.
