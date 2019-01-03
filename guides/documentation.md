# Documenting your work

Good development practices include writing and maintaining documentation that covers the software being written.

The Write the Docs [Documentation Guide](https://www.writethedocs.org/guide/) provides an excellent writeup on [why writing documentation is important](https://www.writethedocs.org/guide/writing/beginners-guide-to-docs/#why-write-docs), best processes for writing and maintaining docs, and tips for building a documentation culture in an organization.

The WTD [Documentation Principles](https://www.writethedocs.org/guide/writing/docs-principles/) are also recommended reading and should be kept in mind when writing documentation at CFPB.

## Where to put documentation

In line with the CFPB [Source Code Policy](https://github.com/cfpb/source-code-policy/), documentation should be made public and open source where possible. Exceptions to this policy include documentation containing sensitive or internal data or processes. CFPB contributors should consult their management for guidance on where to locate private documentation.

Per the [Nearby](https://www.writethedocs.org/guide/writing/docs-principles/#nearby) principle of documentation, docs should be stored as close as possible to the code that they document.

General standards and guides about software development should live in
this repository
([https://github.com/cfpb/development](https://github.com/cfpb/development)). This documentation is written in Markdown and browseable [on GitHub](https://github.com/cfpb/development#cfpb-development-guidelines).

Documentation specific to
[https://www.consumerfinance.gov](https://www.consumerfinance.gov) or
the software that powers it should live in the [cfgov-refresh](https://github.com/cfpb/cfgov-refresh) repository. Documentation is written as [Markdown files](https://github.com/cfpb/cfgov-refresh/tree/master/docs) in that repository, converted to HTML by [MkDocs](https://www.mkdocs.org/), and served by GitHub Pages at [https://cfpb.github.io/cfgov-refresh/](https://cfpb.github.io/cfgov-refresh/).

Documentation specific to other projects should live in that project's repository. Exceptions to this include repositories that only work within the context of a larger project, for example the cf.gov satellite apps. In that case it may be appropriate to locate documentation within the cf.gov repository, as long as the satellite app contains a link pointing to the appropriate location.

## Documentation definition of done

Any new or modified documentation should meet the following criteria before being published:

1. _Thoroughly reviewed:_ Ideally, documentation should be peer-reviewed by two people: one who is familiar with the material being covered, and one who is new to the material. This will ensure that documentation is appropriate for developers with different experience levels.
1. _Tested:_ Any documentation that describes a process (for example, setting up a project) should be tested fully by a developer to verify its correctness. Consider defining and using user stories to guide this process (for example, "as a new member of the team, I need to get cf.gov running locally on my machine").
1. _Properly located:_ Documentation should be located in an appropriate place, and be discoverable for developers seeking it out. See [above](#where-to-put-documentation) for additional detail.