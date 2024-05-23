# Contributing

We have adopted the following style guidelines:
 - Use `kebab-case` file names, as they are used when forming the documentation URLs
   * Lower case with a `-` to separate words
 - Place no more than one sentence per source line whenever allowed by markdown syntax
   * Table rows have to be crammed into a single line
   * One sentence can be spread over multiple source lines if it is long

## Deploying docs to Github Pages Site

We're currently using GitHub Actions to deploy to Github Pages.
The docs on the master branch are automatically deployed to the documentation site.

## Building the Docs Locally

Built with [mkdocs](https://www.mkdocs.org/#mkdocs)

You'll need a working python environment to get started.

```
pip install mkdocs
```

To build the static site in `./site`:
```
mkdocs build
```

To serve the site, with live reloading:
```
mkdocs serve
```
This will be served at localhost:8000 by default
