# AgentEvals Website

The official website for [AgentEvals](https://github.com/agentevals-dev/agentevals) — score agent behavior from OpenTelemetry traces without re-running the agent.

🌐 **Live site:** https://agentevals-dev.github.io/website/

## Prerequisites

- [Hugo](https://gohugo.io/installation/) v0.154+ (extended edition)

## Local Development

```bash
# Clone the repo
git clone https://github.com/agentevals-dev/website.git
cd website

# Start the dev server
hugo server

# Open http://localhost:1313
```

The dev server supports hot reload — changes to templates, content, and styles are reflected instantly.

## Build for Production

```bash
hugo --minify
```

Output is written to the `public/` directory.

## Deployment

The site deploys automatically to GitHub Pages via GitHub Actions on every push to `main`.

## Project Structure

```
.
├── content/
│   ├── _index.md              # Homepage content
│   └── docs/                  # Documentation pages
│       ├── quick-start.md
│       ├── configuration.md
│       ├── examples.md
│       ├── ci-cd.md
│       ├── mcp-server.md
│       └── web-ui.md
├── layouts/
│   ├── index.html             # Homepage template
│   ├── _default/baseof.html   # Base template (head, scripts)
│   └── docs/                  # Docs templates
├── static/
│   ├── css/style.css          # All styles
│   └── images/                # Logo assets
├── hugo.toml                  # Site configuration
└── .github/workflows/         # GitHub Pages deploy
```
