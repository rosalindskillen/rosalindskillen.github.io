# 01. Getting Started & Install

> **Upstream Source:** [https://www.zerostatic.io/docs/jekyll-origin/install/](https://www.zerostatic.io/docs/jekyll-origin/install/)

---

## 1. Upstream Documentation (Vanilla Specification)

### Prerequisites
Make sure you have Ruby & RubyGems installed. For a step-by-step guide, refer to the [Jekyll installation docs](https://jekyllrb.com/docs/installation/).

### Theme Installation
Extract the theme `.zip` file into your local working directory:
```bash
bundle install
bundle exec jekyll build
```

### Local Development
Run `jekyll serve` or `bundle exec jekyll serve` to start the local Jekyll development server:
```bash
bundle exec jekyll serve
```
Visit `http://localhost:4000` in your web browser to view the site with automatic live-reloading.

### Deploy to Netlify
Netlify provides free hosting for static sites. The theme includes a pre-configured [`netlify.toml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/netlify.toml) file that instructs Netlify how to build and publish the Jekyll site.

Vanilla Netlify configuration:
```toml
[build]
  command = "jekyll build"
  publish = "_site"

[build.environment]
  JEKYLL_ENV = "production"
  RUBY_VERSION = "3.2.2"
```

---

## 2. Repository Implementation & What Supersedes Vanilla

### Status: MODIFIED / CONSTRAINED
* **Bundler Version Pinning:** In this repository, [`Gemfile.lock`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/Gemfile.lock) locks dependencies to **Bundler 2.3.11**. If you run `bundle install` using a newer Bundler (e.g. Bundler 2.5+), Ruby may warn or fail with `Could not find 'bundler' (2.3.11)`.
  * *Fix:* Use `gem install bundler:2.3.11` before running `bundle install`.
* **Committed `_site/` Directory:** In the vanilla template, `_site/` is generated locally and ignored in git. In this repository, `_site/` is currently committed to git.
  * *Supersession Rule:* When making source edits, **never edit `_site/` directly**. Always make changes in source files (`pages/`, `collections/`, `_includes/`, `_sass/`, etc.) and let Jekyll rebuild the static assets.
* **Netlify CI/CD:** The site uses the provided [`netlify.toml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/netlify.toml). Netlify builds from source using Ruby 3.2.2.
