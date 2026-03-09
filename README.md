name: Generate Snake Animation

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: Platane/snk@v3
        with:
          github_user_name: alienexa
          outputs: |
            assets/github-contribution-grid-snake.svg
            assets/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Commit and push snake to assets
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add assets/github-contribution-grid-snake.svg
          git add assets/github-contribution-grid-snake-dark.svg
          git diff --cached --quiet || git commit -m "chore: update snake animation"
          git push
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
