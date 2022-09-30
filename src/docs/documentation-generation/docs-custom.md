---
title: Custom Docs Generation
description: Custom Docs Generation
url: /docs/docs-custom
contributors:
  - adamdbradley
  - manucorporat
  - arayik-yervandyan
  - rwaskiewicz
---

# Custom Docs Generation

Stencil exposes an output target titled `docs-custom` where users can access the generated docs json data.
This feature can be used to generate custom markdown or to execute additional logic on the comment metadata during the build process.

To make use of this output target, add the `docs-custom` output target to your Stencil configuration.

```tsx
import { Config } from '@stencil/core';

export const config: Config = {
  outputTargets: [
    {
      type: 'docs-custom',
      generator: (docsJsonData) => {
          // Custom logic goes here
      },
      strict: true
    }
  ]
};
```

## Configuring `docs-custom`

`docs-custom` is able to be configured in a Stencil `config` with the following properties:

| Property    | Description                                                                                                     | Default |
|-------------|-----------------------------------------------------------------------------------------------------------------|---------|
| `generator` | A function that accepts a `JsonDocs` argument to perform perform custom logic with.                             | N/A     |
| `strict`    | An optional field that if set to `true`, Stencil will output a warning whenever there is missing documentation. | `false` |

# Custom Docs Data Model

## JsonDocs

The generated docs JSON data that a `generator` function will operate on will be the type of `JsonDocs`.
It contains metadata for each component in the project, as well as information used to determine which versions of Stencil/TypeScript generated the metadata.

| Property     | Description                                                                                                                                           |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| `compiler`   | An `Object` with `typescriptVersion` (the current TypeScript version), `compiler` (the Stencil compiler name), and `version` (the version of Stencil) |
| `components` | Array with type of `JsonDocsComponent[]` which contains metadata for each component in a project                                                      |
| `timestamp`  | A `string` containing the current timestamp with a format of `YYYY-MM-DDThh:mm:ss`                                                                    |

## JsonDocsComponent

`JsonDocsComponent` contains the metadata for a single component in a project

| Property          | Description                                                                        |
|-------------------|------------------------------------------------------------------------------------|
| `dirPath`         | The directory containing a Stencil component                                       |
| `fileName`        | The name of the file containing the Stencil component, with no path                |
| `filePath`        | The full path of the file containing the Stencil component                         |
| `readmePath`      | The path to the component's `readme.md` file                                       |
| `usagesDir`       | The path to the component's `usage/` directory                                     |
| `encapsulation`   | A component's `encapsulation` type. Possible values are `shadow`, `scoped`, `none` |
| `tag`             | The component's tag, as set in the `@Component` decorator                          |
| `readme`          | The contents of a component's `readme.md` file that are user-generated             |
| `docs`            | The JSDoc description written atop a component's `@Component` decorator            |
| `docsTags`        | JSDoc tags found in the JSDoc written atop a component's `@Component` decorator    |
| `usage`           | An object that maps the name of a markdown file in `usage` to its contents         |
| `props`           | Array of metadata for a component's `@Prop`s                                       |
| `methods`         | Array of metadata for a component's `@Methods`s                                    |
| `events`          | Array of metadata for a component's `@Events`s                                     |
| `listeners`       | Array of metadata for a component's `@Listeners`s                                  |
| `styles`          | Array of metadata for a component's CSS styling information                        |
| `slots`           | Array of component Slot information, generated from `@slot` tags                   |
| `parts`           | Array of component Parts information, generate from `@part` tags                   |
| `dependents`      | Array of metadata describing where the current component is used                   |
| `dependencies`    | Array of metadata listing the components which are used in current component       |
| `dependencyGraph` | Describes a tree of components coupling                                            |

