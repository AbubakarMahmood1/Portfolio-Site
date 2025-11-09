# Portfolio-Site
I need you to create a CLAUDE.md file for building a minimalist developer portfolio site.

Project Requirements:
- Single-page portfolio website showcasing 4 projects
- Tech stack: Astro or Next.js 14+ with Tailwind CSS
- Must achieve 95+ Lighthouse scores
- Deploy to Vercel or Netlify free tier

Core Sections:
1. Hero: Name, title, brief bio (2-3 lines), GitHub/LinkedIn links
2. Projects Section: 4 project cards with:
   - Screenshot/GIF
   - Tech stack badges
   - 2-3 line description
   - Live demo link + GitHub repo link
   - "View Case Study" that expands to show challenges/solutions
3. Skills: Simple grid of technologies (no progress bars or percentages)
4. Contact: Email link and optional contact form (using Formspree free tier)

Technical Requirements:
- Mobile-first responsive design
- Dark mode toggle with system preference detection
- Smooth scroll navigation
- Lazy loading for images
- SEO meta tags and Open Graph tags
- Accessibility: ARIA labels, keyboard navigation, focus states
- Analytics: Simple Vercel/Netlify analytics (no Google Analytics)

Avoid These Common Mistakes:
- No particle effects or complex animations
- No loading screens or splash pages  
- No auto-playing music or videos
- No skill level percentages or ratings
- No generic template look - must feel custom
- No broken links or placeholder content
- No "hire me" pop-ups or aggressive CTAs

File Structure:
portfolio/
├── src/
│   ├── components/
│   │   ├── Hero.astro/tsx
│   │   ├── Projects.astro/tsx
│   │   ├── ProjectCard.astro/tsx
│   │   └── ThemeToggle.astro/tsx
│   ├── layouts/
│   │   └── BaseLayout.astro/tsx
│   ├── pages/
│   │   └── index.astro/tsx
│   └── styles/
│       └── global.css
├── public/
│   ├── projects/
│   │   └── [screenshots]
│   └── favicon.ico
└── package.json

Content to Include:
- Hero: "Full-Stack Developer specializing in enterprise systems and real-time applications"
- Project placeholders for: BLE Mesh Chat App (FYP), ASP.NET Inventory System, Next.js Tracker, Go Rate Limiter
- Skills: TypeScript, C#, Go, React, Next.js, ASP.NET Core, PostgreSQL, Redis, Git

Create a comprehensive CLAUDE.md with step-by-step implementation, component code examples, deployment instructions, and a checklist for launch readiness. Focus on shipping quickly with clean, professional design rather than fancy features.
