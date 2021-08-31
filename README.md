# Monorepo with Lerna & Yarn Workspaces


-   🐉 [Lerna](https://lernajs.io/)  - The Monorepo manager
-   📦 [Yarn Workspaces](https://yarnpkg.com/lang/en/docs/workspaces/)  -  Sane multi-package management
-   🛠 [Babel](https://babeljs.io/)  -  Compiles next-gen JavaScript
-   🃏 [Jest](https://jestjs.io/)  -  Unit/Snapshot Testing

## Usage

-   `yarn dev` - This starts Storybook for viewing all the components locally.
-   `yarn bootstrap` - This installs all of the packages and links dependent packages together.
-   `yarn build` - This babelfies all of the packages and creates `/lib` folders for each one.
-   `yarn test` - Run all linting and unit tests before committing.
-   `yarn test -o` - Run only the tests that have changed.
-   `yarn test -u` - Update all of the snapshot tests.

## Lerna

-   `lerna changed` - Show which packages have changed.
-   `lerna diff` - Show specifically what files have cause the packages to change.

## Linking

When linking inside of the Monorepo, everything works as expected. If you are trying to consume packages from this Monorepo _in a different application_ locally, using `npm link` or `yarn link` [does not work as expected](https://github.com/yarnpkg/yarn/issues/5538). However, we have a workaround for the time being.

1. Run `yarn build`
1. Run `yarn dev`
1. Change the `package.json` of the consumer from `$YOUR_PACKAGE_NAME` (which lives inside the monorepo) to `file:./../monorepo/packages/$YOUR_PACKAGE_NAME`
1. Run `rm -rf node_modules && yarn` in the consumer
1. 🎉

## FAQ

### Why the limitation on yarn versions?

[It's a known issue](https://github.com/yarnpkg/yarn/issues/7807) that yarn workspaces using yarn versions > `1.18.0` can produce the following false positive error message when adding or updating dependencies in packages.

```
error An unexpected error occurred: "expected workspace package to exist for "XXX".
```

To guard against this, we've:

-   Changed `package.json` to enforce a yarn version range.
-   Added a `.yvmrc` file, so if you're using [yvm](https://yvm.js.org/docs/overview) to manage your yarn versions (like [nvm](https://github.com/nvm-sh/nvm) for node version), it'll pick up the yarn version automatically.
