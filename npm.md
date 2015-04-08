# Authoring npm modules

Whenever possible, we should write JavaScript as small modules that can be published to [npm](https://www.npmjs.com/). This document includes guidelines for authoring and publishing these modules.

The [full list of CFPB published node modules](https://www.npmjs.com/~cfpb) is availble on npm.

## Authoring

npm published modules should consist of tiny, reusable components. A general guideline is that if there is a bit of JavaScript that your app is using more than once, it would probably make a great npm published module. To simplify the process of authoring node modules, we've created [a yeoman generator](https://github.com/cfpb/generator-node-cfpb).

To use the generator:

1. Install it by running: `npm install -g generator-node-cfpb`
2. cd into an empty directory, run this command and follow the prompts: `yo node-cfpb`


## Testing

The beauty of tiny modules is that they're easy to test! Traditionally we have used [nodeunit](https://github.com/caolan/nodeunit) to test our modules.

## Publishing

When publishing a module to be used in CFPB projects, we ask that you do the following:

- Make sure the project is hosted in a CFPB github repo
- Publish the project to npm using your personal account
- Add the [CFPB](https://www.npmjs.com/~cfpb) and at least one colleague as a collaborator on the project.