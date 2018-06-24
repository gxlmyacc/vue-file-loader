# vue-asset-loader 

this is a `file-loader` wrapper for Vue and Webpack, solve the image relative path issues when HTML and CSS are not in the same directory.

## install

To begin, you'll need to install `vue-asset-loader`:

```console
$ npm install vue-asset-loader --save-dev
```

`vue-asset-loader` works like
[`file-loader`](https://github.com/webpack-contrib/file-loader), but solve a file-loader`s relative path issue when image, css and html in diffrence directorys.

## purpose

if your output directory structure is like this:

```
index.html
images
  1.png
  2.png
styles
  1.css
  2.css
script
  1.js
  2.js
```
1.css load 1.png, the url in `vue style` section may be write like this `url(images/1.png)`, to support that, your webpack config maybe like this:
```js
{ 
  test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/, 
  use: [{ loader: 'file-loader', options: { publicPath: '../', name: 'images/[name].[ext]?[hash]' }]
}
```

then webpack will translate `images/1.png` to `../images/1.png`, this does solve the issues, but wait, if your `vue template` section has img tag that:
```html
<img src="image/2.png">
```
webpack will translate `images/2.png` to `../images/2.png`, it leads html load `2.png` failure, so file-loader can't tell the picture from HTML or CSS.
`vue-asset-loader` just solving this issues!
your webpack config can be like this:

```js
{ 
  test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/, 
  use: [{ loader: 'vue-asset-loader', options: { publicPath: '', publicStylePath: '../', name: 'images/[name].[ext]?[hash]' }]
}
```
or 
```js
{ 
  test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/, 
  use: [{ 
    loader: 'url-loader', 
    options: { 
      limit: 2048, 
      publicPath: '', 
      publicStylePath: '../', 
      fallback: 'vue-asset-loader',
      name: 'images/[name].[ext]?[hash]' 
    }
  ]
}
```
`vue-asset-loader` will choose `publicPath` or `publicStylePath` based on the image from `vue template` or `vue style`.

## License

#### [MIT](./LICENSE)
