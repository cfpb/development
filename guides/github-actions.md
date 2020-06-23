# GitHub Actions recipes

We use [GitHub Actions](https://github.com/features/actions) for some of our contionuous integration and other automations. 
This guide is a collection of actions recipes to perform tasks that may be common across repositories. 

- [Running front-end unit tests](#running-front-end-unit-tests)
- [Running back-end unit tests with tox](#running-back-end-unit-tests-with-tox)
- [Fetching git history for setuptools-git-version](#fetching-git-history-for-setuptools-git-version)
- [Attaching a Python wheel file to a GitHub release](#attaching-a-python-wheel-file-to-a-github-release)
- [Publishing a Python package to PyPI on release](#publishing-a-python-package-to-pypi-on-release)
- [Publishing MkDocs documentation to GitHub pages](#publishing-mkdocs-documentation-to-github-pages)


## Running front-end unit tests 

```yml
jobs:

  frontend:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Set up Node
        uses: actions/setup-node@v1
        with:
          node-version: 10.x

      - name: Install Node dependencies
        run: |
          npm config set package-lock false
          npm install

      - name: Lint front-end code
        run: gulp lint

      - name: Run front-end unit tests
        run: npm test
```

## Running back-end unit tests with tox

```yml
jobs:
  backend:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v1
        with:
          python-version: 3.6

      - name: Install Python dependencies
        run: |
          python -m pip install --upgrade pip
          pip install tox

      - name: Run back-end tests
        run: |
          tox
```

## Fetching git history for setuptools-git-version

Some of our consumerfinance.gov satellite apps use [setuptools-git-version](https://github.com/pyfidelity/setuptools-git-version) to set the Python package version from the latest git tag. This requires tags to be available in the checkout in the GitHub action: 

```yml
    steps:
      - uses: actions/checkout@v2

      - name: Fetch tags and commits needed for setuptools-git-version
        run: |
          git fetch --depth=1 origin +refs/tags/*:refs/tags/*
          git fetch origin ${{ github.head_ref }} && git checkout ${{ github.head_ref }}
```

The intention is for the command `git describe --tags --long --dirty` to succeed.

## Attaching a Python wheel file to a GitHub release

Some of our consumerfinance.gov satellite apps have a Python wheel package file attached to their GitHub releases.

```yml
name: Publish 

on: 
  release:
    types: [published]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1

      # If the package has a front-end build we need to set up and install 
      # Node and other dependencies
      - name: Set up Node
        uses: actions/setup-node@v1
        with:
          node-version: 10.x

      - name: Install Node dependencies
        run: |
          npm install -g gulp-cli
          npm config set package-lock false

      - name: Set up Python
        uses: actions/setup-python@v1
        with:
          python-version: 3.6

      - name: Install Python dependencies
        run: python -m pip install --upgrade pip wheel

      - name: Build the packages
        id: build
        run: |
            python setup.py sdist bdist_wheel
            # Get the name of the .whl and .tar.gz files and set them as 
            # "outputs" of this step so we can upload them
            echo "::set-output name=bdist_wheel::$(cd dist && ls *.whl)"
            echo "::set-output name=sdist::$(cd dist && ls *.tar.gz)"

      - name: Upload the wheel
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ github.event.release.upload_url }}
          asset_path: dist/${{ steps.build.outputs.bdist_wheel }}
          asset_name: ${{ steps.build.outputs.bdist_wheel }}
          asset_content_type: application/zip

      - name: Upload the source distribution
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ github.event.release.upload_url }}
          asset_path: dist/${{ steps.build.outputs.sdist }}
          asset_name: ${{ steps.build.outputs.sdist }}
          asset_content_type: application/gzip
```

## Publishing a Python package to PyPI on release

Some of our libraries get [published to PyPI](pypi.md). 
This requires `TWINE_USERNAME` and `TWINE_PASSWORD` set as repository secrets.

```yml
name: Publish to PyPI
on: 
  release:
    types: [published]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:

    - uses: actions/checkout@v1

    - name: Set up Python
      uses: actions/setup-python@v1
      with:
        python-version: 3.8

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install twine wheel

    - name: Build the package
      run: |
        python setup.py sdist bdist_wheel --universal

    - name: Upload to PyPI
      run: twine upload dist/*
      env: 
        TWINE_USERNAME: ${{ secrets.TWINE_USERNAME }}
        TWINE_PASSWORD: ${{ secrets.TWINE_PASSWORD }}
```

## Publishing MkDocs documentation to GitHub pages

```yml
on: 
  push:
    branches:
      - master

jobs:

  publish:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - run: |
          git fetch --no-tags --prune --depth=1 origin gh-pages

      - name: Set up Python
        uses: actions/setup-python@v1
        with:
          python-version: 3.6

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r mkdocs

      - name: Build docs
        run: mkdocs build

      - name: Publish docs
        run: mkdocs gh-deploy
```
