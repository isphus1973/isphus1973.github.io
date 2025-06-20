# isphus1973.github.io

Personal blog built with [Astro](https://astro.build/), ready for deployment on GitHub Pages.

## Features

- Markdown support for posts
- LaTeX equations rendering with KaTeX (use `$...$` or `$$...$$`)
- Responsive theme with pure CSS
- SEO, sitemap, RSS, and optimized performance
- Automatic deploy via GitHub Actions

## Project Structure

```
├── public/
├── src/
│   ├── components/
│   ├── content/
│   ├── layouts/
│   └── pages/
├── astro.config.mjs
├── README.md
├── package.json
└── tsconfig.json
```

## Commands

| Command           | Action                                         |
|------------------|------------------------------------------------|
| `npm install`     | Install dependencies                           |
| `npm run dev`     | Start local server at `localhost:4321`         |
| `npm run build`   | Generate static site in `./dist/`              |
| `npm run preview` | Preview the build locally                      |

## Deploy on GitHub Pages

Deployment is done automatically via GitHub Actions when committing to the `main` branch.

## How to use LaTeX

Just write equations between `$...$` (inline) or `$$...$$` (block) in your Markdown files.

---

This project is based on the official Astro blog template.
