# :airplane: setup-preflight-action

Set up your GitHub Actions workflow with a specific version of [Preflight](https://github.com/spectralops/preflight).

## :bulb: Usage

Use the following to set up a `preflight` binary that's available in your workflow steps:

```yaml
      - name: Setup Preflight
        uses: spectralops/setup-preflight@v1
```

And then, use preflight. Turn this risky business:

```yaml
      - name: Install w/curl
        run:  curl -fsSL https://deno.land/x/install/install.sh | sh
```

Into this 1-2 karate chop:

:one:  On your mac (`brew tap spectralops/tap && brew install preflight`)

```
$ curl -fsSL https://deno.land/x/install/install.sh | preflight create
sha256=561aa9d6533ca228c43e784bf9fe8b74c8bfd8c443386dbf20bb714ac2969ceb
```

:two: In your workflow (you now have `preflight` available):

```yaml
      - name: Install w/curl protected with Preflight
        run:  curl -fsSL https://deno.land/x/install/install.sh | preflight run sha256=561aa9d6533ca228c43e784bf9fe8b74c8bfd8c443386dbf20bb714ac2969ceb
```




## :clipboard: Workflow

```yaml
name: run with preflight
on:
  push:
    branches:
      - master
      - main
  pull_request:

jobs:
  build:
    name: Build your code
    runs-on: ubuntu-latest

    steps:
      - name: Clone repo
        uses: actions/checkout@master


      # set up preflight
      - name: Setup Preflight
        uses: spectralops/setup-preflight@v1


      - name: Install w/curl protected with Preflight
        run:  curl -fsSL https://deno.land/x/install/install.sh | preflight run sha256=561aa9d6533ca228c43e784bf9fe8b74c8bfd8c443386dbf20bb714ac2969ceb
      
      - name: do stuff
        run: echo stuff
```
