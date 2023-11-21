# install-koyeb-cli

This action to install and configure Koyeb CLI.

## Usage

### Inputs

| Input              | Required | Description                                                                 |
|--------------------|:--------:|-----------------------------------------------------------------------------|
| api_token          | [x]      | Your Koyeb API token. See https://app.koyeb.com/account/profile.            |
| github_token       | [ ]      | A github API token allowing to read public repositories (can be read-only). |
| api_url            | [ ]      | The API URL. Defaults to https://app.koyeb.com                              |

### How to use

The example below checks out a copy of your repository, build & push the Docker to your GitHub Container Registry and trigger a new deployment of your application on Koyeb.

```yaml
name: Deploy to Koyeb
on: [push]

env:
  GHCR_TOKEN: ${{ secrets.GHCR_TOKEN }}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Docker build
        run: docker build --rm=false -t ghcr.io/<YOUR_GITHUB_USERNAME>/<YOUR_DOCKER_IMAGE_NAME>:<YOUR_DOCKER_IMAGE_TAG> .
      - name: Docker login
        run: echo $GHCR_TOKEN | docker login ghcr.io -u <YOUR_GITHUB_USERNAME> --password-stdin
      - name: Docker push
        run: docker push ghcr.io/<YOUR_GITHUB_USERNAME>/<YOUR_DOCKER_IMAGE_NAME>:<YOUR_DOCKER_IMAGE_TAG>
      - name: Install and configure the Koyeb CLI
        uses: koyeb-community/install-koyeb-cli@v2
        with:
          api_token: "${{ secrets.KOYEB_TOKEN }}"
          github_token: "${{ secrets.GITHUB_TOKEN }}"
      - name: Deploy to Koyeb
        run: koyeb service redeploy app_name/service_name
```
