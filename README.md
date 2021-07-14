# install-koyeb-cli
An Action to install and configure Koyeb cli

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
      - name: Install and configure Koyeb cli
        uses: koyeb-community/install-koyeb-cli@v2
        with:
          api_token: "${{ secrets.KOYEB_TOKEN }}"
          
      - name: Redeploy service
        run: koyeb services redeploy --app=app-name service-name
```
