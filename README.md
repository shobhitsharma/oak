# oak
Architecture for Java backend with React in client-side.

## Stack

- [Webpack](https://github.com/webpack/webpack) to bundle all the
  JavaScript and dependencies, plus LESS + CSS handling.
- [Babel](https://babeljs.io/) for ES6 syntax, using Babel 6 with the "es2016" and "react" presets.
- [Hot module reloading
  (HMR)](https://github.com/gaearon/react-transform-hmr) of React components
- [Redux](https://github.com/rackt/redux) to manage state, both in the
  client and when rendering on the server.
- [react-router](https://github.com/rackt/react-router) for page routing,
  on client and server. Note that this is version 4, with a very different (and
  simpler) API to previous versions.
- Linting integrated with Webpack via [eslint](https://github.com/MoOx/eslint-loader).
- Type checking with [Flow](https://flowtype.org/).

## Development

Execute `mvn` if you have Maven already installed, or `./mvnw` if you don't. You'll need
[Java8 installed](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) either way at
a minimum version of `1.8.0_65`. Older versions have a bug that makes rendering
brutally slow.

Run webpack in hot-module reloading mode with: `npm start`.

## Resources

- [Project Lombok](https://projectlombok.org/)
- [Jackson](https://github.com/FasterXML/jackson) to serialize model data before rendering on the server. For more information, see
  [this OpenJDK thread on the subject](http://mail.openjdk.java.net/pipermail/nashorn-dev/2013-September/002006.html)

## Conventions

Controllers that render views are suffixed with `Controller`. REST endpoints are
suffixed with `Resource`, and handle requests under `/api`.

## Testing the webpack bundle

In order to pre-empty runtime errors with Nashorn loading the bundle, a test
script is executed by Maven during the `test-compile` phase, located at
`src/test/js/test_bundle.js`. If this script fails, you can diagnose the problem
by:

* Running a debug build with `npm run debug`. This runs webpack in a production
  mode but without uglification.
* Run the script again: `jjs src/test/js/test_bundle.js`
* Look at the line in the bundle from the stacktrace and figure out the problem.

It's easy to create a bundle that's broken on the server by including code that
expects a DOM - and that includes the Webpack style loader. This is the root of
most problems. You should note that server-side rendering *does not* require a
DOM - which is why `src/main/resources/static/js/polyfill.js` doesn't provide
any `window` or `document` stubs.
