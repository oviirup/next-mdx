# @oviirup/next-mdx

Use [MDX](https://github.com/mdx-js/mdx) with [Next.js](https://github.com/vercel/next.js)

> [!NOTE]
> This is a slightly modified version of the original [@next/mdx](https://www.npmjs.com/package/@next/mdx) package

## Installation

For usage with the `app` directory see the section below.

```
npm install @oviirup/next-mdx
```

## Usage

Update your a `next.config.js` in your project

```js
// next.config.js
const withMDX = require('@oviirup/next-mdx')();
module.exports = withMDX();
```

Optionally you can provide [MDX plugins](https://mdxjs.com/advanced/plugins#plugins):

```js
// next.config.js
const withMDX = require('@oviirup/next-mdx')({
  options: {
    remarkPlugins: [],
    rehypePlugins: [],
  },
});
module.exports = withMDX();
```

Optionally you can add your custom Next.js configuration as parameter

```js
// next.config.js
const withMDX = require('@oviirup/next-mdx')();
module.exports = withMDX({
  // your custom next config ...
  webpack(config, options) {
    return config;
  },
});
```

By default MDX will only match and compile MDX files with the `.mdx` extension.
However, it can also be optionally configured to handle markdown files with the `.md` extension, as shown below:

```js
// next.config.js
const withMDX = require('@oviirup/next-mdx')({
  extension: /\.(md|mdx)$/,
});
module.exports = withMDX();
```

In addition, MDX can be customized with compiler options, see the [mdx documentation](https://mdxjs.com/packages/mdx/#compilefile-options) for details on supported options.

## Top level .mdx pages

Define the `pageExtensions` option to have Next.js handle `.md` and `.mdx` files in the `pages` or `app` directory as pages:

```js
// next.config.js
const withMDX = require('@oviirup/next-mdx')({
  extension: /\.mdx?$/,
});
module.exports = withMDX({
  pageExtensions: ['js', 'jsx', 'ts', 'tsx', 'md', 'mdx'],
});
```

## Custom Components

The `mdx-components` file is required specially in case if you are using app directory. By default the plugin looks for the file at `mdx-components.js` or `src/mdx-components.js`, or its typescript equivalent, but you can specify your own path.

Create an `mdx-components.js` file at the root of your project with the following contents:

```js
// This file is required to use @next/mdx in the `app` directory.
export function useMDXComponents(components) {
  return components;
  // Allows customizing built-in components, e.g. to add styling.
  // return {
  //   h1: ({ children }) => <h1 style={{ fontSize: "100px" }}>{children}</h1>,
  //   ...components,
  // }
}
```

Create a `next.config.js` in your project

```js
// next.config.js
const withMDX = require('@next/mdx')({
  // Provide path to custom mdx-components path (optional)
  components: 'src/components/mdx',
  // Optionally provide remark and rehype plugins
  options: {
    // If you use remark-gfm, you'll need to use next.config.mjs
    // as the package is ESM only
    // https://github.com/remarkjs/remark-gfm#install
    remarkPlugins: [],
    rehypePlugins: [],
  },
});

/** @type {import('next').NextConfig} */
const nextConfig = {
  // Configure pageExtensions to include md and mdx
  pageExtensions: ['ts', 'tsx', 'js', 'jsx', 'md', 'mdx'],
  // Optionally, add any other Next.js config below
  reactStrictMode: true,
};

// Merge MDX config with Next.js config
module.exports = withMDX(nextConfig);
```

## TypeScript

Follow [this guide](https://mdxjs.com/advanced/typescript) from the MDX docs.
