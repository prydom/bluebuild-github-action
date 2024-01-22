# BlueBuild GitHub Action

This reusable Action can be used to build custom operating system images using BlueBuild tools.

```yaml
# build.yml
name: bluebuild
on:
  schedule:
    - cron: "30 16 * * *"
  push:
    branches:
      - live
      - template
      - main
    paths-ignore:
      - "**.md"
  pull_request:
  workflow_dispatch:
jobs:
  bluebuild:
    name: Build Custom Image
    runs-on: ubuntu-22.04
    permissions:
      contents: read
      packages: write
      id-token: write
    strategy:
      fail-fast: false
      matrix:
        recipe:
          # !! Add your recipes here 
          - recipe.yml
    steps:
      - name: Build Custom Image
        uses: blue-build/github-action@v1
        with:
          recipe: ${{ matrix.recipe }}
          cosign_private_key: ${{ secrets.SIGNING_SECRET }}
          registry_token: ${{ github.token }}
          pr_event_number: ${{ github.event.number }}
```