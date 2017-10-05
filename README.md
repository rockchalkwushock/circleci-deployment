# circleci-deployment :black_circle: :rocket: :alien: :metal:

[![CircleCI](https://img.shields.io/circleci/project/github/rockchalkwushock/circleci-deployment.svg?style=flat-square)](https://circleci.com/gh/rockchalkwushock/circleci-deployment)
[![Codecov](https://img.shields.io/codecov/c/github/rockchalkwushock/circleci-deployment.svg?style=flat-square)](https://codecov.io/gh/rockchalkwushock/circleci-deployment)
[![NSP Status](https://nodesecurity.io/orgs/rcws-development/projects/fbd7dfdd-7089-4b9d-adf7-a5cf48513780/badge)](https://nodesecurity.io/orgs/rcws-development/projects/fbd7dfdd-7089-4b9d-adf7-a5cf48513780)
[![Known Vulnerabilities](https://snyk.io/test/github/rockchalkwushock/circleci-deployment/badge.svg)](https://snyk.io/test/github/rockchalkwushock/circleci-deployment)

[![Create-React-App](https://img.shields.io/badge/made%20with-create--react--app-blue.svg?style=flat-square)](https://github.com/facebookincubator/create-react-app)
[![Now](https://img.shields.io/badge/deployed%20with-now--cli-orange.svg?style=flat-square)](https://github.com/zeit/now-cli)

[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg?style=flat-square)](http://commitizen.github.io/cz-cli/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://github.com/rockchalkwushock/circleci-deployment/pulls)
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![Twitter URL](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/RockChalkDev)

[![Alpha](https://img.shields.io/badge/alpha-release-orange.svg?style=flat-square)](https://circleci-deployment-alpha.now.sh)
[![Beta](https://img.shields.io/badge/beta-release-yellow.svg?style=flat-square)](https://circleci-deployment-beta.now.sh)

## Notes

1. React 16 Environment

With the React 16 the requirement of some polyfills are needed for production and testing environments. Read more [here](https://reactjs.org/docs/javascript-environment-requirements.html) and in the original [gist](https://gist.github.com/gaearon/9a4d54653ae9c50af6c54b4e0e56b583). This is why the `raf` package has been included and the reasoning behind `src/setupTests.js` (read more about that in the official `create-react-app` docs [here](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#initializing-test-environment)).

2. Jest Snapshot Testing on Continuous Integration

The `test:coverage` script has the `--updateSnapshot` flag because `jest` no longer explicitly updates the snapshots, read more [here](http://facebook.github.io/jest/docs/en/snapshot-testing.html#snapshots-are-not-written-automatically-on-continuous-integration-systems-ci) (I know we are doing no snapshot tests in this, but it's worth noting this).

3. Jest configuration with `create-react-app`

Barring running a `yarn eject` to be able to have more control these are the limitations you will run into at the time of writing this:

When adding `"jest"` to the `package.json` you can only pass the following configuration options (you will get a very pretty error message if you add more):

- `collectCoverageFrom`
- `coverageReporters`
- `coverageThreshold`
- `snapshotSerializers`

All other options must be passed through the `jest` [CLI flags](http://facebook.github.io/jest/docs/en/cli.html#content). A particularly useful flag for seeing the underlying configuration being set by `create-react-app` is the `--showConfig` flag that will exit with the output of the configuration. This can be seen by running `yarn test:config`.

4. Building & Testing `create-react-app` using Continuous Integration

As per the [documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#circleci) it is required that for both the build and testing that we pass `CI=true`.

## `devDependencies` & configurations

- `codecov`: [Read more on codecov here.](https://github.com/codecov/codecov-node)
- `commitizen`: [Read more on commitizen here.](https://github.com/commitizen/cz-cli)
- `cz-conventional-changelog`: [Read more on cz-conventional-changelog here.](https://github.com/commitizen/cz-conventional-changelog)
- `husky`: [Read more on husky here.](https://github.com/typicode/husky)
- `lint-staged`: [Read more on lint-staged here.](https://github.com/okonet/lint-staged)
- `nsp`: [Read more on nsp here.](https://github.com/nodesecurity/nsp)
- `prettier`: [Read more on prettier here.](https://github.com/prettier/prettier)
- `raf`: [Read more on raf here.](https://github.com/chrisdickinson/raf)

1. With `commitizen` we must declare the path to the adapter that is going to be used.

2. `create-react-app` does not look for a `jest.config.json` so we must declare our configuration in the `package.json` under the `jest` object.

3. `lint-staged` requires us to tell it what files should be looked at in the repository and accepts an array of commands to be executed. It is important to run `git add` last because we are running `lint-staged` on the `husky` managed `precommit` git hook. If you do not put this command as the last index of the array all files changed by the `prettier` formating or other scripts in the array will no longer be staged for committing.

4. We can either use a `now.json` or declare a `now` config object in the `package.json`. Here we declare the 3 possible aliases, the only files to be sent to the cloud, and what to name the deployment instance.

5. As per #1 under Notes `react@16` requires some polyfills now. `raf` is added to the `/src/setupTests.js` file as `import 'raf/polyfill'`.

```json
{
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  },
  "jest": {
    "collectCoverageFrom": ["src/App.js"],
    "coverageThreshold": {
      "global": {
        "branches": 100,
        "functions": 100,
        "lines": 100,
        "statements": 100
      }
    }
  },
  "lint-staged": {
    "*.js": ["yarn prettier", "git add"]
  },
  "now": {
    "alias": [
      "circleci-deployment-alpha",
      "circleci-deployment-beta",
      "circleci-deployment-production"
    ],
    "files": ["./build"],
    "name": "circleci-deployment"
  },
}
```
