# Personal site

A minimal, single-page bio site — dark mode, no build step, no framework. Just `index.html` + `style.css`.

## Edit your content

Everything is in `index.html`. Search for these and replace with your own info:

- **Name & role** — `<h1>Jordan Rivera</h1>` and the `.role` line
- **Status line** — the "now building..." sentence (delete the whole `<p class="status">` block if you don't want it)
- **Bio** — the `<section class="bio">` paragraph
- **Links** — email, GitHub, LinkedIn, Twitter/X, resume href's in `<nav class="links">`
- **Projects** — each `<li class="work-item">` is one project: title, link, year, description, tags
- **Writing** — optional list of posts/notes; delete the whole `<section>` if you don't write
- **Photo** — swap the placeholder `<span>JR</span>` inside `.avatar` for `<img src="assets/photo.jpg" alt="Your Name">`, and drop your photo in an `assets/` folder
- **Resume** — put a `resume.pdf` next to `index.html`, or change the link

## Deploy on GitHub Pages

1. Create a new repo named `krishpri.github.io` (must match your GitHub username exactly).
2. Push these files to the repo's default branch:
   ```
   git init
   git add .
   git commit -m "initial site"
   git branch -M main
   git remote add origin https://github.com/krishpri/krishpri.github.io.git
   git push -u origin main
   ```
3. In the repo settings → Pages, set the source to the `main` branch, root folder.
4. Your site will be live at `https://krishpri.github.io/` within a minute or two.

If you'd rather host it at a project URL (`yourusername.github.io/site-name`) instead of the root, any repo name works — just enable Pages the same way.

## Notes

- Fonts (Fraunces, Inter, JetBrains Mono) load from Google Fonts via CDN — no local font files needed.
- Colors, spacing, and type scale all live in the `:root` variables at the top of `style.css` if you want to retheme.
- Respects `prefers-reduced-motion` (disables the blinking cursor).
