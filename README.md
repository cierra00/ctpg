# CTPG Design System

A public static-asset repository for the CTPG CSS/JS design system, hosted via Netlify.

---

## Folder Structure

```
ctpg-design-system/
├── v1/
│   ├── css/
│   │   └── ctpg.min.css
│   └── js/
│       └── ctpg.min.js
├── index.html
└── README.md
```

---

## Hosting on Netlify (beginner-friendly setup)

### 1. Connect this repo to Netlify

1. Log into [Netlify](https://app.netlify.com/).
2. Click **Add new site** → **Import an existing project**.
3. Choose **GitHub** and authorize Netlify if prompted.
4. Select this repository (`ctpg-design-system`).

### 2. Netlify build settings (important)

When Netlify asks for build settings:

- **Build command:** leave **blank**
- **Publish directory:** `/` (or `.` — both mean the repo root)

Click **Deploy site**.

### 3. Test the Netlify URL

After deployment, Netlify gives you a URL like `https://your-site-name.netlify.app`.

Test these paths in your browser:

- `https://your-site-name.netlify.app/`
- `https://your-site-name.netlify.app/v1/css/ctpg.min.css`
- `https://your-site-name.netlify.app/v1/js/ctpg.min.js`

### 4. Add the custom domain

1. In Netlify, open your site → **Domain management**.
2. Click **Add a domain** and enter `media.cruiseandthemeparkguide.com`.
3. Follow Netlify's instructions to verify.

### 5. Create the DNS CNAME record (Namecheap example)

In your DNS provider (e.g., Namecheap → Advanced DNS → Host Records):

| Type  | Host    | Value                              | TTL       |
|-------|---------|------------------------------------|-----------|
| CNAME | `media` | `your-site-name.netlify.app`       | Automatic |

DNS may take a few minutes to propagate. Netlify will provision a free SSL certificate automatically once DNS resolves.

---

## Final Asset URLs (with custom domain)

Once DNS is live, your assets are available at:

```
https://media.cruiseandthemeparkguide.com/v1/css/ctpg.min.css
https://media.cruiseandthemeparkguide.com/v1/js/ctpg.min.js
```

---

## WordPress Enqueue (functions.php)

Add this to your theme's `functions.php` or a site plugin:

```php
<?php
add_action('wp_enqueue_scripts', function () {
  // CSS
  wp_enqueue_style(
    'ctpg-design-system',
    'https://media.cruiseandthemeparkguide.com/v1/css/ctpg.min.css',
    array(),
    null
  );

  // JS (loads in footer)
  wp_enqueue_script(
    'ctpg-design-system',
    'https://media.cruiseandthemeparkguide.com/v1/js/ctpg.min.js',
    array(),
    null,
    true
  );
});
```

---

## ⚠️ Important: Always version your folders

**Never overwrite files in an existing version folder once they are live.**

Instead, create a new version folder for each release:

```
v1/css/ctpg.min.css   ← leave this alone after publishing
v2/css/ctpg.min.css   ← new version goes here
v3/css/ctpg.min.css   ← future version
```

Why this matters:
- Prevents breaking existing pages/themes that depend on `v1`.
- Makes rollbacks simple.
- Gives you stable, predictable URLs for each release.
