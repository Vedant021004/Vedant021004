name: Generate Snake Contribution Graph

on:
  schedule:
    - cron: "0 0 * * *"   # runs once a day at midnight UTC
  workflow_dispatch: {}    # lets you trigger it manually from the Actions tab
  push:
    branches:
      - main               # regenerate whenever you push to main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: Generate snake SVG
        uses: Platane/snk@v3
        id: snake
        with:
          github_user_name: Vedant021004
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push SVG to the output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
