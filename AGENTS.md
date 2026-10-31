# SuperDucky (public README repo)

## Project overview

This repo currently contains only `README.md` and `assets/img/` (16 UI screenshots). The README is a short gallery of the SuperDucky UI, a QQ group link ("逃离鸭科夫 mod 技术交流") and a link to the docs site `https://ducky7go.github.io//SuperDucky.docs/`. There is no source code, build system, CI or openspec directory here.

The application source lives in the sibling repo `ducky7go/DML` (its `client/` directory; product name SuperDucky). Verifiable link: `DML/client/electron-builder.yml` publishes releases to `ducky7go/SuperDucky`, and the app's default update source is `ducky7go/SuperDucky`. So this repo is where release artifacts and issues for the app are expected to land; the Releases themselves are not visible in a clone.

## Working here

- Edits are limited to `README.md` and screenshots in `assets/img/` (named `Screenshot_YYYYMMDD_HHMMSS.png`).
- Do not add code or docs for the app here; user docs go to `SuperDucky.docs`, app code to `DML`.
- The README docs link contains a double slash (`ducky7go.github.io//SuperDucky.docs/`); the docs site `baseUrl` is `/SuperDucky.docs/`.

## Related repositories

- `DML` - SuperDucky application source (Electron + React).
- `SuperDucky.docs` - Docusaurus documentation site (zh-Hans default, en).
- `SuperDucky.Connector` - companion game mod used by the app's save gallery.
