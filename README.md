# talib-web

[TA-Lib](https://ta-lib.org/), the technical analysis library written in C, ported to WebAssembly. Plus a nice API wrapper layer, typescript support and docs.

**talib-web** is based on [talib.js](https://github.com/hackape/talib.js),and make some changes to using it easier.

## Installation

```
npm install --save talib-web
```

## Usage

### Normal

```js
const talib = require("talib-web");

const inReal = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

async function main() {
  await init(/* optionally pass in .wasm file path here */);
  console.log(talib.ADD({ inReal0: inReal, inReal1: inReal }));
}

main();
```

Or:

```js
import { init, ADD } from "talib-web";

async function main() {
  await init(/* optionally pass in .wasm file path here */);
  const inReal = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  console.log("talib.ADD result:", ADD({ inReal0: inReal, inReal1: inReal }));
}

main();
```

output:
```js
{ output: [
    2,  4,  6,  8, 10,
  12, 14, 16, 18, 20
] }
```

### Use TAFuncs object

```js
import { init,  TAFuncs } from "talib-web";

async function main() {
  await init(/* optionally pass in .wasm file path here */);
  const inReal = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
  console.log("TAFuncs.ADD result:", TAFuncs.ADD({ inReal0: inReal, inReal1: inReal }));

  //you can call other talib function easily without import,just like TAFuncs.MA()
}
```


## Documentation

Visit https://ancheGT.github.io/talib-web/

Docs are generated using [typedoc](https://github.com/TypeStrong/typedoc) and hosted on [GitHub Pages](https://github.com/ancheGT/talib-web/tree/gh-pages).


## Develop

**develop environment:**

- OS: CentOS 7
- node: v14.15.5
- NPM: 6.14.11
- emscripten: 2.0.34
- tsc: 4.5.2

**dependencies installation**

- [emscripten](https://emscripten.org/docs/getting_started/downloads.html)

- npm 

	```sh
	npm install typescript -g
	npm install
	```

**build:**

```sh
make clean # clean temp
make
```

**branchs**

- master-hackape: original repo of **talib.js**

- master-new: current repo

- gh-pages: github pages, after `make` branch **master-new**, copy the **docs** dir to this branch.It's relies on the setting of **GitHub Pages ->  Source**

## Troubleshooting:

- in terminal, run **make** got: `error while loading shared libraries: libatomic.so.1: cannot open shared obj`
	- run command: `yum -y install libatomic`

- in browser console,got: 

	```
	index.esm.js?80e5:561 
			
		Uncaught (in promise) RuntimeError: abort(CompileError: WebAssembly.instantiate(): expected magic word 00 61 73 6d, found 3c 21 44 4f @+0). Build with -s ASSERTIONS=1 for more info.
		at abort (webpack-internal:///./node_modules/talib-web/lib/index.esm.js:886)
		at eval (webpack-internal:///./node_modules/talib-web/lib/index.esm.js:962)
	index.esm.js?80e5:2146 
			
		Uncaught (in promise) Error: TA-Lib WASM runtime init fail.
	Error: 
	abort(CompileError: WebAssembly.instantiate(): expected magic word 00 61 73 6d, found 3c 21 44 4f @+0). Build with -s ASSERTIONS=1 for more info.
		at eval (webpack-internal:///./node_modules/talib-web/lib/index.esm.js:2471)
	```
	- Pass a URL param to init,like this: `await init("https://unpkg.com/talib-web@0.1.2/lib/talib.wasm");`

	- You can host this file yourself if you like. It’s just a static asset, like a picture, nothing magical.

	- If you decide to host it, 2 things to check:

    	- Make sure the URL is accessible from your origin, e.g. double check CORS config if involved.
    	- Make sure the MIME type is correctly set to `application/wasm`.
