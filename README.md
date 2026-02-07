# Batota

A personal tech blog built with [Astro](https://astro.build). Programming, IT, and lessons learned the hard way.

**Live site:** [isbatotatech.vercel.app](https://isbatotatech.vercel.app)

## Features

- **Pages:** Home, Blog, About, Contact, Privacy
- **Blog:** Markdown/MDX posts with pagination
- **Dark / light mode** with system preference and manual toggle
- **SEO:** Canonical URLs, Open Graph, sitemap
- **RSS feed**
- **Vercel Speed Insights** for performance metrics
- **Deployed on Vercel** — pushes to `main` auto-deploy

## Project structure

```text
├── public/           # Static assets (favicon, fonts)
├── src/
│   ├── assets/       # Images used in content
│   ├── components/   # Header, Footer, Pagination, etc.
│   ├── content/
│   │   └── blog/     # Markdown/MDX blog posts
│   ├── layouts/     # BlogPost layout
│   ├── pages/       # Astro pages (index, blog, about, contact, privacy)
│   └── styles/      # global.css
├── astro.config.mjs
├── package.json
└── README.md
```

## Commands

| Command             | Action                                      |
| :------------------- | :------------------------------------------ |
| `npm install`        | Install dependencies                         |
| `npm run dev`        | Start dev server at `http://localhost:4321` |
| `npm run build`      | Build for production to `./dist/`            |
| `npm run preview`    | Preview the production build locally         |

## Adding a blog post

1. Add a `.md` or `.mdx` file in `src/content/blog/`.
2. Use frontmatter: `title`, `description`, `pubDate`, and optional `heroImage`, `updatedDate`.
3. Commit and push — Vercel will build and deploy.

## Deployment

The site is deployed on [Vercel](https://vercel.com). Every push to the `main` branch triggers a new build and deployment.

## License

Private project. Theme inspired by [Bear Blog](https://github.com/HermanMartinus/bearblog/).
