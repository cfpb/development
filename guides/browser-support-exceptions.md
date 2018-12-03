# Browser Support Exceptions Guide

New projects and features often have questions about browser support: which
browsers and features need to be supported, when it is acceptable to provide
alternate displays, etc.

[Browser support standards](browser-support.md)
have been established by the Front End Development (FEWD) team. In some cases,
however, there will be reasons that customers, product owners, or engineering
will want to change what they are required to support in their application.
This could be because a new leading-edge technology delivers value that cannot
be achieved otherwise, or the customer's target market is not our normal user
base and will use a known set of browsers, etc.

Without a clear idea of the process for requesting these exceptions, projects
will not be able to effectively plan their engineering efforts. This document
outlines a decision making process for requesting an exception to current
browser support standards.

## Process flow

1. Project starts and team assesses front end needs
2. If project team decides their needs are unique and cannot be supported by
current CFPB browser standards, they create a Hubcap GHE (https://[GHE]/CFPB/hubcap) Issue to review the
situation, including answers to the following questions:   
    - What is the device/OS/browser/feature for which you would like to drop
    support in this app?
    - What problem does dropping this support solve?
        - Are you dropping support because engineering compatible solutions
        is too hard, or because no compatible solution yet exists? If the
        former, discuss the business impact of adding the time to build the
        compatible solution.
    - What is your fallback for providing this content in an accessible way
    to unsupported browsers?
    - What percentage of site users will not have access to your app because
    of this?
        - Request information from Digital Analytics so team can assess based
        on exact numbers. Include audiences if relevant.
        - Include affected pages from [list of key pages]()
        - Include affected satellite [apps/repos](
        browser-support.md#individual-projects-to-test)
   - What are the performance impacts? Provide estimates or prototyped numbers
   for key metrics using Google Analytics and WebPageTest on impacted pages.
    - What is the deadline for the response to this request? (minimum one week
    turnaround)
3. FEWD team reviews and may offer comments at this time.
4. A facilitator for FEWD team keeps track of these requests in Hubcap, alerts
working group members in chat, and asks if group feels need to meet and
discuss or is ready to make a recommendation based on the provided
information.
    - If at least one member requests a meeting, facilitator schedules it and
    team meets to discuss and make a decision on recommendation.
    - If meeting is not requested, recommendation is posted in Github and sent
    to Adam and Jess for approval.
5. Decision is posted in Github. Decision should include the following:
    - The decision (yes/no)
    - Do we suggest this change goes for all future apps/does this change
    standards?
6. For new projects, FEWD team schedules meeting with CF.gov product owners 
(Adam, Jess), project team, and business customers/product owners if further
discussion is required.
7. Do the thing.
