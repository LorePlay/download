# LorePlay — landing / download

Strona pobierania LorePlay (VitePress + Vue 3 + SCSS).

## Dev

```bash
npm install
npm run dev
```

## Build

```bash
npm run build:site # build do .site-dist
npm run build      # build + sync do katalogu głównego (GitHub Pages)
```

Base URL: `/` w dev, `/download/` w produkcji; nadpisz zmienną `LANDING_BASE`.

## Struktura

- `channels/stable.json` — manifest wydania (wersja + linki do plików);
  `npm run sync:channels` kopiuje go do `site/public/channels/`.
- `site/index.md` — strona składana z `DownloadSection` i `MainContent`.
- `site/.vitepress/theme/components/ui/` — `AppButton`, `AppHeading`, `SectionCard`.
- `site/.vitepress/theme/styles/_app-button.scss` — styl przycisku pobierania.
