# install-koyeb-cli
An Action to install and configure koyeb cli

Example:
```
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  STACK_NAME: hello-world

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Install koyeb cli
        uses: koyeb-community/install-koyeb-cli@v1
        with:
          api_key: "${{ secrets.KOYEB_TOKEN }}"
          
      # Get data from koyeb
      - run: koyeb get all
      
      # Upload a new revision
      - run: echo 'functions: {}' > koyeb.yaml
      - run: koyeb create revision $STACK_NAME -f koyeb.yaml

```
