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

- Scripting: We do not serve interactive scripting to IE 8 but we do deliver
  analytics via scripting.
- Icons: We currently use icon fonts to deliver scalable icons. Browsers that
  do not support icon fonts unfortunately do not receive backups but we try to
  always pair icons with text to ensure a user can continue to interpret a
  page's content.

### Automation

Thankfully the world has created tools to help us track and polyfill
disparities between browsers. We rely on a handful of of these tools to help
us provide a level of backward compatability for modern features. As mentioned
above, this doesn't necessarily mean feature parity. Where it's impossible or
impractical to implement a modern feature, we fallback to standard practices
for that browser. For example, we do not deliver interactive scripting for
Internet Explorer 8, but we do ensure that default browser features continue to
work so users that can't or don't want to upgrade continue to have access to
the site and our content.

#### Configuration

Each project has its own configuration depending on the requirements of the
tools and features that project depends on. As a general rule, we strive to
cover the following browser list in each project.

We configure [Autoprefixer](#autoprefixer) and [Babel](#babel) to support
the following list of browsers:

- Latest 2 releases of all browsers including:
    - Chrome
    - Firefox
    - Safari
    - Internet Explorer
    - Edge
    - Opera
    - iOS Safari
    - Opera Mini
    - Android Browser
    - BlackBerry Browser
    - Opera Mobile
    - Chrome for Android
    - Firefox for Android
    - Samsung Internet

http://browserl.ist/?q=last+2+versions%2C+Explorer+%3E%3D+9

As well as additional Autoprefixer support for:

- Internet Explorer 9
- Internet Explorer 8

http://browserl.ist/?q=last+2+versions%2C+Explorer+%3E%3D+8

### Browser testing

We use automated tests with a headless version of Chrome to ensure the majority
of the site works as expected. For manual testing, we realistically test these
projects locally or in a virtual environment with the following list of
browsers:

- Chrome
- Firefox
- Safari
- Internet Explorer 8, 9, 10, and 11
- Edge
- iOS Safari
- Chrome for Android

#### Manual testing

We use a mix of [Sauce Labs](https://saucelabs.com/) and
[localtunnel](https://localtunnel.github.io/www/) to manually test our site.
Be sure to review both the
[testing](https://github.com/cfpb/development/blob/master/guides/front-end-testing.md)
and [accessibility](https://github.com/cfpb/development/blob/master/guides/accessibility.md)
guides when reviewing sites locally.

##### Sauce Labs

See [#171](https://github.com/cfpb/development/pull/171) for details on testing
with Sauce Labs.

##### localtunnel

To test changes using your CFPB issued iPhone or other devices, you'll need to
use localtunnel to expose your localhost. Start by installing localtunnel if
you haven't already.

```bash
npm install -g localtunnel
```

Start your project's localhost and in a separate tab run `lt --port 8000`
editing the port value to match your localhost setup. This will create a
temporary public URL that can be accessed from any device on any connection.
Navigate to the URL from your device and test to your heart's content, the URL
will remain active as long as the instance is active (keep this in mind, don't
leave it running forever). Use `ctr + c` to terminate the connection.

#### Individual projects to test

- [consumerfinance.gov](https://github.com/cfpb/consumerfinance.gov/blob/master/CONTRIBUTING.md#browser-support)
- [Design System](https://github.com/cfpb/design-system/blob/master/CONTRIBUTING.md#browser-support)
- [Paying for College](https://github.com/cfpb/college-costs/blob/master/CONTRIBUTING.md#browser-support)
- [Retirement](https://github.com/cfpb/retirement/blob/master/CONTRIBUTING.md#browser-support)
- [Complaint](https://github.com/cfpb/complaint/blob/master/CONTRIBUTING.md#browser-support)
- [eRegs](https://github.com/cfpb/eregs-2.0/blob/master/CONTRIBUTING.md#browser-support)
- [Salesforce/Mosaic/Submit a complaint app/Complaint portal]([GHE]/Mosaic/mosaic-toolbelt/blob/master/CONTRIBUTING.md#browser-support)
- [cfpb-chart-builder](https://github.com/cfpb/cfpb-chart-builder/blob/master/CONTRIBUTING.md#browser-support)
- [HMDA Platform](https://github.com/cfpb/hmda-platform/blob/master/CONTRIBUTING.md#browser-support)

## Definitions

### Autoprefixer

Autoprefixer parses our CSS and adds vendor prefixes to rules where necessary
using reported feature support by [Can I Use](https://caniuse.com/). For more
information, visit the [Autoprefixer documentation site]
(https://autoprefixer.github.io/).

### Babel

Babel compiles our [ES6](http://es6-features.org/) JavaScript where necessary
for the browsers that either don't support or have limited support for ES6
features. For more information, visit the [Babel documentation site]
(https://babeljs.io/).

### Interactive scripting

Interactive scripting is additions or adjustments to the browser's default
response to a user's interaction. Examples are the Mega Menu, Expandables,
Notifications, and Sortable Tables.

### Non-interactive scripting

Non-interactive scripting is tooling and analytics that we utilize as
developers but that remain hidden to the end user. Examples are Modernizr and
Google Analytics.


## Resources

### Best practices

- [Introduction to cross browser testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction)
- [Make the Web Work For Everyone](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/)
- [Building Web Applications for Everyone](https://github.com/ascott1/ethical-web-dev/blob/master/web-apps-for-everyone/02-progressive-enhancement.md)

### Browser testing

- [Download virtual IE machines](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)
- [Test your work with a variety of devices and browsers](https://saucelabs.com/beta/dashboard/tests)
- [Test your work with a variety of Samsung devices](http://developer.samsung.com/remotetestlab/rtlDeviceList.action#)
