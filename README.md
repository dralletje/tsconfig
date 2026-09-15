# @dral/tsconfig

My ready-to-go tsconfigs.
Not meant for compilation using `tsc`, instead for `node`'s typestripping or when building with a bundler.
These configs are for universal modules, so no node or DOM types.
To provide things like `setTimeout` and `TextEncoder`, which are not actually part of ecmascript itself, but are generally available I use `"lib": ["WebWorker"]`, which seems like a nice compromise.

## Quick start

### Universal package

```jsonc
{
  "extends": [
    "@dral/tsconfig/modern",
    "@dral/tsconfig/universal",
    "@dral/tsconfig/reasonable",
  ],
  "compilerOptions": {},
  "include": ["src"],
}
```

### Node package

```jsonc
{
  "extends": [
    "@dral/tsconfig/modern",
    "@tsconfig/node24",
    "@dral/tsconfig/reasonable",
  ],
  "compilerOptions": {},
  "include": ["src"],
}
```

### Composite (universal package + tests/configs)

Use this template to mix and match different types.
If you have other files running like configs, I recommend either
adding a separate `tsconfig.node.json`, or whatever environment the
code runs in.

```jsonc
/// tsconfig.json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.universal.json" },
    { "path": "./tsconfig.test.json" },
  ],
}
```

```jsonc
/// tsconfig.universal.json
{
  "extends": [
    "@dral/tsconfig/modern",
    "@dral/tsconfig/universal",
    "@dral/tsconfig/reasonable",
    "@dral/tsconfig/composite",
  ],
  "compilerOptions": {},
  "include": ["src"],
  "exclude": ["**/*.test.ts"],
}
```

```jsonc
/// tsconfig.test.json
// {
  "extends": [
    "@dral/tsconfig/modern",
    "@tsconfig/node24",
    "@dral/tsconfig/reasonable",
    "@dral/tsconfig/composite",
  ],
  "compilerOptions": {},
  "include": ["tests", "src/**/*.test.ts"],
  /// Reference to the universal config so we can import the package
  "references": [{ "path": "./tsconfig.universal.json" }],
}
```

## Configs

### `@dral/tsconfig/modern`

Setting up module resolution:

- Sets `noEmit: true` because it doesn't work with `.ts` extensions
- Forces using exact extensions (`.ts`, generally)
- No typescript-only syntax (e.g. enums)

### `@dral/tsconfig/reasonable`

My opinionated config defining what should be errors and what should not.
Most of all I don't make unused variables error, because... they aren't errors!

### `@dral/tsconfig/universal`

Currently really just `"lib": ["ES2024", "WebWorker"]`, but I might add some extra `"types": [...]` for baseline included features (I'm considering browser, Node, Cloudflare Workers, e.g.)

### `@dral/tsconfig/composite`

The odd one out.
If you want to use a composite config, things get tricky with emitting and path rewriting, yadi yadi yada.
This config makes tsc emit declarations (necessary for references between composite tsconfigs), but sets the build directory to `./node_modules/.cache/@dral/tsconfig/builds`, so very much out of the way.
