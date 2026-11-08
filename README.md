# Till Aust: personal website

Academic homepage built on [al-folio](https://github.com/alshedivat/al-folio) v1.2 (Jekyll).
The layout follows [juevn.github.io](https://juevn.github.io).
The content comes from Till's CV and publication list.

## Deployment

The site is published with GitHub Pages at <https://tilly111.github.io>.

- `.github/workflows/deploy.yml` builds the site on every push to `main` and publishes it to the `gh-pages` branch.
- GitHub Pages serves the `gh-pages` branch (repository settings, Pages).
- The build downloads fonts and scripts (`third_party_libraries.download: true`), so visitors load no files from third-party servers.
- The other upstream workflows and `bin/deploy` are removed.

Update the privacy notice in `_pages/impressum.md` before adding analytics, embeds, or other third-party services.

## Preview on this Mac

### Option A: Docker (recommended by al-folio)

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and start it.
2. Run:

   ```bash
   cd ~/PycharmProjects/website
   docker compose up
   ```

3. Open <http://localhost:8080>. The first start downloads the image and takes a few minutes.
4. Stop with `Ctrl+C`, then `docker compose down`.

Edits to pages, news, projects, and the bibliography reload automatically. Edits to `_config.yml` restart Jekyll inside the container.
Docker binds the preview server to `127.0.0.1`, so only this Mac can open it.

### Option B: Ruby without Docker

macOS ships an old Ruby that is too old for Jekyll 4. Install a current one with Homebrew:

```bash
brew install ruby imagemagick
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
cd ~/PycharmProjects/website
bundle install
bundle exec jekyll serve
```

Then open <http://localhost:4000>. `jekyll serve` listens on `127.0.0.1` by default.

## Where things live

| What                                | File                                                                                |
| ----------------------------------- | ----------------------------------------------------------------------------------- |
| Bio, subtitle, buttons under photo  | `_pages/about.md`                                                                   |
| Email, Scholar, GitHub, LinkedIn    | `_data/socials.yml` (also hard-coded as buttons in `_pages/about.md`, keep in sync) |
| Publications                        | `_bibliography/papers.bib`                                                          |
| Selected publications on about page | `selected = {true}` in `papers.bib`                                                 |
| Project cards and pages             | `_projects/*.md` (`importance` sets the order)                                      |
| Profile photo                       | `assets/img/prof_pic.jpg` (3:4 crop, EXIF and GPS data stripped)                    |
| CV linked in the navbar             | `assets/pdf/cv.pdf` (website version of the CV, without phone number)               |
| Impressum and privacy notice        | `_pages/impressum.md` (linked in the footer via `impressum_path`)                   |
| Blue theme color, profile buttons   | `assets/css/main.scss` (local override, see below)                                  |
| News (turned off)                   | `_news/announcement_*.md` (see below)                                               |

### News

News is turned off.
`announcements.enabled: false` in `_pages/about.md` hides the block on the about page.
The `news` collection is removed from `_config.yml`, and `_pages/news.md` is in the `exclude` list.
To restore news, re-add the `news` collection block, drop the exclude entry, and set `announcements.enabled: true`.

### Bibliography fields

`papers.bib` is the CV bibliography with `\textbf` removed.
al-folio marks the own name through `scholar.last_name` and `scholar.first_name` in `_config.yml`.
Optional fields per entry: `abstract`, `preview` (image in `assets/img/publication_preview/`), `pdf`, `code`, `website`, `bibtex_show = {true}`.
The Altmetric and Dimensions badges are turned off in `_config.yml`, because their scripts load from third-party servers.
The `dimensions = {true}` fields in `papers.bib` therefore have no effect.

### Local override of the theme

al-folio v1 keeps layouts and styles in the `al_folio_core` gem.
This site overrides one gem file, `assets/css/main.scss`.
The file is the unmodified gem file (al_folio_core 1.0.15) plus a marked block at the end.
That block sets the blue theme color and styles the labelled buttons under the profile photo.
After a gem update, run `bundle exec al-folio upgrade overrides audit` to see whether the upstream file changed.

## Open items

- The email button, `_data/socials.yml`, and the Impressum use the university address. Replace it in all three places if that address expires.
- Add abstracts to the selected publications if you want the full look of the reference site.
- If news is turned back on, check the day in three news dates. The CV gives only the month for these, so they use the 1st: PhD start (Nov 2022), Kilometer 1 Idea Cup (Apr 2025), NII research stay (Apr 2026).
