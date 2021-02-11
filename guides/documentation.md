# Documenting your work

Good development practices include writing and maintaining documentation that covers the software being written.

The Write the Docs
[Documentation Principles](https://www.writethedocs.org/guide/writing/docs-principles/)
are recommended reading and should be kept in mind when writing documentation at CFPB.

The WTD [Documentation Guide](https://www.writethedocs.org/guide/) also provides an excellent writeup on
[why writing documentation is important](https://www.writethedocs.org/guide/writing/beginners-guide-to-docs/#why-write-docs),
best processes for writing and maintaining docs, and tips for building a documentation culture in an organization.

## Table of Contents

1. [Where to put documentation](#where-to-put-documentation)
1. [Documentation definition of done](#documentation-definition-of-done)
1. [Recommendations for good documentation](#recommendations-for-good-documentation)

## Where to put documentation

In line with the CFPB [Source Code Policy](https://github.com/cfpb/source-code-policy/),
documentation should be made public and open source where possible.
Exceptions to this policy include documentation containing sensitive or internal data or processes.
CFPB contributors should consult their management for guidance on where to locate private documentation.

Per the [Nearby](https://www.writethedocs.org/guide/writing/docs-principles/#nearby) principle of documentation,
docs should be stored as close as possible to the code that they document.

General standards and guides about software development should live in this repository
([https://github.com/cfpb/development](https://github.com/cfpb/development)).
This documentation is written in Markdown and browseable
[on GitHub](https://github.com/cfpb/development#cfpb-development-guidelines).

Documentation specific to [https://www.consumerfinance.gov](https://www.consumerfinance.gov)
or the software that powers it should live in the [consumerfinance.gov](https://github.com/cfpb/consumerfinance.gov) repository.
Documentation is written as [Markdown files](https://github.com/cfpb/consumerfinance.gov/tree/main/docs)
in that repository, converted to HTML by [MkDocs](https://www.mkdocs.org/),
and served by GitHub Pages at [https://cfpb.github.io/consumerfinance.gov/](https://cfpb.github.io/consumerfinance.gov/).

Documentation specific to other projects should live in that project's repository.
Exceptions to this include repositories that only work within the context of a larger project,
for example the cf.gov satellite apps.
In that case it may be appropriate to locate documentation within the consumerfinance.gov repository,
as long as the satellite app contains a link pointing to the appropriate location.

## Documentation definition of done

Any new or modified documentation should meet the following criteria before being published:

1. _Thoroughly reviewed:_
Ideally, documentation should be peer-reviewed by two people:
one who is familiar with the material being covered, and one who is new to the material.
This will ensure that documentation is appropriate for developers with different experience levels.

1. _Tested:_
Any documentation that describes a process (for example, setting up a project)
should be tested fully by a developer to verify its correctness.
Consider defining and using user stories to guide this process
(for example, "as a new member of the team, I need to get cf.gov running locally on my machine").

1. _Properly located:_
Documentation should be located in an appropriate place.
See ["Where to put documentation"](#where-to-put-documentation) above for additional detail.

1. _Properly discoverable:_
Documentation should be linked to from an appropriate landing page
(for example, [this repository's README](https://github.com/cfpb/development/blob/main/README.md))
and, if possible, be discoverable through search
(for example, [the consumerfinance.gov docs search](https://cfpb.github.io/consumerfinance.gov/search.html?q=testing)).

The onboarding of new team members can be a great opportunity to review and
update existing documentation to ensure that it meets these goals.

## Recommendations for good documentation

Source code repositories should contain a README that provides an overview of
what they contain. See the
[CFPB Open Source Project Template](https://github.com/cfpb/open-source-project-template)
for a recommended example structure.

Installation instructions should clearly present what a user might expect upon
successful completion of a setup process.
For example, include expected console output or screenshots of webpages or graphical interfaces.
When setup instructions include the running of console commands,
make sure to specify within which directory those commands should be run.

Make sure that project documentation includes information on how to install any
required third-party dependencies.
Be as specific as possible about the required versions of any external dependencies.

When referencing external links, try to make the references as specific as possible,
for example by using anchor links
([like this](https://github.com/cfpb/development/#guides)) to target specific places on the page.
When referencing specific lines in source code, consider linking to specific
releases or commit hashes instead of the main branch
([this](https://github.com/cfpb/consumerfinance.gov/blob/7.2.2/tox.ini#L105) or
[this](https://github.com/cfpb/consumerfinance.gov/blob/fb16e906bc4935669f880c270ccf4e32b930b068/tox.ini#L105)
instead of
[this](https://github.com/cfpb/consumerfinance.gov/blob/main/tox.ini#L105))
when the file being linked to is likely to change often and a specific line reference would be hard to maintain.
Links to the main branch of source code should be used as long as the
documentation is updated appropriately when the source files are.
