## Welcome to the SRC folder for the Gohugo Ananke Theme.

The contents of this folder are used to generate CSS and javascript. You may never have to touch anything here, unless you want to more deeply customize your styles.
此文件夹的内容用于生成CSS和javascript。除非您想更深入地定制您的风格，否则您可能永远不必在这里触摸任何东西。
## Tools

### Yarn

We use [Yarn](https://yarnpkg.com) for package managment (instead of NPM) because it's fast and generates a lock file to make dependency management more consistent. The theme's `.gitignore` file should be kept intact to make sure that all files in the `node_modules` folder are not pushed to the repository.
我们使用Yarn进行包管理（而不是NPM），因为它速度快，并且生成一个锁定文件，使得依赖管理更加一致。主题的.gitignore文件应保持完整，以确保node_modules文件夹中的所有文件不会被推送到代码仓库。

### Webpack

We use Webpack to manage our asset pipeline. Arguably, Webpack is overkill for this use-case, but we're using it here because once it's set up (which we've done for you), it's really easy to use. If you want to use an external script, just add it via Yarn, and reference it in main.js. You'll find instructions in the js/main.js file.
Webpack：我们使用Webpack来管理资源管道。可以说，Webpack对于这种用例来说有些大材小用，但我们之所以使用它，是因为一旦设置完成（我们已经为您完成了设置），使用起来非常简单。如果您想使用外部脚本，只需通过Yarn添加并在main.js中引用它。相关说明可以在js/main.js文件中找到。

### PostCSS

PostCSS is just CSS. You'll find `postcss.config.js` in the css folder. There you'll find that we're using [`postcss-import`](https://github.com/postcss/postcss-import) which allows us import css files directly from the node_modules folder, [`postcss-cssnext`](http://cssnext.io/features/) which gives us the power to use upcoming CSS features today. If you miss Sass you can find PostCss modules for those capabilities, too.
PostCSS：PostCSS 就是CSS。您将在css文件夹中找到postcss.config.js文件。在这里，您会发现我们使用了postcss-import，它允许我们直接从node_modules文件夹中导入CSS文件，postcss-cssnext则使我们今天就可以使用即将推出的CSS功能。如果您怀念Sass，您也可以找到支持这些功能的PostCSS模块。

### Tachyons

This theme uses the [Tachyons CSS Library](http://tachyons.io/). It's about 15kb gzipped, highly modular, and each class is atomic so you never have to worry about overwriting your styles. It's a great library for themes because you can make most all the style changes you need right in your layouts.
Tachyons：此主题使用Tachyons CSS库。它经过gzip压缩后约为15kb，高度模块化，每个类都是原子的，因此您不必担心覆盖样式。这是一个非常适合主题的库，因为您可以在布局中完成大多数样式更改。

## How to Use

You'll find the commands to run in `src/package.json`.

For development, you'll need Node and Yarn installed:
您可以在src/package.json文件中找到运行的命令。
对于开发，您需要安装Node和Yarn：

```
$ cd themes/gohugo-theme-ananke/src/

$ yarn install

$ npm start

```

This will process both the postcss and scripts.

For production, instead of `npm start`, run `npm run build:production,` which will output minified versions of your files.
