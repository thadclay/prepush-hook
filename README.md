prepush-hook
============

An npm installable git pre-push hook used to lint and test your code.
Thanks to [nlf] for his [precommit-hook] project. prepush-hook borrows heavily from his work.

[nlf]:https://github.com/nlf
[precommit-hook]:https://github.com/nlf/precommit-hook

Installation
-----

    npm install prepush-hook


Everything else is automatic!

I recommend putting precommit-hook in your project's devDependencies to make sure that anyone who may be contributing to your project will have the hook installed.

```
{
  "name": "your_project",
  "description": "just an example",
  "scripts": {
    "validate": "./command/to/run",
    "test": "./other/command"
  },
  "devDependencies": {
    "precommit-hook": "latest"
  }
}
```

Usage
-----

When you install this project, by default it will create sane `.jshintignore` and `.jshintrc` files for you if they do not already exist. That means it's safe to upgrade the hook after customizing these files, as they will never be overwritten. If you have your jshint configuration in your package.json, then the `.jshintrc` file will not be created ever.

You can prevent this behavior by modifying your package.json file. If a `lint` script is specified and does not start with the string `"jshint"` OR if you configure the hook to run specific scripts, and that list of scripts does not include `"lint"`, the jshint configuration files will not be created.

For example:

```javascript
{
  "name": "your_project",
  "description": "just an example",
  "scripts": {
    "lint": "some_other_linter ."
  }
}
```

OR

```javascript
{
  "name": "your_project",
  "description": "just an example",
  "precommit": ["test"]
}
```

If you do not configure the hook with an array of scripts to run, it will default to `["lint", "validate", "test"]` to maintain backwards compatibility with the old version of this hook. In addition to that, if a `lint` script is not specified, it will default to `"jshint ."`. If a `lint` script is configured, it will not be overridden. If an array of scripts is configured, it will be used and there will be no default `lint` script.

Package.json
------------

```javascript
{
  "name": "your_project",
  "description": "just an example",
  "scripts": {
    "validate": "./command/to/run",
    "test": "./other/command"
  }
}
```

The contents of the validate and test properties are the shell command to be run to perform those functions. Having these specified in your package.json also
lends you the ability to be able to run them manually like so

```
npm run-script validate
npm test
```

These scripts can be any shell executable commands, but must exit with a status code of 0 for success and 1 or greater for failure. The `PATH` environment variable used when executing these scripts will be similar to how npm configures it. That means if you `npm install jshint` locally to your project, you can put simply `"jshint ."` for your script rather than `"./node_modules/.bin/jshint ."`.

You may configure what scripts will be run by the hook, by passing an array of script names to the `"precommit"` key in your package.json.

```javascript
{
  "name": "your_project",
  "description": "just an example",
  "scripts": {
    "lint": "jshint --with --different-options",
    "validate": "./command/to/run",
    "test": "./other/command"
  },
  "precommit": ["lint", "test"]
}
```

This example would run only the `lint` and `test` scripts, in that order.

JSHint Defaults
---------------

The default `.jshintrc` looks like the following:

```javascript
{
  "node": true, // node environment
  "curly": true, // enforce using curly braces around blocks
  "latedef": true, // enforce defining a variable before using it
  "undef": true, // error on use of undefined variables
  "unused": true, // error on variables that are defined but never used
  "trailing": true // error on trailing whitespace
  "eqeqeq": true // prohibits the use of == and != in favor of === and !==
  "camelcase": true // force all variable names to use either camelCase style or UPPER_CASE
  "maxlen": true // enforce line length maximum of 80
}
```

And the default `.jshintignore` contains only one line

```
node_modules
```

License
-------

MIT
