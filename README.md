# Fims Documentation

A wiki site of prototype project `Fims`.

[Link to the docs](https://fims-proto.github.io/documentations/).

## How to contribute

### 1. Install `mkdocs` and others

#### 1.1. Install `mkdocs`

``` shell
pip install mkdocs
```

(If pip is not found, consider export it):

``` shell
export PATH="$PATH:$HOME/Library/Python/3.9/bin"
```

#### 1.2. Install [mkdocs-material](https://squidfunk.github.io/mkdocs-material/) theme

``` shell
pip install mkdocs-material
```

`mkdocs-material` integrates with [mermaid](https://mermaid.js.org) natively, no need to install `mkdocs-mermaid2-plugin` plugin.  

### 2. Make your change in markdown files

For full documentation visit [mkdocs](https://www.mkdocs.org).

### 3. Verify locally

Run command to serve `mkdocs` locally:

``` shell
mkdocs serve
```

### 4. Deploy the docs

Commit the changes of source file into `main` branch.

Run command:

``` shell
mkdocs gh-deploy
```

`mkdocs` will generate site files in `/site` folder and push to `gh-pages` branch.

Now the changes should be visible in github pages: <https://fims-proto.github.io/documentations/>
