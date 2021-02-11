# Code Review Guide

> A friendly guide for reviewing code--and not each other--at CFPB.

## Why reviews?

Code reviews are incredibly important step to the development process. Our goals
when reviewing code are to:

- expose bugs before they make it to production;
- ensure a consistent quality in the codebase;
- create an environment for sharing knowledge and developing skills;
- encourage open communication between the entire team.


## Tips for a successful review

For a code review to be successful, we all need to be on the same page. To help
accomplish this, everyone should follow these tips.

### For everyone

- Remember that we're reviewing the code, not the author. Nothing should be
  given or taken taken personally.
- There is often more than one way to approach a solution. Discuss tradeoffs and
  reach a resolution quickly.
- Refer to the style guides whenever there's a discussion that can't be resolved.
- Ask questions rather than make statements. ("What do you think about...?")
- Ask for clarification if the code or comments are unclear. ("I didn't understand.
  Can you clarify?")
- Support opinions with reason and facts: "I recommend this approach instead because...."
- Avoid selective ownership of code. ("mine", "not mine", "yours")
- Avoid using terms that could be seen as referring to personal traits.
- Be explicit, people don't always understand your intentions online.
- Be humble. ("I'm not sure - let's look it up.")
- Don't use hyperbole. ("always", "never", "endlessly", "nothing")
- Don't use sarcasm.
- Keep it real. If emoji, animated gifs, or humor aren't you, don't force them.
  If they are, use them with aplomb.
- Talk offline if there is too much back and forth. Post a follow-up comment
  summarizing the discussion.
- If you've paired on the code or otherwise discussed it in detail outside of the pull request,
  note that in the PR so it's clear to everyone that the work has had appropriate collaboration.
- Praise team members when they create exemplary work or suggestions.

### For code submitters

- Assign at least one reviewer to your pull request using GitHub's interface (either members from your project team or someone willing to pair).
- One :+1: comment from the reviewer indicates the Pull Request should be merged.
- Remind reviewers to respond within 1 week, otherwise you are free to merge the PR if you receive no feedback.
- Link to the code review from the originating Jira task/issue, if applicable.
- Remember that the code isn't you, don't get defensive when a reviewer is critical
  of the code.
- Seek to understand the reviewer's perspective.
- Try to respond to every comment.
- Be grateful for the reviewer's suggestions. ("Good catch, fixing in a4994ec")
- Explain why the code exists. ("We need to work around these existing patterns")
- Extract out-of-scope changes and refactorings into future Jira tasks/issues.
- Push commits based on earlier rounds of feedback as isolated commits to the
  branch. Do not squash until the branch is ready to merge. Reviewers should be
  able to read individual updates based on their earlier feedback.

### For code reviewers

- Review the code, make comments, and/or merge within 1 week of being assigned a pull request.
- Understand why the code is necessary (bug, user experience, refactoring)
- Seek to understand the author's perspective.
- Clearly communicate which ideas you feel strongly about and those you don't.
- Identify ways to simplify the code while still solving the problem.
- Offer alternative implementations, but assume the author already considered
  them. ("What do you think about such-and-such here?")
- If discussions turn too philosophical or academic, move the discussion to a new
  issue or offline. In the meantime, the **author has the final say on the current
  implementation**.
- Avoid focusing on trivial issues. This is often termed [bike-shedding](https://en.wikipedia.org/wiki/Law_of_triviality).
- Sign off on the pull request with a :thumbsup: or "Ready to merge" comment.
- Wait to merge the branch until it has passed Continuous Integration testing.
  (TDDium, TravisCI, etc.)
- Merge once you feel confident in the code and its impact on the project.


## <a name="submitting"></a>Submitting code

Before seeking a review, you should be able to check off each of the following:

- [ ] Changes are limited to a single goal (no scope creep)
- [ ] Code can be automatically merged (no conflicts)
- [ ] Code follows the standards laid out in this playbook
- [ ] Passes all existing automated tests
- [ ] New functions include new tests
- [ ] New functions are documented (with a description, list of inputs, and
      expected output)
- [ ] Placeholder code is flagged
- [ ] Visually tested in supported browsers and devices
- [ ] Project documentation has been updated (including the "Unreleased" section of
      the CHANGELOG)

### Opening a PR

Once sure your code meets the checks above, open a Pull Request for your new
feature or bugfix following our [Pull Request Template](https://raw.githubusercontent.com/cfpb/front-end/main/.github/PULL_REQUEST_TEMPLATE.md). You can also create a [Bookmarklet based on this template](https://gist.github.com/cfarm/b9b638943a2eea52a3a8). Remember that the
more information you provide to reviewers, the more context they will have. This
leads to faster reviews, and less back and forth between everyone.


## Reviewing code

When reviewing code, you should be able to check off each of the following:

- [ ] Do the changes address the project's needs?
- [ ] Do the changes respect the project's existing style?
- [ ] Does the new code avoid reproducing existing functionality?
- [ ] Are functions as simple as possible?
- [ ] Is the code as efficient as possible?
- [ ] Is the usage of each function clear?
- [ ] Have edge cases been considered and tested for?

Use this bookmarklet code to add this checklist to your Pull Request comments: [Code Review Checklist](https://gist.github.com/cfarm/a4174fe6f775353a3115)

### Commenting on code

When commenting on breaks to our style guide, reviewers should include a link back
to the appropriate guide.

Ex:
```
[CSS - Property Values](https://github.com/cfpb/front-end/css.md#property-values):

> If the value of a property is `0`, do not specify units
```

If you disagree with a guideline or there is no current guideline, open an issue in
[this front-end repo](https://github.com/cfpb/front-end) and link back to the
discussion, rather than debating it within the review. In the meantime, follow the
existing guideline.


## Credits

- [Code Review in the Lab](http://mozillascience.github.io/codeReview/intro.html)
- [GitHub Code Reviews](https://blog.codeship.com/github-code-review/)
- [thoughtbot Code Review Guide](https://github.com/thoughtbot/guides/blob/main/code-review/README.md)
- [Effective Code Reviews without the Pain](http://www.developer.com/tech/article.php/3579756/Effective-Code-Reviews-Without-the-Pain.htm)
