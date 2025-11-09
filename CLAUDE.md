# Portfolio Site Implementation Guide

## Quick Start

This guide will walk you through building a minimalist, high-performance developer portfolio site from scratch. Target: Ship in 4-6 hours.

---

## Tech Stack Decision

**Recommendation: Astro**
- Faster than Next.js for static sites
- Better Lighthouse scores out of the box
- Simpler deployment
- Less JavaScript shipped to client

Alternative: Next.js 14+ with App Router if you need dynamic features.

---

## Phase 1: Project Setup (15 minutes)

### Step 1: Initialize Astro Project

```bash
npm create astro@latest portfolio -- --template minimal --typescript strict --git
cd portfolio
npm install
```

### Step 2: Install Dependencies

```bash
npm install -D tailwindcss @astrojs/tailwind
npx astro add tailwind
```

Accept all prompts. This configures Tailwind CSS automatically.

### Step 3: Project Structure

Create this folder structure:

```
portfolio/
├── src/
│   ├── components/
│   │   ├── Hero.astro
│   │   ├── Projects.astro
│   │   ├── ProjectCard.astro
│   │   ├── Skills.astro
│   │   ├── Contact.astro
│   │   └── ThemeToggle.astro
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   └── index.astro
│   └── styles/
│       └── global.css
├── public/
│   ├── projects/
│   │   └── (screenshots go here)
│   └── favicon.ico
└── package.json
```

---

## Phase 2: Core Components (2-3 hours)

### BaseLayout.astro

```astro
---
interface Props {
  title: string;
  description: string;
}

const { title, description } = Astro.props;
---

<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />

    <!-- SEO Meta Tags -->
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:type" content="website" />
    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:title" content={title} />
    <meta name="twitter:description" content={description} />

    <link rel="icon" type="image/x-icon" href="/favicon.ico" />
    <title>{title}</title>

    <!-- Theme Script (runs before page load to prevent flash) -->
    <script is:inline>
      const theme = (() => {
        if (typeof localStorage !== 'undefined' && localStorage.getItem('theme')) {
          return localStorage.getItem('theme');
        }
        if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
          return 'dark';
        }
        return 'light';
      })();

      if (theme === 'dark') {
        document.documentElement.classList.add('dark');
      } else {
        document.documentElement.classList.remove('dark');
      }
      window.localStorage.setItem('theme', theme);
    </script>
  </head>
  <body class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 transition-colors duration-300">
    <slot />
  </body>
</html>

<style is:global>
  @import '../styles/global.css';
</style>
```

### global.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  }

  /* Focus states for accessibility */
  *:focus-visible {
    @apply outline-2 outline-offset-2 outline-blue-500;
  }
}

@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
}
```

### Hero.astro

```astro
---
import ThemeToggle from './ThemeToggle.astro';
---

<section id="hero" class="min-h-screen flex items-center justify-center px-4 py-20 relative">
  <div class="absolute top-8 right-8">
    <ThemeToggle />
  </div>

  <div class="max-w-3xl mx-auto text-center">
    <h1 class="text-5xl md:text-7xl font-bold mb-6 text-balance">
      Abubakar Mahmood
    </h1>

    <p class="text-xl md:text-2xl text-gray-600 dark:text-gray-400 mb-4">
      Full-Stack Developer specializing in enterprise systems and real-time applications
    </p>

    <p class="text-lg text-gray-500 dark:text-gray-500 mb-8 max-w-2xl mx-auto">
      Building scalable web applications with modern technologies.
      Passionate about clean code, performance optimization, and solving complex problems.
    </p>

    <div class="flex gap-4 justify-center">
      <a
        href="https://github.com/yourusername"
        target="_blank"
        rel="noopener noreferrer"
        class="inline-flex items-center gap-2 px-6 py-3 bg-gray-900 dark:bg-gray-100 text-white dark:text-gray-900 rounded-lg hover:bg-gray-700 dark:hover:bg-gray-300 transition-colors"
        aria-label="GitHub Profile"
      >
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
          <path fill-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z" clip-rule="evenodd"></path>
        </svg>
        GitHub
      </a>

      <a
        href="https://linkedin.com/in/yourusername"
        target="_blank"
        rel="noopener noreferrer"
        class="inline-flex items-center gap-2 px-6 py-3 border-2 border-gray-900 dark:border-gray-100 text-gray-900 dark:text-gray-100 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"
        aria-label="LinkedIn Profile"
      >
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
        </svg>
        LinkedIn
      </a>
    </div>

    <div class="mt-12">
      <a
        href="#projects"
        class="text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100 transition-colors"
        aria-label="Scroll to projects"
      >
        <svg class="w-6 h-6 mx-auto animate-bounce" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"></path>
        </svg>
      </a>
    </div>
  </div>
</section>
```

### ProjectCard.astro

```astro
---
interface Props {
  title: string;
  description: string;
  image: string;
  tech: string[];
  demoUrl?: string;
  githubUrl: string;
  challenge: string;
  solution: string;
}

const { title, description, image, tech, demoUrl, githubUrl, challenge, solution } = Astro.props;
const id = title.toLowerCase().replace(/\s+/g, '-');
---

<article class="group bg-white dark:bg-gray-800 rounded-xl shadow-lg overflow-hidden border border-gray-200 dark:border-gray-700 transition-all hover:shadow-xl">
  <div class="aspect-video overflow-hidden bg-gray-100 dark:bg-gray-700">
    <img
      src={image}
      alt={`${title} screenshot`}
      loading="lazy"
      class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
    />
  </div>

  <div class="p-6">
    <h3 class="text-2xl font-bold mb-2">{title}</h3>
    <p class="text-gray-600 dark:text-gray-400 mb-4">{description}</p>

    <div class="flex flex-wrap gap-2 mb-4">
      {tech.map((t) => (
        <span class="px-3 py-1 bg-blue-100 dark:bg-blue-900 text-blue-800 dark:text-blue-200 rounded-full text-sm font-medium">
          {t}
        </span>
      ))}
    </div>

    <div class="flex gap-3 mb-4">
      {demoUrl && (
        <a
          href={demoUrl}
          target="_blank"
          rel="noopener noreferrer"
          class="inline-flex items-center gap-1 text-blue-600 dark:text-blue-400 hover:underline"
        >
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path>
          </svg>
          Live Demo
        </a>
      )}

      <a
        href={githubUrl}
        target="_blank"
        rel="noopener noreferrer"
        class="inline-flex items-center gap-1 text-gray-600 dark:text-gray-400 hover:underline"
      >
        <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
          <path fill-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z" clip-rule="evenodd"></path>
        </svg>
        Code
      </a>
    </div>

    <button
      class="case-study-toggle text-sm text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100 font-medium transition-colors"
      data-target={id}
      aria-expanded="false"
      aria-controls={id}
    >
      View Case Study ↓
    </button>

    <div id={id} class="case-study hidden mt-4 pt-4 border-t border-gray-200 dark:border-gray-700">
      <div class="mb-3">
        <h4 class="font-semibold text-sm uppercase text-gray-700 dark:text-gray-300 mb-1">Challenge</h4>
        <p class="text-gray-600 dark:text-gray-400 text-sm">{challenge}</p>
      </div>
      <div>
        <h4 class="font-semibold text-sm uppercase text-gray-700 dark:text-gray-300 mb-1">Solution</h4>
        <p class="text-gray-600 dark:text-gray-400 text-sm">{solution}</p>
      </div>
    </div>
  </div>
</article>

<script>
  document.addEventListener('DOMContentLoaded', () => {
    const toggles = document.querySelectorAll('.case-study-toggle');

    toggles.forEach(toggle => {
      toggle.addEventListener('click', () => {
        const targetId = toggle.getAttribute('data-target');
        const target = document.getElementById(targetId);

        if (target) {
          target.classList.toggle('hidden');
          const isExpanded = !target.classList.contains('hidden');
          toggle.setAttribute('aria-expanded', isExpanded.toString());
          toggle.textContent = isExpanded ? 'Hide Case Study ↑' : 'View Case Study ↓';
        }
      });
    });
  });
</script>
```

### Projects.astro

```astro
---
import ProjectCard from './ProjectCard.astro';

const projects = [
  {
    title: "BLE Mesh Chat App",
    description: "Real-time peer-to-peer messaging app using Bluetooth Low Energy mesh networking for offline communication.",
    image: "/projects/ble-mesh.jpg",
    tech: ["React Native", "BLE", "TypeScript", "Redux"],
    githubUrl: "https://github.com/yourusername/ble-mesh-chat",
    demoUrl: "",
    challenge: "Building reliable mesh networking on mobile devices with varying Bluetooth capabilities and maintaining message delivery across disconnected nodes.",
    solution: "Implemented custom flooding algorithm with message deduplication, node discovery protocol, and persistent queue for offline message relay. Achieved 95% delivery rate within 3-hop network."
  },
  {
    title: "ASP.NET Inventory System",
    description: "Enterprise inventory management system with real-time stock tracking, automated reordering, and role-based access control.",
    image: "/projects/inventory-system.jpg",
    tech: ["ASP.NET Core", "C#", "PostgreSQL", "Entity Framework"],
    githubUrl: "https://github.com/yourusername/inventory-system",
    demoUrl: "https://inventory-demo.example.com",
    challenge: "Handling concurrent inventory updates from multiple warehouses while preventing overselling and maintaining data consistency.",
    solution: "Designed distributed locking mechanism with PostgreSQL advisory locks, implemented CQRS pattern for read/write separation, and added event sourcing for full audit trail."
  },
  {
    title: "Next.js Activity Tracker",
    description: "Personal productivity tracker with habit streaks, goal setting, and analytics dashboard built with modern React patterns.",
    image: "/projects/tracker.jpg",
    tech: ["Next.js 14", "TypeScript", "Prisma", "TailwindCSS"],
    githubUrl: "https://github.com/yourusername/activity-tracker",
    demoUrl: "https://tracker-demo.vercel.app",
    challenge: "Creating responsive real-time charts and maintaining performant infinite scroll with large datasets while ensuring sub-200ms page loads.",
    solution: "Leveraged React Server Components for initial data fetch, implemented virtual scrolling for activity lists, and used Chart.js with canvas rendering for 60fps chart updates."
  },
  {
    title: "Go Rate Limiter",
    description: "High-performance distributed rate limiting library using Redis with support for sliding window and token bucket algorithms.",
    image: "/projects/rate-limiter.jpg",
    tech: ["Go", "Redis", "Docker", "Prometheus"],
    githubUrl: "https://github.com/yourusername/go-rate-limiter",
    demoUrl: "",
    challenge: "Achieving <1ms latency for rate limit checks at 100k+ requests/second while maintaining accuracy across distributed instances.",
    solution: "Built Lua script-based atomic operations in Redis, implemented local cache with write-through strategy, and used consistent hashing for horizontal scaling. Benchmarked at 850k ops/sec."
  }
];
---

<section id="projects" class="py-20 px-4 bg-gray-50 dark:bg-gray-950">
  <div class="max-w-6xl mx-auto">
    <h2 class="text-4xl font-bold mb-4 text-center">Featured Projects</h2>
    <p class="text-gray-600 dark:text-gray-400 text-center mb-12 max-w-2xl mx-auto">
      A selection of projects showcasing full-stack development, system design, and problem-solving skills.
    </p>

    <div class="grid md:grid-cols-2 gap-8">
      {projects.map((project) => (
        <ProjectCard {...project} />
      ))}
    </div>
  </div>
</section>
```

### Skills.astro

```astro
---
const skillCategories = [
  {
    category: "Languages",
    skills: ["TypeScript", "JavaScript", "C#", "Go", "Python", "SQL"]
  },
  {
    category: "Frontend",
    skills: ["React", "Next.js", "Astro", "TailwindCSS", "React Native"]
  },
  {
    category: "Backend",
    skills: ["Node.js", "ASP.NET Core", "Express", "Prisma", "Entity Framework"]
  },
  {
    category: "Databases & Tools",
    skills: ["PostgreSQL", "Redis", "MongoDB", "Docker", "Git", "Linux"]
  }
];
---

<section id="skills" class="py-20 px-4">
  <div class="max-w-5xl mx-auto">
    <h2 class="text-4xl font-bold mb-12 text-center">Skills & Technologies</h2>

    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
      {skillCategories.map(({ category, skills }) => (
        <div>
          <h3 class="text-lg font-semibold mb-4 text-gray-700 dark:text-gray-300">{category}</h3>
          <ul class="space-y-2">
            {skills.map((skill) => (
              <li class="flex items-center gap-2 text-gray-600 dark:text-gray-400">
                <svg class="w-4 h-4 text-green-500" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
                </svg>
                {skill}
              </li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  </div>
</section>
```

### Contact.astro

```astro
<section id="contact" class="py-20 px-4 bg-gray-50 dark:bg-gray-950">
  <div class="max-w-2xl mx-auto text-center">
    <h2 class="text-4xl font-bold mb-4">Get In Touch</h2>
    <p class="text-gray-600 dark:text-gray-400 mb-8">
      Interested in working together? Let's connect.
    </p>

    <div class="mb-8">
      <a
        href="mailto:your.email@example.com"
        class="inline-flex items-center gap-2 px-8 py-4 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors text-lg font-medium"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path>
        </svg>
        Send Email
      </a>
    </div>

    <!-- Optional: Formspree Contact Form -->
    <div class="text-left">
      <form
        action="https://formspree.io/f/YOUR_FORM_ID"
        method="POST"
        class="space-y-4"
      >
        <div>
          <label for="name" class="block text-sm font-medium mb-2">Name</label>
          <input
            type="text"
            id="name"
            name="name"
            required
            class="w-full px-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-800 focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        </div>

        <div>
          <label for="email" class="block text-sm font-medium mb-2">Email</label>
          <input
            type="email"
            id="email"
            name="email"
            required
            class="w-full px-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-800 focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        </div>

        <div>
          <label for="message" class="block text-sm font-medium mb-2">Message</label>
          <textarea
            id="message"
            name="message"
            rows="4"
            required
            class="w-full px-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-800 focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          ></textarea>
        </div>

        <button
          type="submit"
          class="w-full px-6 py-3 bg-gray-900 dark:bg-gray-100 text-white dark:text-gray-900 rounded-lg hover:bg-gray-700 dark:hover:bg-gray-300 transition-colors font-medium"
        >
          Send Message
        </button>
      </form>
    </div>
  </div>
</section>
```

### ThemeToggle.astro

```astro
<button
  id="theme-toggle"
  class="p-2 rounded-lg bg-gray-200 dark:bg-gray-700 hover:bg-gray-300 dark:hover:bg-gray-600 transition-colors"
  aria-label="Toggle dark mode"
>
  <svg id="theme-toggle-dark-icon" class="hidden w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
    <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path>
  </svg>
  <svg id="theme-toggle-light-icon" class="hidden w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
    <path d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" fill-rule="evenodd" clip-rule="evenodd"></path>
  </svg>
</button>

<script>
  const themeToggle = document.getElementById('theme-toggle');
  const themeToggleDarkIcon = document.getElementById('theme-toggle-dark-icon');
  const themeToggleLightIcon = document.getElementById('theme-toggle-light-icon');

  // Show correct icon on load
  if (document.documentElement.classList.contains('dark')) {
    themeToggleLightIcon?.classList.remove('hidden');
  } else {
    themeToggleDarkIcon?.classList.remove('hidden');
  }

  themeToggle?.addEventListener('click', () => {
    // Toggle icons
    themeToggleDarkIcon?.classList.toggle('hidden');
    themeToggleLightIcon?.classList.toggle('hidden');

    // Toggle dark mode
    if (document.documentElement.classList.contains('dark')) {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('theme', 'light');
    } else {
      document.documentElement.classList.add('dark');
      localStorage.setItem('theme', 'dark');
    }
  });
</script>
```

### index.astro (Main Page)

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Hero from '../components/Hero.astro';
import Projects from '../components/Projects.astro';
import Skills from '../components/Skills.astro';
import Contact from '../components/Contact.astro';
---

<BaseLayout
  title="Abubakar Mahmood - Full-Stack Developer"
  description="Full-Stack Developer specializing in enterprise systems and real-time applications. Building scalable web applications with TypeScript, C#, Go, and modern frameworks."
>
  <main>
    <Hero />
    <Projects />
    <Skills />
    <Contact />
  </main>

  <footer class="py-8 px-4 text-center text-gray-500 dark:text-gray-400 text-sm border-t border-gray-200 dark:border-gray-800">
    <p>© {new Date().getFullYear()} Abubakar Mahmood. Built with Astro.</p>
  </footer>
</BaseLayout>
```

---

## Phase 3: Configuration (30 minutes)

### Tailwind Configuration

Update `tailwind.config.mjs`:

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
  darkMode: 'class',
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Astro Configuration

Update `astro.config.mjs`:

```js
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [tailwind()],
  site: 'https://yoursite.com', // Update with your domain
});
```

### Add Placeholder Images

You need 4 project screenshots. Options:

1. **Use your actual project screenshots** (recommended)
2. **Use placeholder service temporarily**:

Create `/public/projects/` folder and add:
- `ble-mesh.jpg`
- `inventory-system.jpg`
- `tracker.jpg`
- `rate-limiter.jpg`

Temporary placeholder option:
```astro
<!-- In Projects.astro, temporarily use: -->
image: "https://placehold.co/800x450/1e293b/60a5fa?text=BLE+Mesh+Chat"
```

### Favicon

Generate favicon at [favicon.io](https://favicon.io) and place in `/public/favicon.ico`

---

## Phase 4: Testing & Optimization (1 hour)

### Local Development

```bash
npm run dev
```

Visit `http://localhost:4321`

### Pre-Launch Checklist

- [ ] Test all navigation links
- [ ] Verify theme toggle works
- [ ] Test case study expand/collapse
- [ ] Check mobile responsiveness (DevTools)
- [ ] Verify all images load with lazy loading
- [ ] Test keyboard navigation (Tab through all interactive elements)
- [ ] Check focus states are visible
- [ ] Update all URLs (GitHub, LinkedIn, email)
- [ ] Replace project screenshots with real images
- [ ] Update project URLs (demo + GitHub links)
- [ ] Test contact form (sign up at [Formspree](https://formspree.io) first)

### Lighthouse Audit

```bash
npm run build
npm run preview
```

1. Open Chrome DevTools (F12)
2. Go to Lighthouse tab
3. Run audit for Desktop and Mobile
4. Target: All scores 95+

**Common fixes if scores are low:**
- Images too large? Compress with [TinyPNG](https://tinypng.com)
- Missing alt tags? Add to all images
- Contrast issues? Adjust text colors

---

## Phase 5: Deployment (30 minutes)

### Option A: Vercel (Recommended)

```bash
npm install -g vercel
vercel login
vercel
```

Follow prompts:
- Project name: portfolio
- Framework: Astro
- Build command: `npm run build`
- Output directory: `dist`

**Setup custom domain** (optional):
1. Go to Vercel dashboard → Your project → Settings → Domains
2. Add your domain
3. Update DNS records as instructed

**Enable Analytics**:
1. Vercel dashboard → Your project → Analytics
2. Enable Vercel Analytics (free tier)

### Option B: Netlify

```bash
npm install -g netlify-cli
netlify login
netlify init
```

Follow prompts:
- Build command: `npm run build`
- Publish directory: `dist`

Deploy:
```bash
netlify deploy --prod
```

**Enable Analytics**:
1. Netlify dashboard → Your site → Analytics
2. Enable Netlify Analytics

---

## Phase 6: Post-Launch

### Update Your Profiles

Add portfolio link to:
- GitHub profile README
- LinkedIn profile
- Resume
- Email signature

### Monitor Performance

**Vercel Analytics**: Track real user performance
**Google Search Console**: Submit sitemap for SEO

### Iterate

Collect feedback and update:
- Project descriptions based on questions you get
- Add new projects as you build them
- Keep technology list current

---

## Customization Tips

### Change Color Scheme

In component files, replace:
- `blue-600` with your brand color
- Update dark mode variants accordingly

### Add Blog Section

```bash
npx astro add mdx
```

Create `/src/pages/blog/[slug].astro` for blog posts.

### Add Resume Download

```astro
<a
  href="/resume.pdf"
  download
  class="..."
>
  Download Resume
</a>
```

Place `resume.pdf` in `/public/`

---

## Troubleshooting

### Theme Flicker on Load
The inline script in `BaseLayout.astro` should prevent this. If it persists, ensure script has `is:inline` attribute.

### Case Study Not Expanding
Check browser console for JavaScript errors. Ensure script in `ProjectCard.astro` is wrapped in `<script>` tag, not `<script is:inline>`.

### Build Fails
```bash
rm -rf node_modules package-lock.json
npm install
npm run build
```

### Images Not Loading in Production
Ensure images are in `/public/` directory and paths start with `/` (e.g., `/projects/image.jpg`)

---

## Performance Targets

- **First Contentful Paint**: < 1.5s
- **Largest Contentful Paint**: < 2.5s
- **Time to Interactive**: < 3.5s
- **Cumulative Layout Shift**: < 0.1
- **Total Page Size**: < 500KB

---

## Final Checklist for Launch

### Content
- [ ] Personal info updated (name, bio, links)
- [ ] All 4 projects have real screenshots
- [ ] Project descriptions are accurate
- [ ] Skills list is current
- [ ] Contact email is correct
- [ ] Social links work

### Technical
- [ ] Lighthouse scores 95+ (all categories)
- [ ] Mobile responsive (test on real device)
- [ ] Dark mode works
- [ ] All links open correctly
- [ ] Forms work (if using Formspree)
- [ ] Images optimized (< 200KB each)
- [ ] Favicon shows correctly

### SEO
- [ ] Meta description set
- [ ] Open Graph tags present
- [ ] Site title is descriptive
- [ ] Alt text on all images

### Deployment
- [ ] Deployed to Vercel/Netlify
- [ ] Custom domain configured (optional)
- [ ] Analytics enabled
- [ ] HTTPS working

---

## Maintenance Schedule

**Monthly**:
- Update project list if new work completed
- Check for broken links
- Review analytics data

**Quarterly**:
- Update skills/tech stack
- Refresh screenshots if projects changed
- Run Lighthouse audit again

**Annually**:
- Major redesign consideration
- Update bio/experience level
- Evaluate new features

---

## Next Steps After Launch

1. Share on social media
2. Add to job applications
3. Include in email signature
4. Submit to dev portfolio showcases
5. Gather feedback from peers
6. Plan next project to add

---

**Estimated Total Time**: 4-6 hours for initial build and deployment.

**Cost**: $0 (using free tiers for hosting and forms)

You now have a production-ready portfolio site. Focus on shipping it quickly, then iterate based on real feedback. Good luck! 🚀
