# RxJS documentation project

Everything in this folder is part of the documentation project. This includes

* the web site for displaying the documentation
* the dgeni configuration for converting source files to rendered files that can be viewed in the web site.
* the tooling for setting up examples for development; and generating live-example and zip files from the examples.

## Developer tasks

We use `npm` to manage the dependencies and to run build tasks.
You should run all these tasks from the `rxjs/docs_app` folder.
Here are the most important tasks you might need to use:

* `npm install` - install all the dependencies.
* `npm run setup` - install all the dependencies, boilerplate, stackblitz, zips and run dgeni on the docs.
* `npm run setup-local` - same as `setup`, but use the locally built RxJS packages for docs and docs examples boilerplate.

* `npm run build` - create a production build of the application (after installing dependencies, boilerplate, etc).
* `npm run build-local` - same as `build`, but use `setup-local` instead of `setup`.

* `npm start` - run a development web server that watches the files; then builds the doc-viewer and reloads the page, as necessary.
* `npm run serve-and-sync` - run both the `docs-watch` and `start` in the same console.
* `npm run lint` - check that the doc-viewer code follows our style rules.
* `npm test` - watch all the source files, for the doc-viewer, and run all the unit tests when any change.
* `npm run e2e` - run all the e2e tests for the doc-viewer.

* `npm run docs` - generate all the docs from the source files.
* `npm run docs-watch` - watch the RxJS source and the docs files and run a short-circuited doc-gen for the docs that changed (don't work properly at the moment).
* `npm run docs-lint` - check that the doc gen code follows our style rules.
* `npm run docs-test` - run the unit tests for the doc generation code.

* `npm run boilerplate:add` - generate all the boilerplate code for the examples, so that they can be run locally. Add the option `--local` to use your local version of Angular contained in the "dist" folder.
* `npm run boilerplate:remove` - remove all the boilerplate code that was added via `npm run boilerplate:add`.
* `npm run generate-stackblitz` - generate the stackblitz files that are used by the `live-example` tags in the docs.
* `npm run generate-zips` - generate the zip files from the examples. Zip available via the `live-example` tags in the docs.

* `npm run example-e2e` - run all e2e tests for examples
  - `npm run example-e2e --setup` - force webdriver update & other setup, then run tests
  - `npm run example-e2e --filter=foo` - limit e2e tests to those containing the word "foo"
  - `npm run example-e2e --setup --local` - run e2e tests with the local version of RxJS contained in the "dist" folder

* `npm run build-ie-polyfills` - generates a js file of polyfills that can be loaded in Internet Explorer.

## Using ServiceWorker locally

Running `npm run start --prod` will no longer set up the ServiceWorker, which
would require manually running `npm run sw-manifest` and `npm run sw-copy` (something that is not possible
with webpack serving the files from memory).

If you want to test ServiceWorker locally, you can use `npm build` and serve the files in `dist/`
with `npm run http-server dist -p 4200`.

## Guide to authoring

There are two types of content in the documentation:

* **API docs**: descriptions of the modules, classes, interfaces, etc that make up RxJS.
API docs are generated directly from the source code.
The source code is contained in TypeScript files, located in the `rxjs/src` folder.
Each API item may have a preceding comment, which contains JSDoc style tags and content.
The content is written in markdown.

* **Other content**: guides, tutorials, and other marketing material.
All other content is written using markdown in text files, located in the `rxjs/docs_app/content` folder.
More specifically, there are sub-folders that contain particular types of content: guides, tutorial and marketing.

* **Code examples**: code examples need to be testable to ensure their accuracy.
Also, our examples have a specific look and feel and allow the user to copy the source code. For larger
examples they are rendered in a tabbed interface (e.g. template, HTML, and TypeScript on separate
tabs). Additionally, some are live examples, which provide links where the code can be edited, executed, and/or downloaded. For details on working with code examples, please read the [Code snippets](https://angular.io/guide/docs-style-guide#code-snippets), [Source code markup](https://angular.io/guide/docs-style-guide#source-code-markup), and [Live examples](https://angular.io/guide/docs-style-guide#live-examples) pages of the [Authors Style Guide](https://angular.io/guide/docs-style-guide).

We use the [dgeni](https://github.com/angular/dgeni) tool to convert these files into docs that can be viewed in the doc-viewer.

The [Authors Style Guide](https://angular.io/guide/docs-style-guide) prescribes guidelines for
writing guide pages, explains how to use the documentation classes and components, and how to markup sample source code to produce code snippets.

### Generating the complete docs

The main task for generating the docs is `npm run docs`. This will process all the source files (API and other),
extracting the documentation and generating JSON files that can be consumed by the doc-viewer.

### Partial doc generation for editors

Full doc generation can take up to one minute. That's too slow for efficient document creation and editing.

You can make small changes in a smart editor that displays formatted markdown:
>In VS Code, _Cmd-K, V_ opens markdown preview in side pane; _Cmd-B_ toggles left sidebar

You also want to see those changes displayed properly in the doc viewer
with a quick, edit/view cycle time.

For this purpose, use the `npm run docs-watch` task, which watches for changes to source files and only
re-processes the the files necessary to generate the docs that are related to the file that has changed.
Since this task takes shortcuts, it is much faster (often less than 1 second) but it won't produce full
fidelity content. For example, links to other docs and code examples may not render correctly. This is
most particularly noticed in links to other docs and in the embedded examples, which may not always render
correctly.

The general setup is as follows:

* Open a terminal, ensure the dependencies are installed; run an initial doc generation; then start the doc-viewer:

```bash
npm run setup
npm run start
```

* Open a second terminal and start watching the docs

```bash
npm run docs-watch
```

>Alternatively, try the consolidated `serve-and-sync` command that builds, watches and serves in the same terminal window
```bash
npm run serve-and-sync
```

* Open a browser at https://localhost:4200/ and navigate to the document on which you want to work.
You can automatically open the browser by using `npm run start -o` in the first terminal.

* Make changes to the page's associated doc or example files. Every time a file is saved, the doc will
be regenerated, the app will rebuild and the page will reload.

* If you get a build error complaining about examples or any other odd behavior, be sure to consult
the [Authors Style Guide](https://angular.io/guide/docs-style-guide).

## Disclaimer

Starting the new documentation, we worked closely together with the Angular team and therefore adapted their way of generating docs. This leads to the effect, that there may be some references to angular (e.g. variable names, file names ...). Don't be confused by this, this shouldn't bother you. Thanks to the Angular Team for their support.  
Anyway RxJS will always be an independent project, which aims to work closely with other technologies and frameworks!