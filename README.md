[![npm][npm]][npm-url]
[![node][node]][node-url]
[![npm-stats][npm-stats]][npm-url]
[![deps][deps]][deps-url]

<div align="center">
  <img height="100"
    src="https://worldvectorlogo.com/logos/sass-1.svg">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Sass Remove Imports Loader</h1>
  <p>Removes all* @imports in a sass resource.</p>
</div>


This loader will remove every `@import` line in your SASS resources. 
So you can use shared SASS resources in your project when those are not "relative" imported. 
Made to work with `sass-resources-loader`!

Note, this loader is not limited to SASS resources. It supposedly works with less, post-css, etc.

### Supports **Webpack 4**.

---------------

## Installation

Get it via npm:

```bash
npm install sass-remove-imports-loader
```

Get it via yarn:

```bash
yarn add -D sass-remove-imports-loader
```

## Usage

Create your file (or files) with resources that includes some other resources:

```scss
/* resources.scss */
@import 'sections/sizes';

@mixin section-mixin {
  margin: 0 auto;
  width: $section-width;
}
```

## Tips
* Avoid using SASS `@import` rules inside resources files as it slows down incremental builds. 
  Use `sass-resources-loader` in webpack config instead. 
* If you still want to use SASS `@imports` make sure your paths are relative to the file, except the ones started with `~` (`~` is resolved to `node_modules` folder).
  By default `sass-remove-imports-loader` will not remove imports that starts with `./`, `.//` or `~` keep
  that in mind if you want to customize the exclusion

Apply loader in webpack config (`v1.x.x` & `v2.x.x` are supported) and provide path to the file with resources:

```js
/* Webpack@2: webpack.config.js */

module: {
  rules: [
    // Apply loader
    {
      test: /\.scss$/,
      use: [
        'style-loader',
        'css-loader',
        'postcss-loader',
        'sass-loader',
        'sass-resources-loader',
        {
          loader: 'sass-remove-imports-loader',
          options: {
            // Provide regular expresion for the import line to maintain
            exclude: /\.json$/,

            // Or array
            exclude: [ /^screens/, /\.json$/]
          },
        },
      ],
    },
  ],
},

/* Webpack@1: webpack.config.js */

module: {
  loaders: [
    // Apply loader
    { test: /\.scss$/, loader: 'style!css!sass!sass-resources-loader!sass-remove-imports-loader' },
  ],
},
```

### Example of Webpack 4 Config for Vue

```
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: 'vue-loader'
      },
      {
        test: /\.css$/,
        use: [
          { loader: 'vue-style-loader' },
          { loader: 'css-loader', options: { sourceMap: true } },
        ]
      },
      {
        test: /\.scss$/,
        use: [
          { loader: 'vue-style-loader' },
          { loader: 'css-loader', options: { sourceMap: true } },
          { loader: 'sass-loader', options: { sourceMap: true } },
          { loader: 'sass-resources-loader', options: { sourceMap: true } },
          { loader: 'sass-remove-imports-loader',
            options: {
              sourceMap: true,
              exclude: /\.json$/
            }
          }
        ]
      }
    ]
  }
```

## License
_sass-resources-loader_ is available under MIT.
