Before Rust version 1.82, it is easy to create a Rust/Webpack project
using rust-webpackt template. You simply follow the steps below:

    md my_proj
    cd my_proj
    npm init rust-webpack
    npm install
    npm run start

But if you updated to Rust version 1.82, you will see the following message:

    ERROR in ./pkg/index_bg.wasm
    Module parse failed: Internal failure: parseVec could not cast the value
    You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
    Error: Internal failure: parseVec could not cast the value
        at parseVec (C:\Users\Yong\RustProjects\myproj\node_modules\@webassemblyjs\wasm-parser\lib\decoder.js:343:15)
        at parseTypeSection (C:\Users\Yong\RustProjects\myproj\node_modules\@webassemblyjs\wasm-parser\lib\decoder.js:375:22)
        at parseSection (C:\Users\Yong\RustProjects\myproj\node_modules\@webassemblyjs\wasm-parser\lib\decoder.js:1379:23)
        at Object.decode (C:\Users\Yong\RustProjects\myproj\node_modules\@webassemblyjs\wasm-parser\lib\decoder.js:1740:25)
        at decode (C:\Users\Yong\RustProjects\myproj\node_modules\@webassemblyjs\wasm-parser\lib\index.js:253:21)
        at WebAssemblyParser.parse (C:\Users\Yong\RustProjects\myproj\node_modules\webpack\lib\wasm-async\AsyncWebAssemblyParser.js:61:19)
        at C:\Users\Yong\RustProjects\myproj\node_modules\webpack\lib\NormalModule.js:1303:19
        at processResult (C:\Users\Yong\RustProjects\myproj\node_modules\webpack\lib\NormalModule.js:937:11)
        at C:\Users\Yong\RustProjects\myproj\node_modules\webpack\lib\NormalModule.js:1030:5
        at C:\Users\Yong\RustProjects\myproj\node_modules\loader-runner\lib\LoaderRunner.js:407:3
        at iterateNormalLoaders (C:\Users\Yong\RustProjects\myproj\node_modules\loader-runner\lib\LoaderRunner.js:233:10)
        at C:\Users\Yong\RustProjects\myproj\node_modules\loader-runner\lib\LoaderRunner.js:224:4
        at C:\Users\Yong\RustProjects\myproj\node_modules\webpack\lib\NormalModule.js:984:15
        at Array.eval (eval at create (C:\Users\Yong\RustProjects\myproj\node_modules\tapable\lib\HookCodeFactory.js:33:10), <anonymous>:12:1)
        at runCallbacks (C:\Users\Yong\RustProjects\myproj\node_modules\enhanced-resolve\lib\CachedInputFileSystem.js:45:15)
        at C:\Users\Yong\RustProjects\myproj\node_modules\enhanced-resolve\lib\CachedInputFileSystem.js:279:5
    @ ./pkg/index.js 1:0-40 4:15-19 5:0-21
    @ ./js/index.js

The problem is caused by the fact: 

Starting with Rust 1.82 reference-types is always present in the target_features section. The problem is that JS bundlers like Webpack can't currently handle the reference-types proposal.

Below is the instructions how to avoid the problem. Beware there are other ways. 

# Instructions

- Following the [instructions](https://www.rust-lang.org/learn/get-started) to install Rust if not yet

- Downgrade your Rust installation to version 1.81 if you have version 1.82 installed
> rustup install 1.81

- Reset the default Rust 1.81 if you set the default to nightly
> rustup default 1.81-x86_64-pc-windows-msvc

- Add a new target
> rustup target add wasm32-unknown-unknown

- to install wasm pack, a tool to work with rust generated web assembly
> cargo install wasm-pack

- Create a folder and project

> mkdir my_proj
> cd my_proj
>
> npm init rust-webpack

- Update webpack to version 5
> npm install webpack@latest webpack-cli@latest webpack-dev-server@latest --save-dev

- modify the package.json and webpack.config.js files

- Start the app. Then wait until the build is complete.
> npm run start

From your browser, for URL, http://localhost:8080/.
Open Developer tools, locate the console. You should see
"Hello world!".

Congratulations! You have successfully created a default Rust/Webpack project
using rust-webpackt template!