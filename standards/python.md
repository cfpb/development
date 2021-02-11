# Python Standards

- [Style](#style)
- [Linting](#linting)
- [Imports](#imports)
- [Dependency support and matrix testing](#dependency-support-and-matrix-testing)
  - [Considerations](#considerations)
  - [Specifying dependencies in `requirements.txt` or `setup.py`](#specifying-dependencies-in-requirementstxt-or-setuppy)

## Style

All of our Python code should match the [PEP8 style guide](https://www.python.org/dev/peps/pep-0008/) and the [Django coding style guidelines](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/).

We follow [PEP8's line length](https://www.python.org/dev/peps/pep-0008/#maximum-line-length) instead of Django's for consistency with our other development standards. We follow [Django's recommended import ordering in Django projects](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#imports).

We use [Black](http://black.readthedocs.io) to standardize formatting of our Python code and make linting easier. We override the default Black line length to 79 to match PEP8. We also exclude generated files like migrations. We provide a sample [`pyproject.toml` file containing line length and exclude configuration for Black](../pyproject.toml).

## Linting

For linting we recommend [flake8](http://flake8.pycqa.org/en/latest/), and provide a [sample flake8 configuration](../.flake8).
Exceptions to PEP8 are found in that [sample flake8 configuration](../.flake8), and include:

- `E731`, we allow assignment of `lambda` expressions
- `W503`, we allow line breaks after binary operators
- `W504`, we allow line breaks before binary operators

On the whole, generated Python files like Django migrations can be safely ignored.

The flake8 and configuration can go into a `tox.ini` file under a `[flake8]` header.

It is highly recommended to configure flake8 linting in your editor of choice to use the configuration file above or one provided in the project you're working on.

## Imports

See [Imports in PEP8](https://www.python.org/dev/peps/pep-0008/#imports) and [Imports in Django Coding Style](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#imports).

We use [isort](https://github.com/timothycrosley/isort) to lint imports to comply with both style guidelines, and provide a [sample isort configuration](../.isort.cfg). This configuration attempts to enforce the following conventions:

- Multiple-line `from` imports become grouped within parenthesis with one imported name per-line and a trailing comma for the last imported name.
- Django imports are included as their own section between the standard library and third-party imports.
- For Wagtail-specific projects like [consumerfinance.gov](https://github.com/cfpb/consumerfinance.gov), Wagtail imports are included as their own section between Django and third-party imports.

The isort configuration can go into a `tox.ini` file under an `[isort]` header.

## Dependency support and matrix testing

### Matrix testing

We recommend using [tox](https://tox.readthedocs.io/en/latest/) for matrix testing against our supported versions of our core dependencies, and we provide a [sample tox configuration](../tox.ini) that tests against both our current production the latest versions Python and Django.

When dealing with support for multiple versions of Python or major dependencies, we prefer to [use feature detection instead of version detection](https://docs.python.org/3/howto/pyporting.html#use-feature-detection-instead-of-version-detection) for incompatibilities/breaking changes that may exist between versions.

See [Tracking Django and Wagtail](../guides/tracking-django-wagtail.md) for more detailed guidance.

### Specifying dependencies in `requirements.txt` or `setup.py`

The difference between pinned versions of dependencies in `requirements.txt`  and dependencies specified in `setup.py` is frequently confusing and often misunderstood. The difference is between *concrete* and *abstract* dependencies, which is most easily understood in terms of applications (intended to be deployed and run), which specify *concrete* dependencies, and libraries (reusable code that may be included in applications), which specify *abstract* dependencies.

[Donald Stufft has an excelent article with a more in-depth discussion of this difference](https://caremad.io/posts/2013/07/setup-vs-requirement/). For our purposes, we use both requirements files and requirements specified in `setup.py` depending on the circumstance of the dependency.

Our applications should use requirements files with pinned versions. For example, [consumerfinance.gov has several requirements files](https://github.com/cfpb/consumerfinance.gov/tree/main/requirements) that pin all relevant versions for deployment, testing, local execution, and other specific needs. Some of these requirements files inherit from each other (e.g., [`base.txt` uses the `-r libraries.txt` directive](https://github.com/cfpb/consumerfinance.gov/blob/main/requirements/base.txt#L3) to include the requirements from that file).

Our libraries should specify dependencies with supported version ranges. The minimum version in the range and the last available version [should be tested with tox](../tox.ini) and can be major, minor, or patch). The maximum version that is expected to break compatibility can be a theoretical future major. A [tox test environment](../tox.ini) should be included that pulls in the latest version within that range and tests it. The range should be specified in `setup.py` using [the appropriate setuptools `_requires` keyword](https://setuptools.readthedocs.io/en/latest/setuptools.html#declaring-dependencies). For example, our [wagtail-sharing library](https://github.com/cfpb/wagtail-sharing) declares install-time requirements with `install_requires` and testing requirements with `extras_require={'testing': testing_extras}`.
