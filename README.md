# Tagir Magomedov

Personal site with two sections: Judo and Programming.

- `index.html`: homepage and interest links.
- `judo/index.html`: Judo section index.
- `judo/levels/index.html`: competition infographic at `/judo/levels/`.
- `judo/dutch-judo-pathway.pdf`: original archived PDF; not linked from the live guide.
- `programming/index.html`: index of the existing Scala articles. Add a link here when adding a new article.
- `_posts/scala/`: original articles, published at their existing `/scala/...` URLs by Jekyll.
- `css/personal.css`: homepage and Programming styles. The infographic keeps its own styles.

The four new pages are plain HTML and require no JavaScript framework or build step. GitHub Pages still uses Jekyll to render the original Markdown articles. Keep Jekyll enabled; do not add `.nojekyll`.

## Preview

Run `python -m http.server 8000` in the repository and visit `http://localhost:8000`. This previews the homepage, Programming index, Judo chart, and PDF. Original article URLs require a Jekyll build; with Ruby and the dependencies in `Gemfile` installed, use `bundle exec jekyll serve`.

## Publish

Push to the branch configured in this repository's GitHub **Settings → Pages**. The site lives at https://groz.github.io/. The existing repository branch is `master`; verify the Pages source before changing deployment settings.

The Judo HTML and PDF were imported from the final competition-pathway artifacts created in September 2026. The live chart is a compact guide with an abbreviation key; the original PDF remains archived.


## Content format

Use semantic HTML and CSS for visual guides such as the Judo levels, and Markdown with Jekyll for prose articles. Keep the website as the current edition. The Judo guide has no print button or PDF-download control: use the browser Print command (Ctrl+P / Cmd+P), then a printer or Save as PDF. Its print stylesheet hides navigation and formats the chart for A4.
