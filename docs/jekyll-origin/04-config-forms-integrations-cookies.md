# 04. Forms, Integrations & Cookies

> **Upstream Sources:**
> * [https://www.zerostatic.io/docs/jekyll-origin/config/contact-form/](https://www.zerostatic.io/docs/jekyll-origin/config/contact-form/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/cookie-banner/](https://www.zerostatic.io/docs/jekyll-origin/config/cookie-banner/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/comments/](https://www.zerostatic.io/docs/jekyll-origin/config/comments/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/newsletter/](https://www.zerostatic.io/docs/jekyll-origin/config/newsletter/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/analytics/](https://www.zerostatic.io/docs/jekyll-origin/config/analytics/)

---

## 1. Upstream Documentation (Vanilla Specification)

### Contact Form
Supports two providers: Netlify Forms (default) or Formspree.
```yaml
contact_form:
  use_netlify_form: true
  use_formspree_form: false
  formspree_endpoint: ""
  netlify_form_name: "contact"
```

### Cookie Banner
The theme integrates the open-source [CookieConsent](https://github.com/orestbida/cookieconsent) library:
```yaml
cookie_banner:
  enabled: true
  show_manage_cookies_at_bottom: false
```
* Can gate external scripts (`data-cookiecategory="analytics"`) until user consent is granted.

### Comments
Supports Disqus and Commento:
```yaml
comments:
  commento:
    enabled: false
  disqus:
    shortname: "zerostatic"
```

### Newsletter
Mailchimp subscription embed:
```yaml
newsletter:
  mailchimp:
    form_action_url: "https://..."
    form_title: "Stay In Touch"
```

### Analytics
Supports Google Analytics 4, Google Tag Manager, Plausible, and Umami:
```yaml
analytics:
  disable_analytics_on_localhost: false
  google_analytics_id: ""
  gtm_id: ""
  plausible_data_domain: ""
  umami_data_website_id: ""
  umami_src: ""
```

---

## 2. Repository Implementation & What Supersedes Vanilla

### Status: SUPERSEDED / PARTIALLY STALE
* **Formspree Supersedes Netlify Forms:**
  * In [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml):
    ```yaml
    contact_form:
      use_netlify_form: false
      use_formspree_form: true
      formspree_endpoint: https://formspree.io/f/xldqwdqb
      netlify_form_name: "contact"
    ```
  * Submissions from [`pages/contact.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/contact.md) post directly to Rosalind's Formspree endpoint via [`_includes/framework/form-contact-formspree.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/form-contact-formspree.html).
* **Flattened Cookie Include Path:**
  * Upstream docs point to `_includes/framework/global/cookies/cookie-consent.html`.
  * **In this repo, the file is flat:** [`_includes/framework/cookie-consent.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/cookie-consent.html).
* **Analytics & Tracking State:**
  * All analytics IDs in `_config.yml` are blank (`""`).
  * No Google Analytics, Plausible, or Umami cookies are currently set, making the active cookie banner mostly decorative until an analytics provider is configured.
* **Stale Demo Defaults Warning:**
  * `disqus.shortname` is still set to `"zerostatic"`.
  * Mailchimp URL still points to a Zerostatic demo list.
  * If comments or newsletter blocks are rendered, they should be disabled or pointed to Rosalind's real accounts.
