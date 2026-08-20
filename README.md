# Save this file at: .github/workflows/snake.yml
# It generates the contribution-snake SVG and pushes it to an "output" branch.

name: Generate Snake Animation

on:
  schedule:
    - cron: "0 */12 * * *"   # runs twice a day
  workflow_dispatch:          # lets you run it manually
  push:
    branches:
      - main

jobs:
  generate:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Generate snake SVGs
        uses: Platane/snk@v3
        id: snake-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg
            dist/snake-dark.svg?palette=github-dark

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
