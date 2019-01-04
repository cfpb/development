# Using npm Modules in Projects

This document provides general guidelines for using [npm](https://www.npmjs.com/) with CFPB projects.

*If you are looking for guidelines on publishing Node modules, see [authoring-npm-modules.md](authoring-npm-modules.md).*

Show me the code! Do you prefer seeing how something works instead of
instructions? If so, check out this pull request to the Amortize module. Each
instruction below lives as its own commit. https://github.com/cfpb/amortize/pull/11


## 1. Add an `.npmrc` file.

The contents of the file should be:

```
save-exact=true
```

This will ensure that all new dependencies are installed at the exact version
number, ensuring that incremental version changes are intentional.


## 2. Pin existing dependencies

Pin all project dependencies in the `package.json`. npm's defualt behavior is
to download incremental updates to packages. By removing `~` or `^` from a
dependency's version number in the package.json we are able to ensure that
we're always using the same version of the package.

Example of pinned dendencies:

```
"dependencies": {
    "backbone": "1.0.0",
    "jquery": "1.11.3",
    "underscore": "1.4.4"
}

```


## 3. Run npm shrinkwrap

Run `npm shrinkwrap` in the root of your project. This will not only pin your
direct dependencies but do the same for their dependencies. This ensures that
the project uses the same dependency versions in all environments. Running this
command will produce an `npm-shrinkwrap.json` file.


## 4. Set up Snyk

[Snyk](https://snyk.io/) allows you to monitor and test your npm dependencies
for known security vulnerabilite. **Snyk now requires user authentication. It
is recommended that you run Snyk locally any time tests are run, but at this
time it will not work with CI builds (Travis and Jenkins), due to the GitHub
permissions required.**

First, create an account at [snyk.io](https://snyk.io/)

Next, install snyk globally, authorize it with your account, and then run
Snyk's wizard from the root of your project's directory:

```
npm install –g snyk
snyk auth
snyk wizard
```

Snyk will walk you through updating vulnerable dependencies and ask if you want
to add `snyk test` to your package.json, reply **yes**.

Note: For a project with some out of date dependencies, this may need to be run
a few times to get everything to a passing stae.

Finally, add `.snyk` to your `.gitignore` as there is no need for it to be
checked into source control.
