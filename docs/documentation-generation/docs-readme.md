---
title: Docs Readme Auto-Generation
sidebar_label: README Docs (docs-readme)
description: README Docs
slug: /docs-readme
---

# Docs Readme Markdown File Auto-Generation

Stencil is able to auto-generate `readme.md` files in markdown.
This can help you to maintain consistently formatted
documentation for your components which live right next to them and render in
GitHub.

To auto-generate readme files, add the `docs-readme` output target to your
`stencil.config.ts` like so:

```tsx title="stencil.config.ts"
import { Config } from '@stencil/core';

export const config: Config = {
  outputTargets: [
    { type: 'docs-readme' }
  ]
};
```

[//]: # (TODO: Differentiate between when the docs-readme OT exists and when it does not for these next two options)
Another option would be to use the `--docs` CLI flag as a part of the [build task](../config/cli.md#stencil-build), like so:

```bash
stencil build --docs
```

This will cause the Stencil compiler to perform a one-time generation of README
files as a part of the build process.

:::note
If you don't add the `docs-readme` output target to your Stencil configuration, the `--docs` flag must be used to regenerate documentation.
:::

Alternatively, the [docs task](../config/cli.md#stencil-docs) can be used to perform a one time generation of the documentation:
```bash
stencil docs
```

## How-To Add:
### Custom Markdown Content

Once you've generated a `readme.md` file, you can add your own
markdown content. You may add any content above the following comment in a component's `readme.md`:
`<!-- Auto Generated Below -->`.

The content that you author above this comment will be persisted on subsequent builds of the README file.

### A Deprecation Notice

A Stencil component may be marked as deprecated using the [JSDoc `@deprecated` tag](https://jsdoc.app/tags-deprecated).
By placing `@deprecated` in a component's class-level JSDoc will cause the generated README to denote the component is deprecated.

```tsx title="my-component.tsx with @deprecated"
/**
 * A simple component for formatting names.
 * 
 * @deprecated since v2.0.0
 */
@Component({
  tag: 'my-component',
  shadow: true,
})
export class MyComponent { }
```

In the code block above, `@deprecated` is added to the JSDoc for `MyComponent`.
This causes the generated README to contain:
```
> **[DEPRECATED]** since v2.0.0
```

The deprecation notice will always begin with `> **[DEPRECATED]**`, followed by the deprecation description.
In this case, that description is "since v2.0.0".

The deprecation notice will be placed after the `<!-- Auto Generated Below -->` comment in the README.
If a component is not marked as deprecated, this section will be omitted from the generated README.

### Overview Documentation

A Stencil component that has a JSDoc comment on its class component like so:

```tsx title="my-component.tsx with an overview"
/**
 * A simple component for formatting names
 *
 * This component will do some neat things!
 */
@Component({
  tag: 'my-component',
  shadow: true,
})
export class MyComponent { }
```
will generate the following section in your component's README:

```
## Overview

A simple component for formatting names

This component will do some neat things!
```

The overview will be placed after the [deprecation notice](#deprecation-notice) section of the README.
If a component's JSDoc does not contain an overview, this section will be omitted from the generated README.

### Usage Examples

You can save usage examples for a component in the `usage/` subdirectory within a component's directory.
The content of these files will be added to the `Usage` section of the generated README. 
This allows you to keep examples right  next to the code, making it easy to include them in a documentation site or other downstream consumer(s) of your docs.

:::caution
Stencil doesn't check that your usage examples are up-to-date!
If you make any changes to your component's API you'll need to remember to update your usage examples manually.
:::

If, for instance, you had a usage example like this:

````md title="src/components/my-component/usage/my-component-usage.md"
# How to Use `my-component`

This component is used to provide a way to greet a user using their first, middle, and last name.

This component will properly format the provided name, even when all fields aren't provided:

```html
<my-component first="Stencil"></my-component>
<my-component first="Stencil" last="JS"></my-component>
```
````

The following output will be added to the generated README:

````md
## Usage

### My-component-usage

# How to Use `my-component`

This component is used to provide a way to greet a user using their first, middle, and last name.

This component will properly format the provided name, even when all fields aren't provided:

```html
<my-component first="Stencil"></my-component>
<my-component first="Stencil" last="JS"></my-component>
```
````

The usage section will be placed after the [overview section](#adding-overview-documentation) of the README.
If a component's directory does not contain any usage files, this section will be omitted from the generated README.

### @Prop() Information

TODO

The properties section will be placed after the [usage examples section](#usage-examples) of the README.
If a component does not use `@Prop()`, this section will be omitted from the generated README.

### @Event() Information

TODO

The events section will be placed after the [@Prop() section](#prop-information) of the README.
If a component does not use `@Event()`, this section will be omitted from the generated README.

### @Method() Information

TODO

The methods section will be placed after the [@Event section](#event-information) of the README.
If a component does not use `@Method()`, this section will be omitted from the generated README.

### Slot Details

TODO

The methods section will be placed after the [@Event section](#event-information) of the README.
If a component does not use `@Method()`, this section will be omitted from the generated README.

### Shadow Part Details

TODO

The methods section will be placed after the [@Event section](#event-information) of the README.
If a component does not use `@Method()`, this section will be omitted from the generated README.

### Styling Details

TODO

The methods section will be placed after the [@Event section](#event-information) of the README.
If a component does not use `@Method()`, this section will be omitted from the generated README.

### A Custom Footer

Removing or customizing the footer can be done by adding a `footer` property to
the output target. This string is added to the generated Markdown files without
modification, so you can use Markdown syntax in it for rich formatting:

```tsx title="stencil.config.ts"
import { Config } from '@stencil/core';

export const config: Config = {
  outputTargets: [
    {
      type: 'docs-readme',
      footer: '*Built with love!*',
    }
  ]
};
```

The following footer will be placed at the bottom of your component's README file:
```
*Built with love!*
```

## Configuration
### Specifying the Output Directory

By default, a readme file will be generated in the same directory as the
component it corresponds to. This behavior can be changed by setting the `dir`
property on the output target configuration. Specifying a directory will create
the structure `{dir}/{component}/readme.md`.

```tsx title="stencil.config.ts"
import { Config } from '@stencil/core';

export const config: Config = {
  outputTargets: [
    {
      type: 'docs-readme',
      dir: 'output'
    }
  ]
};
```

### Strict Mode

Adding `strict: true` to the output target configuration will cause Stencil to output a warning whenever the project is built with missing documentation.

```tsx title="stencil.config.ts"
import { Config } from '@stencil/core';

export const config: Config = {
  outputTargets: [
    {
      type: 'docs-readme',
      strict: true
    }
  ]
};
```

When strict mode is enabled, the following items are checked:
1. `@Prop()` usages must be documented*
2. `@Method()` usages must be documented*
3. `@Event()` usages must be documented*
4. CSS Part usages must be documented

\* This check does not apply if the component is [marked as deprecated](#adding-a-deprecation-notice).