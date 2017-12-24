# Ant Design of React

Following the Ant Design specification, we developed a React UI library`antd`that contains a set of high quality components and demos for building rich, interactive user interfaces.

![](https://gw.alipayobjects.com/zos/rmsportal/KDpgvguMpGfqaHPjicRK.svg)

+

![](https://t.alipayobjects.com/images/rmsweb/T16xRhXkxbXXXXXXXX.svg)

* [Features](https://ant.design/docs/react/introduce#Features)
* [Environment Support](https://ant.design/docs/react/introduce#Environment-Support)
* [Version](https://ant.design/docs/react/introduce#Version)
* [Installation](https://ant.design/docs/react/introduce#Installation)
* [Usage](https://ant.design/docs/react/introduce#Usage)
* [Links](https://ant.design/docs/react/introduce#Links)
* [Companies using antd](https://ant.design/docs/react/introduce#Companies-using-antd)
* [Contributing](https://ant.design/docs/react/introduce#Contributing)
* [Need Help?](https://ant.design/docs/react/introduce#Need-Help?)

## Features[\#](https://ant.design/docs/react/introduce#Features) {#Features}

* An enterprise-class UI design language for web applications.

* A set of high-quality React components out of the box.

* Written in TypeScript with complete defined types.

* A npm + webpack +[dva](https://github.com/dvajs/dva)front-end development workflow.

## Environment Support[\#](https://ant.design/docs/react/introduce#Environment-Support) {#Environment-Support}

* Modern browsers and Internet Explorer 9+ \(with[polyfills](https://ant.design/docs/react/getting-started#Compatibility)\)

* Server-side Rendering

* [Electron](http://electron.atom.io/)

## Version[\#](https://ant.design/docs/react/introduce#Version) {#Version}

* Stable:[![](https://img.shields.io/npm/v/antd.svg?style=flat-square "npm package")](https://www.npmjs.org/package/antd)

* Next：[![](https://img.shields.io/npm/v/antd/next.svg?style=flat-square "npm \(next\)")](https://www.npmjs.org/package/antd)

You can subscribe to this feed for new version notifications:[https://github.com/ant-design/ant-design/releases.atom](https://github.com/ant-design/ant-design/releases.atom)

## Installation[\#](https://ant.design/docs/react/introduce#Installation) {#Installation}

### Using npm or yarn[\#](https://ant.design/docs/react/introduce#Using-npm-or-yarn) {#Using-npm-or-yarn}

**We recommend using npm or yarn to install**，it not only makes development easier，but also allow you to take advantage of the rich ecosystem of Javascript packages and tooling.

```
$ 
npm
install
 antd --save
```

```
$ yarn add antd
```

If you are in a bad network environment，you can try other registries and tools like[cnpm](https://github.com/cnpm/cnpm).

### Import in Browser[\#](https://ant.design/docs/react/introduce#Import-in-Browser) {#Import-in-Browser}

Add`script`and`link`tags in your browser and use the global variable`antd`.

We provide`antd.jsantd.css`and`antd.min.jsantd.min.css`under`antd/dist`in antd's npm package. You can also download these files directly from[![](https://img.shields.io/cdnjs/v/antd.svg?style=flat-square "CDNJS")](https://cdnjs.com/libraries/antd)or[unpkg](https://unpkg.com/).

> **We strongly discourage loading the entire files**this will add bloat to your application and make it more difficult to receive bugfixes and updates. Antd is intended to be used in conjunction with a build tool, such as[webpack](https://webpack.github.io/), which will make it easy to import only the parts of antd that you are using.

## Usage[\#](https://ant.design/docs/react/introduce#Usage) {#Usage}

```
import
{
 DatePicker 
}
from 'antd';

ReactDOM.render(<DatePicker/>,
 mountNode
);
```

And import stylesheets manually:

```
import 'antd/dist/antd.css';
// or 'antd/dist/antd.less'
```

### Use modularized antd[\#](https://ant.design/docs/react/introduce#Use-modularized-antd) {#Use-modularized-antd}

* Use[babel-plugin-import](https://github.com/ant-design/babel-plugin-import)\(Recommended\)

      // .babelrc or babel-loader option
      {
      "plugins"
      :
      [
      [
      "import"
      ,
      {
      "libraryName"
      :
      "antd"
      ,
      "libraryDirectory"
      :
      "es"
      ,
      "style"
      :
      "css"
      }
      ]
      // `style: true` for less
      ]
      }

  > Note: Don't set`libraryDirectory`if you are using webpack 1.

  This allows you to import components from antd without having to manually import the corresponding stylesheet. The antd babel plugin will automatically import stylesheets.

  ```
  // import js and css modularly, parsed by babel-plugin-import
  import
  {
   DatePicker 
  }
  from
  'antd'
  ;
  ```

* Manually import

  ```
  import
   DatePicker 
  from
  'antd/lib/date-picker'
  ;
  // for js
  import
  'antd/lib/date-picker/style/css'
  ;
  // for css
  // import 'antd/lib/date-picker/style';         // that will import less
  ```

### TypeScript[\#](https://ant.design/docs/react/introduce#TypeScript) {#TypeScript}

* Don't use @types/antd, as antd provides a built-in ts definition already.



