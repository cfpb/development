# Start here

A handy guide to assist software developers at the CFPB.
Similar to a "definition of done" document, the below steps should be completed in order for a project to be considered done.

## 1. Before you start writing code

- [ ] Browse our [development guidelines](https://github.com/cfpb/development)
- [ ] Consult with other developers before making large technical decisions, e.g., before choosing your project's language, framework and/or architecture pattern
- [ ] Use test-driven development (TDD)
  - [ ] Set up your test harnesses before you start coding. If you're working with a pre-existing CFPB repo, this has probably already been done.
- [ ] Read our [accessibility audit documentation](https://docs.google.com/document/d/1MPLfvYRTX7fYKuskSV5FyCbmEW6vXXEn6W_-7uXwzaw/edit)
- [ ] Discuss web analytics goals with your team and schedule a meeting with the Digital Analytics team to plan Google Analytics implementation

## 2. Writing code

- [ ] Code complies with our [development guidelines](https://github.com/cfpb/development)
- [ ] Code is covered by tests and those tests pass locally
- [ ] New features work in browsers listed in our [browser checklist](/tools/browser-checklist.md)
- [ ] New features are usable without JavaScript enabled
- [ ] New features use the [CFPB design system](https://github.com/cfpb/design-system) components when applicable
- [ ] When a feature is nearing completion, perform an accessibility audit on the respective page(s). Report issues and make a plan made for fixing critical bugs before release

## 3. Merging code

- [ ] Use a [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [ ] Add CFPB's [PR template](/.github/PULL_REQUEST_TEMPLATE.md) to your repo if it isn't already there
- [ ] Keep pull requests small and frequent. All pull requests should:
  - [ ] Have passing tests automatically run by Travis CI
  - [ ] Satisfy the requirements in our PR template
  - [ ] Be [reviewed](/guides/code-reviews.md) and approved by at least one other developer
  - [ ] Be small enough in scope that they can be reviewed in a couple hours

## 4. Releasing code

- [ ] All critical and high-priority accessibility bugs have been fixed
- [ ] Project documentation has been updated (usually a README or GHE wiki)
- [ ] If applicable, usage of third-party data has been verified it originates from CFPB-approved sources and usage is documented
