# rms1000watt-web

Website: https://rms1000watt.com

## Overview

This is a static website deployed on Netlify. It consists of a tree of `index.html` files in nested directories. Each `index.html` is independent and self-contained.

## Repo Structure

```
rms1000watt-web/
├── index.html              # Root index page
├── netlify.toml            # Netlify configuration
├── archive/                # Archived content
│   └── content/
│       └── blog/           # Old blog posts
├── exercise/               # Exercise tools
│   └── workout-schedule/
│       └── index.html
├── friends/                # Friend-related pages
│   └── grand/
│       └── index.html
└── games/                  # Browser games
    ├── animal-empire/
    │   └── index.html
    ├── ball-drop/
    │   └── index.html
    └── elastomania/
        └── index.html
```

## Root Index Page

The root `index.html` has two sections:
1. **Top section**: Bio with name, tagline, and tech stack
2. **Bottom section**: Site map with collapsible groups linking to subdirectories

## Deployment

- Deployed on **Netlify**
- Automatic deploy on push to `master`
- Merged code can be verified by checking https://rms1000watt.com

## Rules

- **ALWAYS update the sitemap in the root `index.html` whenever adding a new file or directory**
- Each subdirectory should contain its own `index.html`
- Keep each `index.html` self-contained (no shared templates or layouts)
