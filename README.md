# AWS CodeArtifact login Action

Log into [AWS CodeArtifact](https://aws.amazon.com/codeartifact/) using AWS credentials and get 
a temporary token for usage with Python package managers such as `pip` and `poetry`.

Usage:

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  do-stuff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        id: setup-python
        uses: actions/setup-python@v2
        with:
          python-version: "3.9.10"

      - name: Install Poetry
        uses: snok/install-poetry@v1
        with:
          virtualenvs-create: true
          virtualenvs-in-project: true
          installer-parallel: true
    
      - name: Authenticate to CodeArtifact
        uses: source-ag/codeartifact-login-action@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-central-1
          role-to-assume: ${{ secrets.PACKAGE_REPOSITORY_ROLE }}
          codeartifact-domain: my-domain
          codeartifact-domain-owner: 123456789012
          codeartifact-repository: my-python-packages
          configure-poetry: true

      - name: Install dependencies
        run: poetry install
```
