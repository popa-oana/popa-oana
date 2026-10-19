name: Generate Snake

on:
  schedule:
    - cron: "0 */12 * * *" # every 12 hours
  workflow_dispatch: # lets you run it manually from the Actions tab
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - uses: actions/checkout@v4

      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg?color_snake=#E75480&color_dots=#FFF0F5,#FFD1DC,#FFB7C5,#FF8FAB,#E75480
            dist/github-snake-dark.svg?palette=github-dark&color_snake=#FFB7C5&color_dots=#2B1A22,#5C2A3A,#9D4A65,#D4739A,#FFB7C5

      - name: Push to the output branch
        uses: crazy-max/ghaction-github-pages@v3.2.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
