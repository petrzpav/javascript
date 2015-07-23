# Tooling Recommendations for JavaScript Projects

![It Is Dangerous To Go Alone, Take One Of These](tooling.png)

This is an overview of tools you may use in your project and (rather opinionated) guide to choosing the right option.

## Package Managers

- Use [NPM](https://www.npmjs.com/) with [some bundler](#module-bundlers),
- don't use [Bower](http://bower.io/) unless you know what you are losing,
- look forward to [SystemJS](https://github.com/systemjs/systemjs) and ES6 modules.

When it comes to package management in JavaScript, there is [NPM](https://www.npmjs.com/) and then there are others. You will use NPM to install all Node-based tools, including other package managers. However, it is perfectly okay to use NPM for front-end dependencies (i.e. for browsers) – it is even [officially endorsed](http://blog.npmjs.org/post/101775448305/npm-and-front-end-packaging).

NPM packages are “[CommonJS modules](https://nodejs.org/docs/latest/api/modules.html);” every module _requires_ its dependencies and _exports_ stuff to be consumed by other modules. Thus your code has an obvious [dependency graph](https://en.wikipedia.org/wiki/Dependency_graph) and you don't need to pollute global namespace with your functions. The only disadvantage of NPM is that you need to _bundle_ your modules to be consumed by browsers, even while you are developing the application. There are more details [in separate section](#module-bundlers).

For introduction to NPM read [A Beginner’s Guide to npm](http://www.sitepoint.com/beginners-guide-node-package-manager/).

Another popular package manager specifically for front-end projects is [Bower](http://bower.io/). It is strictly a dependency manager: it will just download your dependencies to `bower_components` directory and the rest is up to you; usually you will place `<script>` into your HTML files for every dependency you want to use. This is fine for very small projects, but it can get out of hand very quickly.

- Dependencies between packages are not explicit, you need to manage them yourself,
- you won't gain any support to split your code into multiple modules,
- since there is no dependency graph, you will usually resort to passing dependencies through the global space and/or by modifying global objects (popular with jQuery plugins),
- for code minification and concatenation you will end-up with [some crazy magic comments](https://github.com/stephenplusplus/grunt-wiredep).

On the other hand, Bower does not require you to bundle your code and does not assume anything about your modules, which gives you more options (usually to do some very messy stuff). It is still better than manually downloading JS files and placing it into your repository, but only marginally. If you have only a few dependencies or you want to avoid compile step, you may as well [use libraries from CDN](https://cdnjs.com/).

<!--
One alternative worth mentioning is [jspm](http://jspm.io/), made specifically to work with module loader and compatible with NPM registry. This may be very interesting future-facing option as more browsers gain support for HTTP/2 and ES6 features; we will go a bit into detail in [Module Loaders](#module-loaders) section.
-->

## Module Bundlers

Once you decide to divide your code into neat modules or tap into [npm's huge registry](https://www.npmjs.com/), you will need to decide how to deliver your code for browser.

Node.js supports CommonJS by default, so your server-side code will work out of the box. However, browser don't understand `require` or `exports` in your modules. This is where _module bundler_ comes to play: it will load your _entry file_, traverse your dependency graph and spits out a single JS file, _bundle_, ready to be served to browsers.

Our favourite bundler is [Browserify](http://browserify.org/); it is very simple to use, can be nicely extended and plays well with existing Node ecosystem. While developing project, you may want to use [Watchify](https://github.com/substack/watchify), which monitors changes in your files and rebuilds the bundle for you.

Recently [Webpack](https://webpack.github.io/) gained traction as more complete solution. It handles all assets, including CSS files and images. Webpack takes a “batteries-included” approach: it has more closed ecosystem and it is less composable, but handles common use cases more easily. The most controversial feature of Webpack is its use of “[loaders](https://webpack.github.io/docs/using-loaders.html)” [directly within the code](https://webpack.github.io/docs/using-loaders.html#loaders-in-require) which breaks interoperability with other CommonJS modules and can lead to duplication of code, since you need to repeat your loaders all over the place. The recommended way is to specify your configuration in [`webpack.config.js` file](https://webpack.github.io/docs/configuration.html) outside of your code.

The choice of bundler depends on your preferences and project needs. Browserify is a great choice for small and JavaScript-only projects, it is easy to tinker with and compose with others and its sharper focus on JS bundling can be also advantage. Webpack seems to be a goto solution for React projects and it lets you want to do more with your assets with less hassle, like inlining images to CSS or [combining CSS with JavaScript](https://github.com/css-modules).

### Recommended Readings

* [browserify-handbook](https://github.com/substack/browserify-handbook) (also for general introduction to CommonJS packages and modularization of your code)
* [Browserify for Webpack Users](https://gist.github.com/substack/68f8d502be42d5cd4942)
* [Browserify VS Webpack – JS Drama](http://blog.namangoel.com/browserify-vs-webpack-js-drama)

<!--
## Module Loaders
-->

## Build Tools

Now that you have some tools in your toolbelt, you surely need a task runner to manage are your build needs! Choose yours: [Grunt](http://gruntjs.com/), [Gulp](http://gulpjs.com/), [Broccoli](http://broccolijs.com/)? Or how about [Mimosa](http://mimosa.io/), [Gobble](https://github.com/gobblejs/gobble), [Brunch](http://brunch.io/), [Jake](http://jakejs.com/), [Cake](http://coffeescript.org/#cake), [Gear.js](http://gearjs.org/) or [Fly](https://git.io/fly)?

Well, you don't need any of these. [Use npm scripts](http://substack.net/task_automation_with_npm_run).

Probably the main promise of most of these build tools is an easy composability; you can combine your [transpiler](#transpilers) with [code minification](#minification), copy assets, publish packages etc. from one place. No need to hunt for commands with long, complicated arguments.

However [module bundlers](#module-bundlers) do that already for us; in the end, your fancy task runner is just a verbose way to pass configuration options to Browserify or Webpack.

Put those tasks you do repeatedly in `"scripts"` section in your `package.json`, for example:

```json
...
"scripts": {
  "test": "tap test/*.js",
  "build": "npm run build-js && npm run build-css",
  "build-js": "browserify index.js -o dist/index.js",
  "build-css": "cp index.css dist/index.css",
  "watch-js": "watchify index.js -o dist/index.js"
  ...
}
```

You can run these scripts with `npm run <script_name>`, e.g. `npm run build`.

If you need something more complex which the bundler won't do for you, like copy HTML files or images to `dist/` directory, just write a shell script.

The only advantage of task runners is a portability: the same task will work in Mac OS X, Linux, and Windows (hopefully…). Your Bash scripts won't work in Windows out of the box. The solution is to write your scripts also in JavaScript. Use [ShellJS](http://documentup.com/arturadib/shelljs) which provides a familiarity of *nix commands with portability of JavaScript.

> It's a Unix system, you know this!

Put your custom scripts into `script/` folder. Name them in `package.json`:

```json
"scripts": {
  "something-complicated": "node script/something-complicated.js",
  ...
}
```

If you need to run some tasks in parallel, for example a development server and a modules bundler, try [npm-run-all](https://github.com/mysticatea/npm-run-all).

### Recommended Readings

- [Task Automation with `npm run`](http://substack.net/task_automation_with_npm_run)
- [Why we should stop using Grunt & Gulp](http://blog.keithcirkel.co.uk/why-we-should-stop-using-grunt/) and [How to Use npm as a Build Tool](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/)
- [Introducing ./task.js, THE new javascript task runner automation framework
](https://gist.github.com/substack/8313379)
- [NPM for Everything](http://beletsky.net/2015/04/npm-for-everything.html)

## Transpilers



## Type Checking

## Live Reloading

## Minification

## Linters


## Testing

## Hosted Services