# Rust release action
This action creates nightly releases.

**WARNING: This action force pushes the `nightly` tag and overwrites the Nightly release! Use at your own risk!**

## Usage
```yaml
name: Rust

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  create-nightly-release:
    name: Create nightly
    if: github.ref == 'refs/heads/main'
    needs: [build]
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Consolidate artifacts
        uses: actions/download-artifact@v4
        with:
          pattern: '*-latest-nightly-binary'
          merge-multiple: true

      - name: Create release
        env:
          GH_TOKEN: ${{secrets.GITHUB_TOKEN}}
        uses: silicon-heaven/rust-nightly-release-action@v1.0.0
        with: '*-latest-nightly-binary'
```
