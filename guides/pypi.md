# Publishing Python packages to PyPI

PyPI - the Python Package Index - is a repository of software for the Python programming language. The production instance of PyPI lives at https://pypi.org.

As a developer, when you type the command `pip install something`, by default that `something` comes from PyPI. When we create Python packages that might be generally useful to the public (for example, Wagtail plugins), it's nice to consider publishing them to PyPI so others can easily find and use them.

## Authoring

Before trying to create a new package for PyPI, it might be useful to review the [official Python documentation](https://packaging.python.org/tutorials/distributing-packages/) about packaging and distributing projects.

Publishing a package generally consists of first creating a distribution file from your source code, using a `setup.py` file, and then
publishing that distribution file to PyPI, using [twine](https://pypi.python.org/pypi/twine).

(In the past, these steps could be accomplished with a single command - `python setup.py upload` - [but this is no longer recommended for security reasons](https://github.com/pypa/twine#why-should-i-use-this).)

A simple set of commands to accomplish this looks like:

1. `python setup.py sdist bdist_wheel` (creates both a source distribution and a [wheel archive](https://packaging.python.org/tutorials/distributing-packages/#wheels) of your project, and places them in a `dist` subdirectory.
2. `twine upload dist/*` (pushes your new distribution files to PyPI - this assumes the directory was new/empty before you ran the previous command)

The contents of your project's `setup.py` file determine how your code is packaged and how it gets published on PyPI. There are a number of useful and important fields that you should include in this file:

- `url`: link to GitHub repository.
- `author`: should be CFPB.
- `author_email`: should be tech@cfpb.gov.
- `description`: short, one line description of the package. This is the headline that appears in PyPI [search results](https://pypi.org/search/?q=wagtail-sharing) and as the headline on the package page.
- `long_description`: lengthier description that appears on PyPI. By default this content must be in [reStructuredText](http://docutils.sourceforge.net/rst.html) format. One simple way to implement this is to read in your package's `README.rst` file [like wagtail-sharing does it](https://github.com/cfpb/wagtail-sharing/blob/main/setup.py#L34).
- `long_description_content_type`: Optionally, if you prefer to write your README in Markdown, you can set this field to `'text/markdown'`.
- `classifiers`: this should consist of the list of PyPI trove classifiers that apply to your package, for example the Python and Django versions with which it is compatible. This ensures that your package is properly [searchable on PyPI](https://pypi.org/search/). See the full list of classifiers [here](https://pypi.python.org/pypi?%3Aaction=list_classifiers).
  - This list should always include proper licensing information for your package. This should typically consist of both `'License :: Public Domain'` and `'License :: CC0 1.0 Universal (CC0 1.0) Public Domain Dedication'` due to our default dual licensing described [here](https://github.com/cfpb/development/blob/main/TERMS.md).
  - When possible, licensing classifiers should be used instead of a `license` field in your `setup.py` file. See documentation [here](https://docs.python.org/3.6/distutils/setupscript.html#additional-meta-data) that states (emphasis added):

     > The license field is a text indicating the license covering the package _where the license is not a selection_ from the “License” Trove classifiers.

See also [the wagtail-sharing repository](https://github.com/cfpb/wagtail-flags/blob/main/setup.py) for a good example of a well-written `setup.py` file.

## Publishing

When publishing a package developed at CFPB, we ask that you do the following:

- Make sure the project is hosted in a CFPB GitHub repository.
- Publish the project to PyPI using your personal account.
- Add the [cfpb](https://pypi.org/user/cfpb/) PyPI user as an owner of the project. This ensures that someone from the organization will always be able to update the package.

One gotcha: once you've uploaded a particular archive filename to PyPI, [you can never ever upload that same filename again](https://github.com/pypa/packaging-problems/issues/74). This means that if you upload, say, a version 1.0.2, and after publishing realize there was some error, you can't delete and re-upload a new corrected archive as version 1.0.2. You have to instead publish the corrected archive as version 1.0.3.

## Publishing Python Packages Automatically with Travis

It is possible to [publish your packages automatically with Travis](https://docs.travis-ci.com/user/deployment/pypi/) when you create a GitHub release. To do this you must add the following to your `.travis.yml` file:

```yml
deploy:
  provider: pypi
  user: cfpb
  password:
    secure: <encrypted password for cfpb>
  distributions: "sdist bdist_wheel --universal"
  on:
    tags: true
```

This tells Travis to publish both an `sdist` and a universal `bdist_wheel` distribution to PyPI as the user `cfpb` with the given password (encrypted using `travis encrypt`) whenever a new git tag is created, which GitHub does when creating a new release.

If you have multiple targets you can add conditions to the `on` value, for example:

```yml
on:
  tags: true
  condition: $TOXENV = "py36-dj111-wag113"
```

This will avoid matrix failures where an distribution cannot be uploaded to PyPI for a release more than once. Note: `condition` requires a string that evaluates to `true` in bash.

Additionally, if you want to test your release, Travis configuration, or anything else about the upload process to PyPI, you can have Travis upload to [TestPyPI])https://test.pypi.org) by specifying the server:

```yml
server: https://test.pypi.org/legacy/
```
