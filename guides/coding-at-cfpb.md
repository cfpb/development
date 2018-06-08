# Coding @ CFPB

A handy guide to assist software developers at CFPB.
Similar to a "definition of done" document, the below steps should be completed in order for a project to be considered done.

## Before you start writing code

- [ ] Browse CFPB's [development guidelines](https://github.com/cfpb/development)
- [ ] Consult with CFPB developers before making large technical decisions, e.g. before choosing your project's language, framework and/or architecture pattern
- [ ] Use test-driven development (TDD)
  - [ ] Set up your test harnesses before you start coding. If you're working with a pre-existing CFPB repo, this has probably already been done.
- [ ] Read CFPB's accessibility audit documentation
- [ ] Undergo accessibility audit training (schedule it if you haven't already)

## Writing code

- [ ] Code complies with CFPB's [development guidelines](https://github.com/cfpb/development)
- [ ] Code is covered by tests and those tests pass locally
- [ ] New features work in browsers listed in CFPB's [browser checklist](/tools/browser-checklist.md)
- [ ] New features are usable without JavaScript enabled
- [ ] New features use [Capital Framework](https://cfpb.github.io/capital-framework/) components when applicable
- [ ] When a feature is nearing completion, perform an accessibility audit on the respective page(s). Report issues and make a plan made for fixing critical bugs before release

## Merging code

- [ ] Use a [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [ ] Add CFPB's [PR template](/.github/PULL_REQUEST_TEMPLATE.md) to your repo if it isn't already there
- [ ] Keep pull requests small and frequent. All pull requests should:
  - [ ] Have passing tests automatically run by Travis CI
  - [ ] Satisfy the requirements in CFPB's PR template
  - [ ] Be [reviewed](/guides/code-reviews.md) and approved by at least one CFPB developer
  - [ ] Be small enough in scope that they can be reviewed in a couple hours

## Releasing code

- [ ] All critical and high-priority accessibility bugs have been fixed
- [ ] Project documentation has been updated (usually a README or GHE wiki)
- [ ] If applicable, usage of third-party data has been verified it originates from CFPB approved sources and usage is documented
