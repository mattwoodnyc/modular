_DISCLAIMER: THIS PROJECT IS EXPERIMENTAL. ITS BEHAVIOR, AND THE BEHAVIOR OF
COMMANDS, WILL LIKELY CHANGE IN THE FUTURE._

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?)](https://github.com/jpmorganchase/modular/blob/master/CONTRIBUTING.md)

`modular` is a collection of tools and guidance to enable UI development at
scale. It is derived from work at JP Morgan to enable development in a single
repository by many teams.

It provides a `modular` CLI to scaffold and develop a new application from
scratch, as well as commands to add and manage further modules.

It is implemented on top of [`create-react-app`](https://create-react-app.dev/)
and [Yarn workspaces](https://classic.yarnpkg.com/en/docs/workspaces/).

## Commands

#### `yarn create modular-react-app <project-name>`

Bootstraps a new project, configured to use workspaces.

This also creates a workspace named 'app' containing a fresh application written
in [TypeScript](https://www.typescriptlang.org/).

#### `yarn modular add <module-name>`

Adds a new package by creating a new workspace under `packages/`. Packages can
currently be one of 3 types:

- A standalone application. This corresponds to a single `create-react-app`
  project in a workspace. Inside this workspace, you can import packages from
  other workspaces freely, and features like jsx and typechecking work out of
  the box.

- A View, which is a package that exports a React component by default. Views
  are primary, top-level components in `modular`. Read more about Views in
  [this explainer](/docs/views.md).

- A typical javascript package. You can use this to create any other kind of
  utility, tool, or whatever your needs require you to do. As an example, you
  could build a node.js server inside one of these.

#### `yarn modular start <app-name>`

Runs
[`react-scripts start`](https://create-react-app.dev/docs/getting-started#npm-start-or-yarn-start)
against the selected app.

#### `yarn modular test`

Runs [`react-scripts test`](https://create-react-app.dev/docs/running-tests)
against the entire `modular` project.

#### `yarn modular build <app-name>`

Runs [`react-scripts build`](https://create-react-app.dev/docs/production-build)
against the selected app.

## Config

#### `modular/setupTests.ts`

This contains the setup for tests (e.g.
[`jest.config.js#setupFilesAfterEnv`](https://jestjs.io/docs/en/configuration#setupfilesafterenv-array)).

Unlike `create-react-app` which stores the setup for tests in an individual
application's
[`src/setupTests.ts`](https://create-react-app.dev/docs/running-tests/#srcsetuptestsjs)
we place it at the root of the project within `modular/setupTests.ts` where it
applies to the whole project.

#### `package.json#modular`

_NOTE: This property is created automatically and is described here for
reference only._

_e.g._

```json
{
  "modular": {
    "type": "root"
  }
}
```

The `package.json#modular.type` can be `"root"`, `"app"`, `"view"` or
`"package"`.

##### `"root"`

This type identifies the root of the project.

##### `"view"`

This type identifies modules that export a single React component as their
default export. `modular` makes these modules available via a dynamically
generated view map with `modular-views.macro`. Read more about Views in
[this explainer](/docs/views.md).

##### `"app"`

This type identifies a standalone application that can be started or built.

##### `"package"`

This type identifies a regular package (e.g. a library that can be used by other
`"view"` or `"app"` modules). s a regular package (e.g. a library that can be
used by other `"view"` or `"app"` modules).
