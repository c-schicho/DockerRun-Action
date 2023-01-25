# DockerRun-Action

This is a simple action to run a docker image in a github action workflow. It only offers the bare minimum of options,
however, further options might be added in the future.

The following options are available:
 - `image` (required) - Docker image
 - `registry` - Container registry
 - `username` - Docker username
 - `password` - Docker password
 - `options` - Docker options
 - `run` - Run command in container

### How to use this action

It is recommended to set the credentials needed for this action as secrets, you can do this in your repository
settings.

An action to run a privately owned docker image could look like the following:

```yml
on:
  workflow_dispatch:

jobs:
  name: Run privately owned image
  runs-on: ubuntu-latest
  steps:
    - name: Checkout repository
      uses: actions/checkout@v3
    - name: Run Image
      uses: c-schicho/DockerRun-Action@v1
      with:
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}
        registry: ghcr.io
        image: image:latest
        options: -p 80:80
        run: echo "hello world"
```

An action to run a docker imaged built by a previous step could look like the following:

```yml
on:
  workflow_dispatch:

jobs:
  name: Run image built by previous step
  runs-on: ubuntu-latest
  steps:
    - name: Checkout repository
      uses: actions/checkout@v3
    - name: Build docker image
      ...
    - name: Run Image
      uses: c-schicho/DockerRun-Action@v1
      with:
        image: image:latest
        options: -p 80:80
        run: echo "hello world"
```