# Portfolio Site

A minimalist, high-performance developer portfolio website built with modern web technologies.

## Overview

Single-page portfolio website showcasing projects, skills, and contact information. Designed for fast load times, accessibility, and great user experience across all devices.

## Features

- **Single-page design** with smooth scroll navigation
- **Dark mode** with system preference detection
- **95+ Lighthouse scores** across all metrics
- **Mobile-first** responsive design
- **Accessibility-focused** with ARIA labels, keyboard navigation, and focus states
- **SEO optimized** with meta tags and Open Graph support
- **Zero-cost hosting** on Vercel or Netlify free tier

## Tech Stack

- **Framework**: Astro or Next.js 14+
- **Styling**: Tailwind CSS
- **Deployment**: Vercel / Netlify
- **Forms**: Formspree (optional)
- **Analytics**: Vercel/Netlify Analytics

## Project Sections

### 1. Hero Section
- Name and professional title
- Brief bio (2-3 lines)
- Social links (GitHub, LinkedIn)
- Smooth scroll indicator

### 2. Projects Section
4 featured project cards with:
- Project screenshot/GIF
- Technology stack badges
- Brief description
- Links to live demo and source code
- Expandable case study (challenges & solutions)

### 3. Skills Section
Clean grid layout of technologies and tools (no percentages or progress bars)

### 4. Contact Section
- Email link
- Optional contact form
- Social media links

## Getting Started

See **[CLAUDE.md](./CLAUDE.md)** for complete step-by-step implementation guide including:
- Project setup and installation
- Component code with examples
- Configuration and customization
- Testing and optimization
- Deployment instructions
- Post-launch checklist

**Estimated build time**: 4-6 hours

## Design Principles

✅ **Keep it simple** - Clean, professional design
✅ **Performance first** - Optimized images, lazy loading
✅ **Accessible** - WCAG compliant, keyboard navigable
✅ **Mobile-friendly** - Responsive on all screen sizes

❌ **Avoid** - Particle effects, loading screens, auto-play media, skill percentages, generic templates

## Performance Targets

- First Contentful Paint: < 1.5s
- Largest Contentful Paint: < 2.5s
- Time to Interactive: < 3.5s
- Cumulative Layout Shift: < 0.1
- Total Page Size: < 500KB

## Quick Start

```bash
# See CLAUDE.md for detailed setup
npm create astro@latest portfolio -- --template minimal --typescript strict
cd portfolio
npm install -D tailwindcss @astrojs/tailwind
npx astro add tailwind
npm run dev
```

## Deployment

Deploy to Vercel:
```bash
npm install -g vercel
vercel login
vercel
```

Or deploy to Netlify:
```bash
npm install -g netlify-cli
netlify login
netlify init
netlify deploy --prod
```

## License

MIT

## Author

Abubakar Mahmood
