# kurtshuler.com

Source for [www.kurtshuler.com](https://www.kurtshuler.com), a bilingual (English and Spanish) blog built with [Hugo](https://gohugo.io) and the [Blowfish](https://blowfish.page) theme, deployed to GitHub Pages by the workflow in `.github/workflows/hugo.yaml`.

## Local preview

```bash
git clone --recurse-submodules https://github.com/kurtshuler/kurtshuler.com
cd kurtshuler.com
hugo server -D        # needs Hugo extended, same version as the workflow pins
```

## Writing a post

Each post is a page bundle holding both languages and their shared images:

```
content/posts/<slug>/
  index.md       English
  index.es.md    Spanish
  img/           images referenced by both
```

Front matter is mirrored between the two files except `title`, `description` and `summary`. Keep `date`, `slug` and `tags` identical so Hugo links the pair.

## Upgrading

```bash
git submodule update --remote --merge themes/blowfish   # read the Blowfish release notes first
```

Then bump `HUGO_VERSION` in the workflow if Blowfish requires it, and check the Hugo release notes for breaking changes.
