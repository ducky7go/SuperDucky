# SuperDucky (public README repo)

## Project overview

This repo contains only `README.md` and `assets/img/` (16 UI screenshots). The README is a short gallery of the SuperDucky UI, a QQ group link ("逃离鸭科夫 mod 技术交流") and a link to the docs site. There is no source code, build system or CI here.

The application source lives in the sibling repo `ducky7go/DML` (its `client/` directory; product name SuperDucky). Releases are published here: `DML/client/electron-builder.yml` publishes to `ducky7go/SuperDucky`, and the app's default update source is `ducky7go/SuperDucky`. The Releases themselves are not visible in a clone.

## Working here

- Edits are limited to `README.md` and screenshots in `assets/img/`. Screenshots are named `Screenshot_YYYYMMDD_HHMMSS.png`. Each new screenshot needs a matching `![Screenshot YYYYMMDD HHMMSS](assets/img/...)` line in the README.
- Do not add code or docs for the app here: user docs go to `SuperDucky.docs`, app code to `DML`.
- The README docs link (`https://ducky7go.github.io//SuperDucky.docs/`) has a double slash and points at the old GitHub Pages address. The docs site is now served from `https://ducky.hagicode.com/` (`SuperDucky.docs` has no base path), so the link is probably stale.

## Related repositories

- `DML`: SuperDucky application source (Electron + React).
- `SuperDucky.docs`: Astro/Starlight documentation site (Chinese source, English under `/en/`).
- `SuperDucky.Connector`: companion game mod used by the app's save gallery.
