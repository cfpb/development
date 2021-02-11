# Atomic Design Guide

> Helpful guidance and tips for building front-end components atomically.

## What is Atomic Design?

Created by [Brad Frost](http://bradfrost.com/blog/post/atomic-web-design/), Atomic Design is a system for building and maintaining reusable components (we will refer to them as elements to avoid confusion with the larger Design System Components) for a web platform. By creating simple elements, and combining them to build larger and more complex elements, we can ensure a consistent design throughout our projects.

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

See the Design System [changing the codebase documentation](https://github.com/cfpb/design-system/blob/main/CONTRIBUTING.md#changing-the-codebase).

## Questions

Feel free to [start a conversation](https://github.com/cfpb/design-system/issues/new) in the Design System repo.
