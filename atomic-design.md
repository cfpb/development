# Atomic Design Guide

> Helpful guidance and tips for building front-end components atomically.

## What is Atomic Design?

Created by [Brad Frost](http://bradfrost.com/blog/post/atomic-web-design/), Atomic Design is a system for building and maintaining reusable components (we will refer to them as elements to avoid confusion with the larger Capital Framework Components) for a web platform. By creating simple elements, and combining them to build larger and more complex elements, we can ensure a consistent design throughout our projects.

## What's an atomic element?

An atomic element is any self contained code pattern that follows the Atomic Design principles. The types of elements are atoms, molecules, organisms, and templates.

### Atoms

- The base unit
- Think buttons, input fields, and other HTML tags
- Abstract and not too useful on their own

### Molecules

- It can do something
- A series of atoms combined to serve a purpose

### Organisms

- Self contained chunks with different functionalities to form a distinct section
- Think headers, comments section, etc

### Templates

Types of pages that have a particular layout. The primary ones are Landing, Sublanding, Browse, Learn, and Document Detail. They are differentiated primarily by their major layout features, such as whether or not they have a hero, whether the sidebar is on the right or left, etc. Each template allows certain atoms, molecules, and organisms to be included in its major sections.

### How do I know what to make into an element?

- If it can't stand on it's own. For example a paragraph or list item have minimal styles added to them and can live on their own so they wouldn't be elements. A button is more complex (comparatively) and so it should become an element.
- If it's not a utility. Utilities are adjustments and helpers, they don't need to become elements.
- If it's not a layout helper. Layout helpers are specifically used to lay out the structure of a page and are never concerned with the content within them.

## File organization

Both prefixing and organizing elements by their type is the easiest way to keep them organized. All element classes should be prefixed by the first letter of its type (ex `.m-pagination`) and placed in a corresponding directory (ex. `/src/molecules/pagination.less`). To save yourself the trouble of moving things multiple times, we have found it easiest to break up an element in its top level stylesheet, then move each class block to its appropriate sub-directory.

## Working with code

### Install the Capital Framework Sandbox

1. Follow the [installation instructions](https://github.com/jimmynotjim/capital-framework-sandbox#installation).
2. Click around to ensure the local server is rendering the sandbox correctly.
3. Stop the server with `ctrl+c`.

### Link your Capital Framework Component

1. From the root of `capital-framework`

  `npm run cf-link`

  _To add your Component to this command, edit [the script](https://github.com/cfpb/capital-framework/blob/prerelease-ver-4/package.json#L28) in `package.json`_
2. In a new tab, navigate to the root of the Sandbox

   `cd capital-framework-sandbox`
3. Run the same command from the root of the Sandbox repo

  `npm run cf-link`

  _Again, to add your Component to this command, edit the script in this repo’s `package.json`_
4. Run the build command

   `npm run build`
5. Start the server and check out the changes

   `npm start`

_When you’re done working on or reviewing a Component, run `npm unlink component-name && npm install component-name`_

### Converting a Capital Framework Component for the v4 release

1. Checkout the [v4 prerelease branch](https://github.com/cfpb/capital-framework/tree/prerelease-ver-4) of Capital Framework

  ```sh
  cd capital-framework
  git checkout prerelease-ver-4
  ```
1. Create a new branch for your Capital Framework Component

  ```sh
  git checkout -b atomic/pagination
  ```
1. Save your changes
1. In the Sandbox create a new preview branch, build the assets, start the server, and adjust markup as needed

  ```sh
  cd capital-framework-sandbox
  git checkout -b cf-pagination-example
  npm run build
  npm start
  ```
1. When you save changes in Capital Framework, the Sandbox will re-build the files (may still need a manual page refresh) as long as the server is running.

## Questions

Feel free to [start a conversation](https://github.com/cfpb/capital-framework/issues/new) in the Capital Framework repo or in the Atomic Design chat channel.
