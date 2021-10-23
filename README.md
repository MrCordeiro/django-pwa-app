# Django PWA App

## Running the webserver

1. Start webpack on watch mode

```shell
cd frontend
npm run watch
```

2. Start the Django webserver

```shell
python manage.py runserver
```

## Generate Web app manifest

The web app manifest provides information about a web application in a JSON text file, necessary for the web app to be downloaded and be presented to the user similarly to a native app

Source: https://www.accordbox.com/blog/add-web-app-manifest-to-django/

Let's first install some packages we need

```shell
$ cd frontend
$ npm install -D html-webpack-plugin favicons-webpack-plugin favicons
```

The `html-webpack-plugin` simplifies creation of HTML files to serve your webpack bundles.

`favicons-webpack-plugin` will leverage on favicons to automatically generate your favicons and Web app manifest.
Create `frontend/src/index.html`, leave it as empty file.

Put the original logo of your project to `frontend/vendors/images/favicon.svg`, PNG and JPG also supported.

Edit `frontend/webpack/webpack.common.js`:

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const FaviconsWebpackPlugin = require('favicons-webpack-plugin');


plugins: [
  ... 

  new FaviconsWebpackPlugin({
    // Your source logo (required)
    logo: './vendors/images/favicon.svg',
    // Prefix path for generated assets
    prefix: 'assets/',
    devMode: 'webapp', // optional can be 'webapp' or 'light' - 'light' by default 
    // Favicons configuration options. Read more on: https://github.com/evilebottnawi/favicons#usage
    favicons: {
      appName: 'AccordBox',              // Your application's name. `string`
      icons: {
        favicons: true,             // Create regular favicons. `boolean`
        android: true,              // Create Android homescreen icon. `boolean` or `{ offset, background }`
        appleIcon: true,            // Create Apple touch icons. `boolean` or `{ offset, background }`
        appleStartup: false,         // Create Apple startup images. `boolean` or `{ offset, background }`
        coast: false,                // Create Opera Coast icon. `boolean` or `{ offset, background }`
        firefox: false,              // Create Firefox OS icons. `boolean` or `{ offset, background }`
        windows: false,              // Create Windows 8 tile icons. `boolean` or `{ background }`
        yandex: false                // Create Yandex browser icon. `boolean` or `{ background }`
      }
    },
  }),
  new HtmlWebpackPlugin()
]
```

We import `HtmlWebpackPlugin` and `FaviconsWebpackPlugin` at the top

And then we add two plugins, one is `FaviconsWebpackPlugin` and the other is `HtmlWebpackPlugin`

As you can see, we add some config to the `FaviconsWebpackPlugin` to define how favicon is generated.

Files will be generated on `frontend/build`.

The Webpack plugins are very powerful, they generate the app icons, web app manifest and the relevant HTML code.

We can copy the HTML in `frontend/build/index.html` to our Django template to make them work.

## Add Service Worker to Django

Source: https://www.accordbox.com/blog/add-service-worker-to-django/