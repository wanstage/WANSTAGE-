name: Node.js Package

on:
  release:
    types: [created]
  workflow_dispatch:

permissions:
  contents: read
  packages: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm test

  publish-npm:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Publish to 
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.GH_PACKAGES_TOKEN }}
          git add .github/workflows/publish.yml
git commit -m "ci: fix workflow to publish to GitHub Packages"
git push
  - uses: actions/setup-node@v4
        with:
          node-version: 20
          registry-url: https://npm.pkg.github.com/
          scope: '@wanstage'
          always-auth: true

      - name: npm ci (依存取得は公開npmだけならそのまま)
        run: npm ci

      - name: Publish to GitHub Packages
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        git add .github/workflows/publish.yml
git commit -m "ci: publish to GitHub Packages via GITHUB_
