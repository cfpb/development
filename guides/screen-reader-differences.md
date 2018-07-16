In this guide:

- [Screen reader overview](#Overview)
  - [Commonalities](#Commonalities)
  - [Differences](#Differences)
      - [Interaction modes](#Interaction-modes)
- [Guidelines for developers](#Guidelines-for-developers)

# Overview

Screen reader software allows users to navigate webpages by having the text of the page read aloud to them linearly. Screen reader users can also interact with the elements on the page using special keyboard commands that are unique to each application. The top 3 screen readers according to the [2017 WebAIM Survey](https://webaim.org/projects/screenreadersurvey7/) are JAWS, NVDA, and VoiceOver:

|  Screen Reader |  % of respondents who commonly use it | 
|---|---|
|  JAWS | 66.0%  |
|  NVDA | 64.9%  |
|  VoiceOver | 39.6%  |

JAWS and NVDA are Windows applications. VoiceOver is available as a desktop Mac OS application and on iOS devices. If you are new to screen readers or need a refresher, check out this [demo video of how a screen reader works](https://www.youtube.com/watch?v=q_ATY9gimOM).


## Commonalities

It is important to keep in mind that screen readers "present content linearly to users, one item at a time. This contrasts with the way in which most people use visual interfaces. ... Users must progress through such systems in a step-wise manner. The insight that audio interfaces are linearized versions of web content is an important one that should guide web developers during the engineering and design process." [Source](https://webaim.org/techniques/screenreader/)

Accessibility guidelines that target screen reader users will benefit users no matter the software they use. Some things to keep in mind include:


- "You can't navigate through the headings of a web page if the page doesn't have headings." 
- "You can't hear the meaning of a graphic unless it has alternative text."
- "Non-descriptive link text such as 'click here' and 'more' offer little or no clues as to what will happen when the user selects them." [Source](https://webaim.org/articles/screenreader_testing/)

"Following the principles of the Web Content Accessibility Guidelines (WCAG) or Section 508 guidelines will definitely help remove barriers, though they are not fool-proof. As a matter of fact, no method is fool-proof. You'll have the most success by paying attention to principles and guidelines and **testing your content**." (emphasis intentionally added) [Source](https://webaim.org/articles/screenreader_testing/)

## Differences 

The main differences are in interaction modes and keyboard shortcuts. Also important is the fact that, [according to WebAIM](https://webaim.org/techniques/screenreader/), "some browsers seem to work better with certain screen readers." 

They recommend testing the following pairings when designing and developing websites:

- Firefox with NVDA
- Internet Explorer with JAWS
- Safari with VoiceOver
- Edge with Narrator

These also reflect the [most popular browser/screen reader combinations](https://webaim.org/projects/screenreadersurvey7/#browsercombos). An important consideration when conducting our own screen reader testing is that **10% of users surveyed use VoiceOver with Safari, and only 1.4% use VoiceOver with Chrome**.

### Interaction modes

In addition to reading the page content in a linear order, screen readers also offer modes for users to interact with specific types of webpage content. These vary across the different applications.

#### 1. Read/Browse mode
This mode is available in Windows screen readers like JAWS and NVDA, **and** in Mac OS and iOS VoiceOver. For VoiceOver, this mode is triggered using `control + option + A`.

Keyboard shortcuts can navigate through specific items in the page in this mode. For example, on VoiceOver you can type `control + option + command + H` to navigate through all the headings on the page. In JAWS, the user can do the same thing with just the `H` key.

#### 2. VoiceOver rotor

This mode is only available in VoiceOver. The rotor provides a menu interface for interacting with specific types of content that is present on the current webpage. 

The user can navigate between menus in the rotor using the left and right arrow keys. The up and down arrow keys navigate through the list of elements in a particular menu. For example, there is a Headings menu which contains all of the **currently visible** headings that are on the page in a list, like this:


Headings
- 1: H1
- 2: H2
- 3: H3
- 3: H3

![screen shot 2018-06-28 at 11 31 26 am](https://user-images.githubusercontent.com/704760/42045575-d944241c-7ac9-11e8-9473-ce2b2726cac7.png)



Because hidden elements are not considered part of the page content for screen readers*, they are not included in the elements listed by the rotor menus. This has important implications for how we design and code interactive elements on our webpages. For example, in the current implementation of our global navigation "mega menu", none of the content that's in the mega menu dropdown is accessible to the VoiceOver rotor unless the user has opened the dropdown with a mouse hover or the keyboard. This as a general problem with drop down menus and similar interaction designs that affects VoiceOver users (and likely all screen reader users- we just haven't tested other software yet).

_*NOTE: This is not the case for elements that are visually hidden using the same CSS techniques as are used to visually hide the "Skip nav" link on every page. It only applies to techniques that hide the content in a straightforward manner such as using `display: none` or `visibility: hidden`._

#### 3. Forms mode
VoiceOver does not have a forms or application mode. JAWS and NVDA do.

"Since screen readers use many of the keys on the keyboard for quick navigation, trying to enter text in a form field presents an interesting problem. For example, trying to type the letter "h" in browse mode with JAWS would take you out of the form field and navigate to the next heading on the page. Luckily, screen readers have a built-in fix for this issue: Forms mode. In forms mode, most standard screen reader quick keys are deactivated, so that you can fill out forms without accidentally triggering a quick key." [Source](https://webaim.org/articles/jaws/#formsmode)

Read about the specific [JAWS keyboard commands that apply in forms mode](https://webaim.org/articles/jaws/#formsmode) and the [NVDA forms mode](https://webaim.org/articles/nvda/#formsmode).

#### 4. Application mode
VoiceOver does not have a forms or application mode; JAWS and NVDA do. The application mode is triggered by the use of the `role="application"` ARIA attribute, which disables the default keyboard shortcuts for navigating the page. The developer is then responsible for implementing unique interaction shortcuts with JavaScript.

The W3C recommends developers **not use `role="application"`** if the interface is made up of standard HTML elements such as form elements, links, headings, etc.

"You only want to use `role=application` if the content you’re providing consists of only focusable, interactive controls, and of those, mostly advanced widgets that emulate a real desktop application" ([source](https://www.w3.org/TR/using-aria/#using-application)). This should virtually never be the case for a website that we are developing.

#### 5. VoiceOver on iOS

Swiping and touch screen interactions are the main difference for mobile. Things to watch out for:

- Users must swipe right to advance between focusable elements on the page. This is akin to using the tab key on a keyboard.
- Swiping left goes back to previous elements, just like using `shift + tab`.

Tabindex, focusable elements, and tab order should be thoroughly tested and verified on small screen interfaces especially to ensure usability with iOS VoiceOver.


# Guidelines for developers

These apply to differences between screen reader applications. Read these [screen reader guidelines for developers by WebAIM](https://webaim.org/techniques/screenreader/) for general guidance.

## Test your features in real screen reader software.

- Use CFPB's internal [web accessibility audit [links to GHE]](https://GHE/CFPB/hubcap/wiki/Accessibility#we-commit-to-auditing-for-accessibility-as-we-build--release-and-we-fix-the-bugs) (`https://GHE/CFPB/hubcap/wiki/Accessibility#we-commit-to-auditing-for-accessibility-as-we-build--release-and-we-fix-the-bugs`) to test new or updated features. It includes steps for conducting VoiceOver testing on desktop and mobile devices.
- Refer to the following excellent guides to do more extensive screen reader testing during the development process:
    - [Using JAWS to Evaluate Web Accessibility](https://webaim.org/articles/jaws/)
    - [Using NVDA to Evaluate Web Accessibility](https://webaim.org/articles/nvda/)
    - [Using VoiceOver to Evaluate Web Accessibility](https://webaim.org/articles/voiceover/)

## Don't use the `role="application"` attribute. There is really no use case for it on a standards-based website. [More info about why you should avoid application mode](https://www.marcozehe.de/2012/02/06/if-you-use-the-wai-aria-role-application-please-do-so-wisely/).

Certain ARIA role attributes trigger application mode on Windows screen readers. When in application mode, the user cannot use the default keyboard shortcuts for navigating the page, such as using the key `H` to navigate the page via headings. This is the **number one method that screen reader users use to find information on a page**, and application mode disables it ([source](https://webaim.org/projects/screenreadersurvey7/#finding)).

What's more, application mode is rare enough that screen reader users are often unfamiliar with it and unaware that the website they're visiting has invoked the screen reader to enter this mode. [Source](https://webaim.org/blog/three-things-voiceover/)


## Don't use the `role="menu"` attribute and only use other ARIA roles when strictly appropriate for the widget you are developing.

The menu role is for application menus, not navigation menus, so avoid it.

"If the navigation links at the top of a webpage have incorrectly been assigned an ARIA role='menu' (which is appropriate for application menus, not for navigation menus), this triggers forms mode, and the screen reader user cannot read with the arrow keys. The same thing can occur with other ARIA roles such as tab, dialog, grid, and toolbar." [Source](https://webaim.org/blog/three-things-voiceover/)

## If you must use ARIA roles for widgets or applications, test them in JAWS and NVDA.

Do not rely on VoiceOver when testing these implementations, because VoiceOver does not have a separate forms or applications mode that would be triggered by these roles. VoiceOver has custom keyboard shortcuts that really on more complex key combinations than Windows screen readers, so it does not rely on separate modes to allow users to enter keyboard input.


## Don't implement custom keyboard shortcuts.

Don't use JavaScript to create custom keyboard shortcuts for navigating your website. These can interfere with screen reader applications' default shortcuts and render the page unusable.