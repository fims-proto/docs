# Fims Documentation

A wiki site of prototype project `Fims`. [Link to the docs](https://fims-proto.github.io/documentations/).

## How to contribute

### Install `mkdocs` and others

Install `mkdocs`:

``` shell
pip install mkdocs
```

If pip is not found, consider export it:

``` shell
export PATH="$PATH:$HOME/Library/Python/3.9/bin"
```

Install [mkdocs-material](https://squidfunk.github.io/mkdocs-material/) theme:

``` shell
pip install mkdocs-material
```

Install plugin `mkdocs-mermaid2-plugin` to render [mermaid](https://mermaid.js.org):

``` shell
pip install mkdocs-mermaid2-plugin
```

### Make your change in markdown files

For full documentation visit [mkdocs](https://www.mkdocs.org).

### Verify locally

Run command to serve `mkdocs` locally:

``` shell
mkdocs serve
```

### Deploy the docs

Commit the changes of source file into `main` branch.

Run command:

``` shell
mkdocs gh-deploy
```

`mkdocs` will generate site files in `/site` folder and push to `gh-pages` branch.

Now the changes should be visible in github pages: <https://fims-proto.github.io/documentations/>
