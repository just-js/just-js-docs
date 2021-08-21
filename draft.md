###################################

- bookmark
  - start here: just/config.js
  - next up
    - run through the examples
    - start just and experiment
    - sync with billy

- everything in this file is in draft mode as i ramp up on just
- general thoughts (sync with billy on these)
  - documentation structure, syntax, nomenclature
    - its really for building software on linux, should lean to the linux distros paradigm, or js?
  - jsdocs/flowtype/ts ?
    - would help in auto generating a lot of information
  - since the `docs` and `source code` are kept separately, whats the most effective way of tightly coupling them?
    - map `just-js/just/**/*/dir` to `just-js/docs/**/*/README.md`
    - perhaps the file file structure wouldnt matter as long as the documentation is deeplinked to the source code
  - is it `justjs` or just `just`
  - whats the high-level 10000 foot architecture of just?
- as items are approved move them over to `readme.md`

####################################

# TLDR

- [Getting Started: checkout just-js/just/readme.md](https://github.com/just-js/just)
- [Building just-js/just](https://github.com/just-js/just/blob/main/BUILD.md)
- [Developing just-js/just with gitpod](https://github.com/just-js/just/blob/main/CONTRIBUTING.md)
- [Setting up VSCode to work with just-js/just](https://github.com/just-js/just/blob/main/VSCode.md)

## vscode

- [syntax highlighting for .S files](https://marketplace.visualstudio.com/items?itemName=dan-c-underwood.arm)

## environment variables

<details>

<summary> Quick Reference </summary>
<ul>
<li>JUST_HOME=$(pwd)/just</li>
<li>JUST_TARGET=$JUST_HOME</li>
</ul>

</details>

## architecture

various github repositories make up `just-js`

<details>

<summary> <b><code>/just</code></b> </summary>

- `/deps`
- `/lib`
  - `/fileX`
- `/modules`

</details>

<details>

<summary> <b><code>/docs</code></b> </summary>

- `/readme.md` concise information, should point to other files

</details>

</details>

<details>

<summary> <b><code>/examples</code></b> </summary>

- `{folderX}/{fileX}` flattens `/just/{lib,modules}/*` and provides reference implementation for each?

</details>
  
<details>

<summary> <b><code>/modules</code></b> </summary>

TODO: internal, blessed (foss), wanted

</details>
  
<details>

<summary> <b><code>/docker</code></b> </summary>

TODO: docker image and container builds

</details>

<details>

<summary> <b><code>/libv8</code></b> </summary>

TODO: TODO

</details>

<details>

<summary> <b><code>/v8headers</code></b> </summary>

TODO: TODO

</details>

## comparison with other v8 based javascript runtimes

[todo: read this](https://www.codecademy.com/articles/introduction-to-javascript-runtime-environments)

| COMPONENT | just | chromium* | deno | nodejs |
| :-------: | :-------: | :-------: | :-------: |:-------: |
USECASE | ... | ... | ... | ... |
ENVIRONMENT | ... | ... | ... | ... |
MODULES | ... | ... | ... | ... |
etc | ... | ... | ... | ... |

## style guide

- TODO: check how high level style of the soruce code & sync with billy

## API contract

```bash
# log of things found in source code & examples
# importing
  # import shared library at default path?
  const { epoll } = just.library('epoll')

  # ^ use some other path
  const { epoll } = just.library('epoll', './override-epoll-with-this.so')

# root/just.js
  # wrapRequire
  const appRoot = just.sys.cwd()
  const { HOME, JUST_TARGET } = just.env()
  const justDir = JUST_TARGET || `${HOME}/.just`
  # load the builtin modules
  just.vm = library('vm').vm
  just.loop = library('epoll').epoll
  just.fs = library('fs').fs
  just.net = library('net').net
  just.sys = library('sys').sys
  just.env = wrapEnv(just.sys.env)
  # overloads ArrayBuffer
  # appears to have plans to do the same for SharedArrayBuffer
  delete global.console # whats this about
  global.process = {
      pid: just.sys.pid(),
      version: 'v15.6.0',
      arch: 'x64',
      env: just.env()
    }
  # function main has all the goodies


# just API contract: just is global? check root/config.js
  const version = just.version.just
  const debug = false # override this via env var?

  just
    # Setup the target ObjectTemplate with 'just' property which holds the
    # ^ basic functions in the runtime core @see root/just.h
    # ^ where do the other fns come from? e.g. in root/just.js` theres a just.sys.X call
    .builtin()
    .chdir()
    .error()
    .exit()
    .load()
    .memoryUsage()
    .pid()
    .print()
    .sleep()
    .version
    .version.just
    .version.kernel
    .version.kernel.os
    .version.kernel.release
    .version.kernel.version
    .version.v8
    # root/just.js
    .args
    .clearTimeout 
    .config
    .cpuUsage 
    .encode
    .env()
    .factory 
    .factory.loop
    .fs
    .heapUsage 
    .inspector
    .library 
    .loop
    .memoryUsage
    .net
    .net.setNonBlocking 
    .opts
    .path
    .process
    .require
    .require.cache
    .requireNative 
    .rUsage 
    .setInterval
    .setTimeout 
    .sha1
    .sys
    .sys.cwd
    .sys.dlclose
    .sys.dlopen
    .sys.dlsy
    .SystemError 
    .workerSource
    .vm

# global object
  global
    .process
    .require
    .inspector

```
