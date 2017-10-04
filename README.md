# circleci-deployment :black_circle: :rocket: :alien: :metal:

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

## Notes

1. React 16 Environment

With the React 16 the requirement of some polyfills are needed for production and testing environments. Read more [here](https://gist.github.com/gaearon/9a4d54653ae9c50af6c54b4e0e56b583). This is why the `raf` package has been included and the reasoning behind `src/setupTests.js` (read more about that in the official `create-react-app` docs [here](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#initializing-test-environment)).

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
