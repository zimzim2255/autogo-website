# AutoGo Website - File Structure & Developer Guide

## 🎯 Project Overview
Car Dealer Website (Vitrine) - Frontend only, built with Astro. No backend, no admin panel needed.

---

## 📁 Project Structure

```
autogo-website/
├── src/
│   ├── components/          # Reusable Astro components
│   │   ├── CarCard.astro    # Individual car display card
│   │   ├── Navbar.astro     # Navigation header
│   │   └── Footer.astro     # Footer section
│   │
│   ├── pages/               # Page routes (Astro routing)
│   │   ├── index.astro      # Home page (GET /)
│   │   ├── cars.astro       # Cars listing page (GET /cars)
│   │   └── car/
│   │       └── [slug].astro # Dynamic car detail page (GET /cars/[slug])
│   │
│   ├── layouts/             # Layout templates
│   │   └── BaseLayout.astro # Base layout wrapper
│   │
│   ├── data/                # Static data files
│   │   └── cars.json        # All cars data (JSON format)
│   │
│   ├── assets/              # Static assets (images, videos links)
│   │   └── cars/            # Car images (WebP format ONLY)
│   │
│   └── styles/              # Global styles
│       └── global.css       # Global CSS
│
├── public/                  # Public static files (favicon, robots.txt, etc)
│
├── package.json             # NPM dependencies & scripts
├── astro.config.mjs         # Astro configuration
├── tsconfig.json            # TypeScript configuration
└── STRUCTURE.md             # This file
```

---

## 🚀 Getting Started

### 1. Install Dependencies
```bash
npm install
```

### 2. Start Development Server
```bash
npm run dev
```
Server runs at: `http://localhost:3000`

### 3. Build for Production
```bash
npm run build
```

---

## 📊 How to Add Cars (Data Management)

### Step 1: Edit `/src/data/cars.json`

Add car objects to the JSON array:

```json
[
  {
    "id": 1,
    "name": "BMW X5",
    "brand": "BMW",
    "year": 2022,
    "price": "45000",
    "currency": "€",
    "slug": "bmw-x5",
    "image": "/cars/bmw-x5.webp",
    "images": [
      "/cars/bmw-x5-1.webp",
      "/cars/bmw-x5-2.webp"
    ],
    "video": "https://www.youtube.com/embed/VIDEO_ID",
    "description": "Luxury SUV with advanced features",
    "mileage": "5,000 km",
    "transmission": "Automatic",
    "fuelType": "Petrol",
    "engine": "3.0L Twin-Turbo"
  }
]
```

### Required Fields:
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | number | ✅ | Unique identifier |
| `name` | string | ✅ | Car model name |
| `brand` | string | ✅ | Brand/Make |
| `year` | number | ✅ | Year of manufacture |
| `price` | string | ✅ | Price number (without currency) |
| `currency` | string | ✅ | € or $ |
| `slug` | string | ✅ | URL-friendly name (lowercase, hyphens) |
| `image` | string | ✅ | Main image path |
| `images` | array | ❌ | Additional images array |
| `video` | string | ❌ | YouTube embed link |
| `description` | string | ❌ | Car details |
| `mileage` | string | ❌ | Kilometers |
| `transmission` | string | ❌ | Auto/Manual |
| `fuelType` | string | ❌ | Petrol/Diesel/Electric |
| `engine` | string | ❌ | Engine specs |

---

## 🖼️ Image Management

### Image Requirements:
- **Format**: WebP ONLY (`.webp`)
- **Max Size**: 300KB per image
- **Dimensions**: 800-1200px width
- **Location**: `/src/assets/cars/`

### Naming Convention:
```
bmw-x5.webp           # Main car image
bmw-x5-interior.webp  # Interior shot
bmw-x5-engine.webp    # Engine view
```

### How to Add Images:
1. Convert images to WebP format
2. Place in `/src/assets/cars/`
3. Reference in `cars.json` with paths like:
   ```
   "/cars/bmw-x5.webp"
   ```

### Image Optimization Tools:
- **Online**: https://convertio.co/webp/ or https://cloudconvert.com/
- **CLI**: `cwebp image.jpg -o image.webp`

---

## 🎨 Component Guide

### `CarCard.astro`
Displays a single car in the listing.

**Props to use:**
```astro
interface Props {
  id: number;
  name: string;
  price: string;
  currency: string;
  image: string;
  slug: string;
}
```

**Example usage in parent:**
```astro
<CarCard id={car.id} name={car.name} price={car.price} 
         currency={car.currency} image={car.image} slug={car.slug} />
```

---

### `Navbar.astro`
Navigation bar with links to Home, Cars listing, and Contact.

---

### `Footer.astro`
Footer with company info, links, and social media.

---

### `BaseLayout.astro`
Main layout component that wraps every page.

**Usage in pages:**
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout>
  <!-- Page content here -->
</BaseLayout>
```

---

## 📄 Page Routes

### `pages/index.astro` (Home Page)
- Hero section with CTA button
- Featured cars carousel or grid
- Call-to-action sections

### `pages/cars.astro` (Cars Listing)
- Display all cars from `cars.json`
- Grid layout (3-4 columns desktop, 1-2 mobile)
- Optional: Filters (brand, price, year)
- Optional: Search bar
- Pagination or "Load More" button

### `pages/car/[slug].astro` (Car Details)
- Dynamic route based on car slug
- Full car details:
  - Image gallery
  - Video embed
  - Specs (year, mileage, transmission, etc.)
  - Description
  - Call-to-action button (Contact/Inquiry)

---

## ⚡ Performance Rules (MUST FOLLOW)

### 1. Image Optimization
```astro
<!-- Always use lazy loading -->
<img src="/cars/bmw-x5.webp" loading="lazy" alt="BMW X5" />

<!-- OR use Astro Image component for auto-optimization -->
import { Image } from 'astro:assets';
<Image src={carImage} alt="Car name" />
```

### 2. Data Fetching
Keep it simple:
```astro
---
import cars from '../data/cars.json';
---

{cars.map(car => (
  <CarCard {...car} />
))}
```

### 3. Limit Initial Load
- Show max **12 cars per page**
- Implement pagination OR "Load More" button
- Reduces initial page load time

### 4. No Unnecessary JavaScript
- Avoid heavy libraries
- Use Astro's static rendering (default)
- Minimize interactivity

### 5. CSS Best Practices
- Keep styles in `/src/styles/global.css`
- Use component-scoped CSS in `.astro` files
- Optional: Add Tailwind CSS for faster styling

---

## 🎥 Video Handling

### ✅ DO:
- Use YouTube embed links
- Use Vimeo embed links

```astro
<iframe 
  src="https://www.youtube.com/embed/VIDEO_ID" 
  width="400" 
  height="300"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
></iframe>
```

### ❌ DON'T:
- Upload video files to the project
- Use local video files

---

## 🌍 Hosting & Deployment

### Vercel (Recommended)
```bash
npm run build
vercel deploy
```

### Netlify
```bash
npm run build
netlify deploy
```

Both platforms auto-detect Astro projects.

---

## 📋 Checklist for Developer

- [ ] Install dependencies: `npm install`
- [ ] Add cars to `/src/data/cars.json`
- [ ] Add car images to `/src/assets/cars/` (WebP format)
- [ ] Build components (CarCard, Navbar, Footer)
- [ ] Create page layouts (Home, Cars, Details)
- [ ] Test responsive design (mobile, tablet, desktop)
- [ ] Optimize images (< 300KB each)
- [ ] Build for production: `npm run build`
- [ ] Deploy to Vercel/Netlify

---

## 🆘 Common Issues & Solutions

### Images not loading?
- Check file path matches `cars.json`
- Ensure image is in `/src/assets/cars/`
- Verify WebP format and file size

### Dynamic routes not working?
- Filename must be `[slug].astro`
- Slug values in `cars.json` must match URL structure

### Page loads slowly?
- Check image file sizes
- Reduce number of cars on first load
- Use lazy loading for images

### Videos not showing?
- Use YouTube/Vimeo embed links only
- Don't use direct video file uploads

---

## 📚 Useful Resources

- **Astro Docs**: https://docs.astro.build
- **WebP Conversion**: https://cloudconvert.com
- **Image Optimizer**: https://tinypng.com
- **JSON Validator**: https://jsonlint.com
- **Vercel Docs**: https://vercel.com/docs
- **Netlify Docs**: https://docs.netlify.com

---

## 🎯 Project Goals (Remember)

✅ Fast loading (< 1 second)
✅ Handle 40+ cars smoothly
✅ Beautiful UI (luxury dealer style)
✅ Mobile responsive
✅ No backend required

Good luck! 🚗

