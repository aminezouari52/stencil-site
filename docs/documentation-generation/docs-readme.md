---
title: Docs Readme Auto-Generation
sidebar_label: README Docs (docs-readme)
description: README Docs
slug: /docs-readme
---

# Docs Readme Markdown File Auto-Generation

Stencil is able to auto-generate `readme.md` files in markdown. This
feature will save the generated readme files as a sibling to the component's source file within the
same directory. This can help you to maintain consistently formatted
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

Another option would be to use the `--docs` CLI flag as a part of the [build task](../config/cli.md#stencil-build), like so:

```bash
stencil build --docs
```

This will cause the Stencil compiler to perform a one-time generation of README
files as a part of the build process.

:::note
If you don't add the `docs-readme` output target to your Stencil configuration, the `--docs` flag must be applied to regenerate documentation.
:::

Alternatively, the [docs task](../config/cli.md#stencil-docs) can be used to perform a one time generation of the documentation:
```bash
stencil docs
```

## Adding Custom Markdown Content

Once you've generated a `readme.md` file, you can add your own
markdown content. Simply add your own markdown above the comment that reads:
`<!-- Auto Generated Below -->`.

The content that you generate before this comment will be persisted on subsequent builds of the README file.

## Adding a Deprecation Notice

A Stencil component that uses the [JSDoc `@deprecated` tag](https://jsdoc.app/tags-deprecated) in the component class' JSDoc like so:
```tsx title="my-component.tsx with @deprecated"
/**
 * A simple component for formatting names.
 * 
 * @deprecated Please use `my-new-component` going forward
 */
@Component({
  tag: 'my-component',
  shadow: true,
})
export class MyComponent { }
```
will have a deprecation notice added to the generated documentation.
The deprecation notice will always begin with `> **[DEPRECATED]**`, followed by the deprecation description:
```
> **[DEPRECATED]** Please use `my-new-component` going forward
```

The deprecation notice will be placed after the `<!-- Auto Generated Below -->` comment in the README.
If a component is not marked as deprecated, this section will be omitted from the generated README.

## Adding Overview Documentation

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

## Adding Usage Examples

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

## Adding Property Information

TODO

The properties section will be placed after the [usage examples section](#adding-usage-examples) of the README.
If a component does not use `@Prop()`, this section will be omitted from the generated README.

## Adding a Custom Footer

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

## Generating to a Directory

By default, a readme file will be generated in the same directory as the
component corresponds to. This behavior can be changed by setting the `dir`
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

## Strict Mode

Adding `strict: true` to the output target configuration will cause Stencil to
output a warning whenever the project is built with missing documentation.

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
