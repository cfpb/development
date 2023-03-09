# Browser support

> A friendly guide for building cross-browser websites


## Why support?

The promise of the web is to allow anyone to [access content using any browser
on any device](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/).
By building cross-browser compatible websites we ensure that our content,
tools, and resources remain available for all that seek them, whether they be
the American public or Bureau staff. We seek to esnure this open accessibility
by designing and coding for unknown client environments, and continually
testing our products in as many environments as is practical.


## Tips for successfully building cross-browser websites

Cross-browser support doesn't come from a single place; it takes a combination
of tools and best practices. We've outlined a few high-level topics here, but
don't let that stop you from reaching further into your toolset.

### User-centered design

We build features for our users, not based on our own technology interests or
whims. When looking at a new feature we first start by asking ourselves how
will it benefit the user compared to our current offerings and what negative
implications will we need to work around. Afterall, what good is a feature if
our users are unable to access it. By keeping our users in the forefront of our
design and development decisions, we can assure ourselves our work is in their
best interest and ensure a level of experience for all.

### Progressive enhancement

We always prefer to build features that work for any browser. When that's
either impossible or impractical, we start with a basic experience, and build
additional features on top. For example, when a user submits a form that
returns a response, the base experience would be to reload the page (or
navigate to a response-specific page) with the server's response. To improve
the experience, we could submit the form via an Ajax request and inject the
response into the page. We could then remove the logic to handle the servers
response on page load to reduce our own technical debt, but what about users
that can't access the scripting? Rather than creating a broken experience, we
ensure the default behavior continues to work unimpeded, and then build upon
it to improve the experience for the browsers that can handle it.

#### Known feature differences

- Scripting: We do not serve interactive scripting to browsers that do not support the Fetch API,
  but we do deliver analytics via scripting.
- Icons: We currently use icon fonts to deliver scalable icons. Browsers that
  do not support icon fonts unfortunately do not receive backups but we try to
  always pair icons with text to ensure a user can continue to interpret a
  page's content.

#### Configuration

Each project has its own configuration depending on the requirements of the
tools and features that project depends on. As a general rule, we aim to support 
the following:

- For JavaScript, every browser that supports the Fetch API
  (see [caniuse fetch stats](https://caniuse.com/fetch)).

- For CSS, every browser over 0.5% or so of average visits. 
  Depending on project, this may be configured through a 
  [browserslist string](https://github.com/browserslist/browserslist) 
  in the project's `package.json` file. In any particular repository with this string, 
  run `npx browserslist` to get a list of browsers that get fed into the CSS build process.

### Browser testing

We use automated tests with a headless browser (generally Chrome or Electron) to ensure the majority
of the site works as expected. For manual testing, we generally test edge-case 
browsers as issues are suspected or arise.

#### Manual testing

We use [Sauce Labs](https://saucelabs.com/) to manually test our site.
Be sure to review both the
[testing](https://github.com/cfpb/development/blob/main/guides/front-end-testing.md)
and [accessibility](https://github.com/cfpb/development/blob/main/guides/accessibility.md)
guides when reviewing sites locally.

Do not use reverse proxy tools like ngrok or localtunnel or you will receive angry emails.

##### Sauce Labs

See [#171](https://github.com/cfpb/development/pull/171) for details on testing
with Sauce Labs.

#### Individual projects to test

- [consumerfinance.gov](https://github.com/cfpb/consumerfinance.gov/blob/main/CONTRIBUTING.md#browser-support)
- [Design System](https://github.com/cfpb/design-system/blob/main/CONTRIBUTING.md#browser-support)
- [Salesforce/Mosaic/Submit a complaint app/Complaint portal]([GHE]/Mosaic/mosaic-toolbelt/blob/master/CONTRIBUTING.md#browser-support)
- [cfpb-chart-builder](https://github.com/cfpb/cfpb-chart-builder/blob/main/CONTRIBUTING.md#browser-support)
- [HMDA Platform](https://github.com/cfpb/hmda-platform/blob/master/CONTRIBUTING.md#browser-support)

## Definitions

### Interactive scripting

Interactive scripting is additions or adjustments to the browser's default
response to a user's interaction. Examples are the Mega Menu, Expandables,
Notifications, and Sortable Tables.

### Non-interactive scripting

Non-interactive scripting is tooling and analytics that we utilize as
developers but that remain hidden to the end user. For example, Google Analytics.


## Resources

### Best practices

- [Introduction to cross browser testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction)
- [Make the Web Work For Everyone](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/)
- [Building Web Applications for Everyone](https://github.com/ascott1/ethical-web-dev/blob/master/web-apps-for-everyone/02-progressive-enhancement.md)

### Browser testing

- [Download virtual IE machines](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)
- [Test your work with a variety of devices and browsers](https://saucelabs.com/beta/dashboard/tests)
- [Test your work with a variety of Samsung devices](http://developer.samsung.com/remotetestlab/rtlDeviceList.action#)
