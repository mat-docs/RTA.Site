# ATLAS Integration Guide

URL: [docs.atlas.mclarenapplied.com/integration/rta/sdk/](https://docs.atlas.mclarenapplied.com/integration/rta/sdk/)  
Hosting: [mclaren-rta.netlify.app](https://mclaren-rta.netlify.app/)

[![Netlify Status](https://api.netlify.com/api/v1/badges/70188770-aad4-4cd1-addf-ce5a36b286e7/deploy-status)](https://app.netlify.com/sites/mclaren-rta/deploys)

## Setup

Install Visual Studio Code.

Install Python 3.7 or later, which should come with `pip`.

From the project directory:

    pip install -r requirements.txt

Preview the docs in a local web browser as follows:

    mkdocs serve

## Editing

The [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/) site has extensive documentation.

_mkdocs.yml_ has all the configuration options and defines the navigation tree.

* Choose filenames to result in a nice-looking public-facing URL
* Use _index.md_ files where it makes sense to be the landing page for an area
* Anything in _mkdocs.yml_ or _docs/_ results in an automatic refresh; anything outside needs an mkdocs restart

## Options

Use [navigation tabs](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/#navigation-tabs)
and [integrate the table of contents](https://squidfunk.github.io/mkdocs-material/setup/setting-up-navigation/#navigation-integration)
into the navigation tree to free up additional space.

```yaml
  features:
    - navigation.tabs
    - toc.integrate
```

Use `#` for the permalink indicator.

Turn off instant navigation. It's not really faster in practice, it's less reliable, and it probably won't play nice with site proxying.

## Style Guide

### Headings

Heading 1 is always the page title.

Use the other headings as required.

### Section Index Pages

Use an _index.md_ where there is a logical introductory page for a section.
A description of the basic concepts is often a good starting point.

Remember there is a navigation sidebar.
Don't make an index page just to repeat those links, unless you think users might not see important content &mdash;
which could happen if content is tucked into sub-folders.

### Links and Buttons

Make heavy use of links between pages &mdash; wiki-style.  
Exception: try not to hot-link too deeply into child sites.

Use buttons primarily for calls to action:

* Primary button if the action is important
* Secondary button if it's not so important

### Diagrams

Draw diagrams using _draw.io_ using the Sketch style.  
See RTA.Site repo for a sample file.

Export as SVG, and include in pages using the `<object>` tag to ensure fonts load.

### Tables

Use tables only for actual tabular data, not for layout.

Alternatives to consider:

* Simple lists
* Definition lists
* Tabbed sections
* Floating images (e.g. mouse gestures page)

### Child Sites

Use child sites (like RTA and Display API) where the topic is extensive (40+ pages) and would make sense on its own.  
Pros and Cons are a bit like maintaining source code in a library:

| Pros                                          | Cons                                                          |
| ----------------------------------------------| ------------------------------------------------------------- |
| Significantly faster edit/preview             | Feels less integrated to users                                |
| Can make full use of site structure (tabs)    | Deep-linking between sites needs to be limited                |
| Can be updated and versioned independently    | Netlify is designed for site-per-repo, so we have to proxy    |
| Easier to reorganise and bulk-edit            |                                                               |
