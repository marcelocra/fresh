---
description: Plugins can add new functionality to Fresh without requiring significant complexity.
---

Plugins can dynamically add new functionality to Fresh without exposing
significant complexity to the user. Users can add plugins by importing and
initializing them in their `main.ts` file:

```ts
// main.ts

// ...

import twindPlugin from "$fresh/plugins/twind.ts";
import twindConfig from "./twind.config.js";

await start(manifest, {
  plugins: [
    // This line configures Fresh to use the first-party twind plugin.
    twindPlugin(twindConfig),
  ],
});
```

Currently, the only available first-party plugin is the Twind plugin.

## Creating a plugin

Fresh plugins are in essence a collection of hooks that allow the plugin to hook
into various systems inside of Fresh. Currently only a `render` hook is
available (explained below).

A Fresh plugin is just a JavaScript object that conforms to the
[Plugin](https://deno.land/x/fresh/server.ts?s=Plugin) interface. The only
required property of a plugin is it's name. Names must only contain the
characters `a`-`z`, and `_`.

```ts
import { Plugin } from "$fresh/server.ts";

const plugin: Plugin = {
  name: "my_plugin",
};
```

A plugin containing only a name is technically valid, but not very useful. To be
able to do anything with a plugin, it must register some hooks.

### Hook: `render`

The render hook allows plugins to intercept rendering -  
