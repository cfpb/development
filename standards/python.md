# Python Guide

1. [Linting](#linting)
1. [Imports](#imports)
3. [Python versions](#python-versions)

## Linting

All of our Python code should match the [PEP8 style guide](https://www.python.org/dev/peps/pep-0008/) and the [Django coding style guidelines](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/). 

We follow [PEP8's line length](https://www.python.org/dev/peps/pep-0008/#maximum-line-length) instead of Django's for consistency with our other development standards. We follow [Django's recommended import ordering in Django projects](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#imports).

For linting we recommend [flake8](http://flake8.pycqa.org/en/latest/), and provide a [sample flake8 configuration](../.flake8).
Exceptions to PEP8 are found in that [sample flake8 configuration](../.flake8), and include:

- `E731`, we allow assignment of `lambda` expressions
- `W503`, we allow line breaks after binary operators

On the whole, generated Python files like Django migrations can be safely ignored. 

The flake8 and configuration can go into a `tox.ini` file under a `[flake8]` header.

It is highly recommended to configure flake8 linting in your editor of choice to use the configuration file above or one provided in the project you're working on.

## Imports

See [Imports in PEP8](https://www.python.org/dev/peps/pep-0008/#imports) and [Imports in Django Coding Style](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#imports).

We use [isort](https://github.com/timothycrosley/isort) to lint imports to comply with both style guidelines, and provide a [sample isort configuration](../.isort.cfg). This configuration attempts to enforce the following conventions:

- Multiple-line `from` imports become grouped within parenthesis with one imported name per-line and a trailing comma for the last imported name.
- Consider `six` to be equivalent to `__future__` imports.
- Django imports are included as their own section between the standard library and third-party imports.
- For Wagtail-specific projects like [cfgov-refresh](https://github.com/cfpb/cfgov-refresh), Wagtail imports are included as their own section between Django and third-party imports.

The isort configuration can go into a `tox.ini` file under an `[isort]` header.

## Python versions

As we transition from Python 2.7 to Python 3 it is highly recommended to test against both Python 2.7 and Python 3.6. 

We recommend using [tox](https://tox.readthedocs.io/en/latest/) for matrix testing, and provide a [sample tox configuration](../tox.ini) that tests against Python 2.7 and 3.6 and Django LTS versions 1.8 and 1.11.

For writing code that is Python 2 and 3-compatible, please see the [Python Porting HOWTO](https://docs.python.org/3/howto/pyporting.html) and the [Django Porting to Python3](https://docs.djangoproject.com/en/1.11/topics/python3/). In particular, we prefer to:

- [Use feature detection instead of version detection](https://docs.python.org/3/howto/pyporting.html#use-feature-detection-instead-of-version-detection)
- Use absolute imports (`from __future__ import absolute_import`)
- Use the `print()` function instead of the `print` statement (`from __future__ import print_function`)
- Use unicode literals (`from __future__ import unicode_literals`), and [the corresponding Django `python_2_unicode_compatible`](https://docs.djangoproject.com/en/1.11/ref/utils/#django.utils.encoding.python_2_unicode_compatible) (`from django.utils.encoding import python_2_unicode_compatible`)
- Use Python 3 division wherein dividing `int`s results in a `float` (`from __future__ import division`)
- Capture exceptions using the `as` keyword (`except Exception as exc`)
