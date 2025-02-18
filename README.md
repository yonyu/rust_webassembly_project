# Creating a Rust Web Assembly project

Before Rust version 1.82, it is easy to create a Rust WebAssembly project by using rust-webpack template. You simply follow the steps below:

```
md my_proj
cd my_proj
npm init rust-webpack
npm install
npm run start
```

But if you updated to Rust version 1.82+, you will see some errors while following the above steps:

*Error: Internal failure: parseVec could not cast the value......*

In this repo, I'll give the correct steps to create a new Rust Web Assembly project for the following versions:

* rust stable: 1.84.1+
* wasm-pack: 0.13.1+

I'll give update for the future versions in case if there is any change required.


## How to set up the project

Versions used:

* rust stable: 1.84.1
* wasm-pack: 0.13.1

Steps

- Create the project folder
  > mkdir my_proj
- Enter the project folder
  > cd my_proj
- Initialize the project
  > npm init rust-webpack
- Update the dependent packages
  > npm install webpack@latest webpack-cli@latest webpack-dev-server@latest --save-dev
- Tune the configuration files (refer to the commit "Tune the configuration files" for details)
  > package.json
  > "start": "rimraf dist pkg && webpack-dev-server --open -d" =>
  > "start": "rimraf dist pkg && webpack-dev-server --open --devtool source-map",

  > webpack.config.js file

  Replace devServer and plugins sections with the new content below:

  ```
  devServer: {
    static: {
      directory: dist,
    },
  },
  experiments: {
    asyncWebAssembly: true,
  },
  module: {
    rules: [
      {
        test: /\.wasm$/,
        type: "webassembly/async",
      }
    ]
  },
  plugins: [
    new CopyPlugin(
      [
        { from: path.resolve(__dirname, "static"), to: dist }
      ],
    ),

    new WasmPackPlugin({
      crateDirectory: __dirname,
    }),
  ]
  ```

## How to install

```sh
npm install
```

## How to run in debug mode

```sh
# Builds the project and opens it in a new browser tab. Auto-reloads when the project changes.
npm start
```

## How to build in release mode

```sh
# Builds the project and places it into the `dist` folder.
npm run build
```

## How to run unit tests

```sh
# Runs tests in Firefox
npm test -- --firefox

# Runs tests in Chrome
npm test -- --chrome

# Runs tests in Safari
npm test -- --safari
```

## What does each file do?

* `Cargo.toml` contains the standard Rust metadata. You put your Rust dependencies in here. You must change this file with your details (name, description, version, authors, categories)

* `package.json` contains the standard npm metadata. You put your JavaScript dependencies in here. You must change this file with your details (author, name, version)

* `webpack.config.js` contains the Webpack configuration. You shouldn't need to change this, unless you have very special needs.

* The `js` folder contains your JavaScript code (`index.js` is used to hook everything into Webpack, you don't need to change it).

* The `src` folder contains your Rust code.

* The `static` folder contains any files that you want copied as-is into the final build. It contains an `index.html` file which loads the `index.js` file.

* The `tests` folder contains your Rust unit tests.
