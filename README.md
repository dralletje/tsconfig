# @dral/tsconfig

My ready-to-go tsconfigs.
Not meant for compilation using `tsc`, instead for `node`'s typestripping or when building with a bundler.
These configs are for universal modules, so no node or DOM types.
To provide things like `setTimeout` and `TextEncoder`, which are not actually part of ecmascript itself, but are generally available I use `"lib": ["WebWorker"]`, which seems like a nice compromise.

### `@dral/tsconfig/modern`

Setting up the types and module resolution

### `@dral/tsconfig/reasonable`

My opinionated config defining what should be errors and what should not.
Most of all I don't make unused variables error, because... they aren't errors!
