# Tracking Django and Wagtail

- [Philosophy](#philosophy)
- [Considerations when supporting multiple versions](#considerations-when-supporting-multiple-versions)
  - [Consult the release notes](#consult-the-release-notes)
  - [Use feature detection](#use-feature-detection)
  - [Imports](#imports)
  - [Version detection](#version-detection)
  - [Matrix testing](#matrix-testing)
- [Upgrade process](#upgrade-process)
  - [Ensure libraries and satellite apps support required versions](#ensure-libraries-and-satellite-apps-support-required-versions)
  - [Ensure consumerfinance.gov supports required versions](#ensure-consumerfinancegov-supports-required-versions)
  - [Upgrading consumerfinance.gov](#upgrading-consumerfinancegov)

## Philosophy

We have two general types of Python projects:

- The running [consumerfinance.gov](https://github.com/cfpb/consumerfinance.gov) website
- Satellite applications of consumerfinance.gov that live in their own repository, e.g., our satellite apps of consumerfinance.gov, like [ccdb5-ui](https://github.com/cfpb/ccdb5-ui)
- Libraries that provide some functionality independent of consumerfinance.gov, e.g., [wagtail-sharing](https://github.com/cfpb/wagtail-sharing)

We use a rubric like the one below to determine which versions of Python and major dependencies should be supported.

|| Library | consumerfinance.gov | Satellite application |
|--|--|--|--|
|Oldest Python 3 supported by Django LTS | ✔️| | |
|Current production Python |  | ✔️ | ✔️|
|Latest Python | ✔️ | ✔️ | ✔️|
|Latest Django LTS version | ✔️ | ✔️ | ✔️|
|Latest Django version |  ✔️ | ✔️ | ✔️|
|Latest Wagtail LTS version | ✔️ | ✔️ | ✔️|
|Latest Wagtail version |  ✔️ | ✔️ | ✔️|

consumerfinance.gov and its satellite apps should support the version of Python 3 we currently run in production and the latest version. At the time of writing that is version 3.6 and 3.8.

Our libraries should support the oldest version of Python 3 that the library's major dependencies supports, and the latest version of Python. For example, [Django 2.2 supports Python 3.5 and up](https://docs.djangoproject.com/en/2.2/releases/2.2/#python-compatibility), so our Django and Wagtail libraries that support Django 2.2 should do the same.

Both consumerfinance.gov and its satellite apps, and our libraries should support the LTS (long-term support) version of Django and Wagtail that we currently run in production, and the latest version (LTS or not) of Django and Wagtail.

## Considerations when supporting multiple versions

### Consult the release notes

Django and Wagtail are generally very good about listing upgrading considerations, deprecations, and breaking changes in their release notes. The release notes should drive the changes that we make.

### Use feature detection

When dealing with support for multiple versions of Python or major dependencies, we prefer to [use feature detection instead of version detection](https://docs.python.org/3/howto/pyporting.html#use-feature-detection-instead-of-version-detection) for incompatibilities/breaking changes that may exist between versions. Feature detection `try`/`except` blocks should always attempt to use **new** code first and then fall back on old code.

### Imports

To detect features on import, or import paths have changed between versions, we prefer to use `try`/`except` to import the **new** version and fall back on the old version:

```python
try:
    # Django >= 2.0
    from django.urls import include
except ImportError:
    # Django < 2.0
    from django.conf.urls import include
```

### Version detection

Only when [feature-detection](https://docs.python.org/3/howto/pyporting.html#use-feature-detection-instead-of-version-detection) is not sufficient to support multiple versions of a dependency do we fall back on version-checking. This can happen when APIs change but the naming conventions remain the same, and exception-handling is insufficient or too greedy to detect the difference between an API change and another reason an execption may be raised.

When using version detection, we prefer `if`/`else` conditionals with the **new** API used in the `if`, falling back on the old API in the `else`. For example:

```python
import django

if django.VERSION >= (2, 0, 0):
    is_anonymous = bool(request.user.is_anonymous)
else:
    is_anonymous = bool(request.user.is_anonymous())
```

When performing a version check, the hard-coded version should always be **inclusive** of the first version to support the new API and should specify the entire semantic version.

Both Django and Wagtail provide versions as tuples of (major version, minor version, patch version, sub-version, sub-version release). For example:

```python
>>> import django
>>> django.VERSION
(2, 2, 13, 'final', 0)
```

Version-detection of Django and Wagtail should specify `(major, minor, patch)`.

### Matrix testing

We use [tox](https://tox.readthedocs.io/en/latest/) for matrix testing against all the versions listed above, and we provide a [sample tox configuration](../tox.ini) that tests against both our current production the latest versions Python and Django suitable as a basis for satellite apps and libraries.

## Upgrade process

The upgrade process for consumerfinance.gov is fairly straight-forward, and reasonably similar for all three categories of projects we maintain.

### Ensure libraries and satellite apps support required versions

1. Add support for the new version(s) to the `tox` matrix
2. Consult release notes of the version(s) being added and make necessary code changes
3. Fix any tests that break as a result
4. Ensure that running functionality works as expected
5. Clean up support for any previous versions that can be dropped
6. Create a new release

New releases of libraries should be automatically uploaded to PyPI by our GitHub Actions.

New releases of satellite apps should be automatically added to their GitHub release page by our GitHub Actions.

### Ensure consumerfinance.gov supports required versions

consumerfinance.gov's [tox.ini](https://github.com/cfpb/consumerfinance.gov/blob/main/tox.ini) matrixes on "current" vs "future" versions rather than on specific version numbers, and unlike our satellite apps and libraries pins its dependencies to specific versions. This can create incompatibilities that we need to deal with.

1. Add support for the new version(s) to the `tox` `future-config` matrix configuration.

   Include new pins of any required libraries. For example, if a future Wagtail requires an update to BeautifulSoup that is incompatible with our current Wagtail's dependency on BeautifulSoup, pin it in the tox `future-config` `deps`:

   ```ini
   [future-config]
   …
   deps=
       Django>=2.2,<3.0
       wagtail>=2.10,<2.11
       beautifulsoup4>=4.8.2
   ```

2. Pin any new releases of satellite apps or libraries in the [requirements](https://github.com/cfpb/consumerfinance.gov/blob/main/requirements/libraries.txt) file.
3. Consult release notes of the version(s) being added and make necessary code changes
4. Fix any tests that break as a result
5. Ensure that running functionality works as expected
6. Clean up support for any previous versions that can be dropped

### Upgrading consumerfinance.gov

When we have a new LTS version of Django, or when there's a new release of Wagtail to deploy, the process of upgrading consumerfinance.gov should be simple, because we already have been tracking support for those versions.

1. Pin [Django](https://github.com/cfpb/consumerfinance.gov/blob/main/requirements/django.txt) and [Wagtail](https://github.com/cfpb/consumerfinance.gov/blob/main/requirements/wagtail.txt) in their respective requirements files.
2. Update the pins of any libraries that we pinned in the tox `future-config` `deps` in the [libraries](https://github.com/cfpb/consumerfinance.gov/blob/main/requirements/libraries.txt) requirements file.
