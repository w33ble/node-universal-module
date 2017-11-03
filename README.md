# Universal Module

> An attempt to ship an npm module that works well anywhere

I want to run es6 modules (esm) transparently in both node 6 and node 8. Using [@std/esm](https://www.npmjs.com/package/@std/esm) makes this possible.

## Why

Node 8.5.0+ has support for es6 modules (esm), at least behind a flag. Node 8 is also the LTS version now, which means it's the version everyone is going to be expected to run in the very near term. But node 6 is still going ot be supported for a while, and it'll probably be a long time before existing applications can be updated from 6 to 8, so it's reasonable to expect to support both for a long time.

Currently, most people ship es6 modules on npm by tranpiling them into es5. This works well enough in node, but it kind of sucks in the browser, since you lose tree shaking in modules, and when you package up all your code the resulting bundle ends up being huge.

I think the future of npm modules in the browser probably means transpilation and bundling your node modules along with your own code. The [pkg.module](https://github.com/rollup/rollup/wiki/pkg.module) is a step towards this, which means you still ship your original code and allow bundlers to bundle your module code.

## Caveats

You still probably want to transpile your code to it can be used directly in the browser, or possibly even in older version of node. [This post about publishing es6 modules](https://medium.com/@tarkus/how-to-build-and-publish-es6-modules-today-with-babel-and-rollup-4426d9c7ca71) gives you an idea, where you publish your core module code the way you wrote it, but you also bundle up version for the browser and other legacy systems.

Writing a "universal module" means you still have to code to the lowest common denominator, which would be node 6. This means you can't use async/await, object spread, or anything new in in node 8. I think this is acceptable personally, but you could also opt to bundle a node 6 version in your dist path. Then users of older version of node simply `require('your-package/dist/node6')` or something along those lines.