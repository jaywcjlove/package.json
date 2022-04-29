<!--idoc:ignore:start-->
PACKAGE.JSON
===
<!--idoc:ignore:end-->

文档复制 [yarnpkg](https://yarnpkg.com/zh-Hans/docs/package-json) 官方文档，并不是完全复制，增加了一些内容和一些描述，作为笔记存储。你也可以参考 [npm documentation](https://docs.npmjs.com/files/package.json), [std-pkg](https://github.com/jamiebuilds/std-pkg), [clean-publish](https://github.com/shashkovdanil/clean-publish), [package-json-validator](http://package-json-validator.com/), [cosmiconfig](https://github.com/davidtheclark/cosmiconfig), [rc](https://github.com/dominictarr/rc)。

> 🚧 注意：如果你直接克隆项目，项目名称为 `package.json`，会导致你其它 `node.js` 项目可能有问题。
<!--rehype:style=border-left: 8px solid #ffe564;background-color: #ffe56440;padding: 12px 16px;-->

<!--idoc:ignore:start-->
<!-- TOC -->

- [重要字段](#重要字段)
  - [`name`](#name)
  - [`version`](#version)
- [信息类字段](#信息类字段)
  - [`description`](#description)
  - [`keywords`](#keywords)
  - [`license`](#license)
- [链接类字段](#链接类字段)
  - [`homepage`](#homepage)
  - [`bugs`](#bugs)
  - [`repository`](#repository)
- [项目维护类字段](#项目维护类字段)
  - [`author`](#author)
  - [`contributors`](#contributors)
- [文件类信息](#文件类信息)
  - [`files`](#files)
  - [`main`](#main)
  - [`bin`](#bin)
  - [`man`](#man)
  - [`directories`](#directories)
  - [`types`](#types)
- [打包包字段](#打包包字段)
  - [`module`](#module)
  - [`browser`](#browser)
  - [`esnext`](#esnext)
- [任务类字段](#任务类字段)
  - [`scripts`](#scripts)
  - [特定的 `scripts`](#特定的-scripts)
  - [`config`](#config)
- [依赖描述类字段](#依赖描述类字段)
  - [`dependencies`](#dependencies)
  - [`devDependencies`](#devdependencies)
  - [`peerDependencies`](#peerdependencies)
  - [`peerDependenciesMeta`](#peerDependenciesMeta)
  - [`optionalDependencies`](#optionaldependencies)
  - [`bundledDependencies`](#bundleddependencies)
- [系统](#系统)
  - [`engines`](#engines)
  - [`os`](#os)
  - [`cpu`](#cpu)
- [发布](#发布)
  - [`private`](#private)
  - [`publishConfig`](#publishconfig)
- [Yarn](#yarn)
  - [`flat`](#flat)
  - [`resolutions`](#resolutions)

<!-- /TOC -->
<!--idoc:ignore:end-->

## 重要字段

你的 `package.json` 中最重要的两个字段是 `name` 和 `version`，如果没有它们，您的包将无法安装。 `name` 和`version` 字段一起用于创建唯一ID。

### `name`

```json
{
  "name": "my-awesome-package"
}
```

这是您的 `包` 的名称。 它在URL中使用，作为参数命令行，以及 `node_modules` 中的目录名。


```sh
yarn add [包名]
# or
npm install [包名]
```

```
node_modules/[包名]
```

```
https://registry.npmjs.org/[包名]/-/[包名]-[version].tgz
```

**规则**

- 必须小于或等于214个字符（包括 `@scope/` for 范围包）。
- 不能以点（`.`）或下划线（`_`）开头。
- 名称中不得包含大写字母。
- 必须仅使用URL安全字符。

**Tips**

- 不要使用和 `Node.js` 核心模块相同的名字。
- 不要在名字里包含 `js` 或者 `node` 单词。
- 短小精悍，让人看到名字就大概了解包的功能，记住它也会被用在 `require()` 调用里。
- 保证名字在 [npm registry](https://www.npmjs.com/) 里是唯一的。


### `version`

包的当前版本，严格遵循 [Semantic Versioning 2.0.0](http://semver.org/lang/zh-CN/) 语义化版本规范。

```json
{
  "version": "1.0.0"
}
```

## 信息类字段

### `description`

Description 是帮助使用者了解包的功能的字符串，包管理器也会把这个字符串作为搜索关键词。

```json
{
  "description": "我的包的概要简短描述"
}
```

### `keywords`

关键字是一个字符串数组，当在包管理器里搜索包时很有用。

```json
{
  "keywords": ["short", "relevant", "keywords", "for", "searching"]
}
```

### `license`

所有包都应该指定许可证，以便让用户了解他们是在什么授权下使用此包，以及此包还有哪些附加限制。

```json
{
  "license": "MIT",
  "license": "(MIT or GPL-3.0)",
  "license": "SEE LICENSE IN LICENSE_FILENAME.txt",
  "license": "UNLICENSED"
}
```

鼓励使用开源 [(OSI-approved)](https://opensource.org/licenses/alphabetical) 许可证，除非你有特别的原因不用它。 如果你开发的包是你工作的一部分，最好和公司讨论后再做决定。

**license字段必须是以下之一:**

- 如果你使用标准的许可证，需要一个有效地 [SPDX 许可证标识](https://spdx.org/licenses/)。
- 如果你用多种标准许可证，需要有效的 [SPDX 许可证表达式2.0语法表达式](https://www.npmjs.com/package/spdx)。
- 如果你使用非标准的许可证，一个 `SEE LICENSE IN <文件名>` 字符串指向你的包里顶级目录的一个 <文件名>。
- 如果你不想在任何条款下授权其他人使用你的私有或未公开的包，一个 `UNLICENSED` 字符串。

## 链接类字段

各种指向项目文档、issues 上报，以及代码托管网站的链接字段。

### `homepage`

是包的项目主页或者文档首页。

```json
{
  "homepage": "https://your-package.org"
}
```

### `bugs`

问题反馈系统的 URL，或者是 email 地址之类的链接。用户通过该途径向你反馈问题。

```json
{
  "bugs": "https://github.com/user/repo/issues"
}
```

### `repository`

是代码托管的位置。

```json
{
  "repository": { "type": "git", "url": "https://github.com/user/repo.git" },
  "repository": "github:user/repo",
  "repository": "gitlab:user/repo",
  "repository": "bitbucket:user/repo",
  "repository": "gist:a1b2c3d4e5f"
}
```

The repository is the location where the actual code for your package lives.

## 项目维护类字段

项目的维护者。

### `author`

作者信息，一个人。

```json
{
  "author": { "name": "Your Name", "email": "you@example.com", "url": "http://your-website.com" },
  "author": "Your Name <you@example.com> (http://your-website.com)"
}
```

### `contributors`

贡献者信息，可能很多人。

```json
{
  "contributors": [
    { "name": "Your Friend", "email": "friend@example.com", "url": "http://friends-website.com" }
    { "name": "Other Friend", "email": "other@example.com", "url": "http://other-website.com" }
  ],
  "contributors": [
    "Your Friend <friend@example.com> (http://friends-website.com)",
    "Other Friend <other@example.com> (http://other-website.com)"
  ]
}
```

## 文件类信息

指定包含在项目中的文件，以及项目的入口文件。

### `files`

```json
{
  "files": [
    "filename.js",
    "directory/",
    "glob/*.{js,json}"
  ]
}
```

项目包含的文件，可以是单独的文件、整个文件夹，或者通配符匹配到的文件。

### `main`

项目的入口文件。

```json
{
  "main": "filename.js"
}
```

### `bin`

随着项目一起被安装的可执行文件。

```json
{
  "bin": "bin.js",
  "bin": {
    "命令名称": "bin/命令路径/命令名称.js",
    "other-command": "bin/other-command"
  }
}
```

### `man`

项目的入口文件。

```json
{
  "man": "./man/doc.1",
  "man": ["./man/doc.1", "./man/doc.2"]
}
```

### `directories`

```json
{
  "directories": {
    "lib": "path/to/lib/",
    "bin": "path/to/bin/",
    "man": "path/to/man/",
    "doc": "path/to/doc/",
    "example": "path/to/example/"
  }
}
```

当你的包安装时，你可以指定确切的位置来放二进制文件、man pages、文档、例子等。

### `types`

这是一个只在 [TypeScript](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html) 中生效的字段，如果您的包有一个 `main.js` 文件，您还需要在  `package.json` 文件中指明主声明文件。 将 `types` 属性设置为指向 bundled 的声明文件。 例如：

```json
{
  "types": "./lib/main.d.ts",
}
```

如果您的主声明文件名为 `index.d.ts` 并且位于包的根目录（`index.js`旁边），则不需要标记 `types` 属性，建议这样做。

## 打包包字段

### `module`

`pkg.module` 将指向具有 `ES2015` 模块语法的模块，但仅指向目标环境支持的语法功能。 完整的描述[在这里](https://github.com/rollup/rollup/wiki/pkg.module)。

支持：[rollup](https://github.com/rollup/rollup-plugin-node-resolve), [webpack](https://webpack.js.org/configuration/resolve/#resolve-mainfields)

### `browser`

字段由模块作者提供，作为 `JavaScript` 包或组件工具的提示，用于打包模块以供客户端使用。 提案就[在这里](https://github.com/defunctzombie/package-browser-field-spec)。

### `esnext`

完整的[提案在这里](http://2ality.com/2017/04/transpiling-dependencies-babel.html)。 简短说明：

- `esnext`：ES模块中使用阶段4功能（或更旧版本）的源代码，未编译。
- `main`：指向一个CommonJS模块（或UMD模块），其 `JavaScript` 与 `Node.js` 当前可以处理的一样现代。
- 大多数 `module` 用例应该可以通过 `esnext` 处理。
- `browser` 可以通过 `esnext` 的扩展版本来处理

```json
{
  "main": "main.js",
  "esnext": {
    "main": "main-esnext.js",
    "browser": "browser-specific-main-esnext.js"
  }
}
```

另请参阅：[Delivering untranspiled source code via npm](http://2ality.com/2017/06/pkg-esnext.html)

## 任务类字段

包里还可以包含一些可执行脚本或者其他配置信息。

### `scripts`

```json
{
  "scripts": {
    "build-project": "node build-project.js"
  }
}
```

脚本是定义自动化开发相关任务的好方法，比如使用一些简单的构建过程或开发工具。 在 `scripts` 字段里定义的脚本，可以通过 `yarn run <script>` 命令来执行。 例如，上述 `build-project` 脚本可以通过 `yarn run build-project` 调用，并执行 `node build-project.js`。

有一些特殊的脚本名称。 如果定义了 `preinstall` 脚本，它会在包安装前被调用。 出于兼容性考虑，`install`、`postinstall` 和 `prepublish` 脚本会在包完成安装后被调用。

`start` 脚本的默认值为 `node server.js`。

参考文档：[npm docs](https://docs.npmjs.com/files/package.json#default-values)

### 特定的 `scripts`

对于以下脚本，`npm` 支持 `package.json` 文件的 `scripts` 默认命令字段：

- `prepublish`: 在打包并发布包之前运行，以及在没有任何参数的本地 `npm` 安装之前运行。 （见下文）
- `prepare`: 在打包和发布包之前运行，在没有任何参数的本地 `npm install` 上运行，以及安装 git 依赖项时（见下文）。 这是在 `preublish` 之后运行，但是在 `preublishOnly` 之前运行。
- `prepublishOnly`: 在包准备和打包之前运行，仅限于npm发布。 （见下文。）
- `prepack`: 在打包 `tarball` 之前运行（在 `npm pack`，`npm publish`，以及安装 git 依赖项时）
- `postpack`: 在生成 `tarball` 之后运行并移动到其最终目标。
- `publish`, `postpublish`: 在包发布后运行。
- `preinstall`: 在安装软件包之前运行。
- `install`, `postinstall`: 安装包后运行。
- `preuninstall`, `uninstall`: 在卸载软件包之前运行。
- `postuninstall`: 在卸载软件包之后运行。
- `preversion`: 在改变包版本之前运行。
- `version`: 改变包版本后运行，但提交之前。
- `postversion`: 改变包版本后运行，然后提交。
- `pretest`, `test`, `posttest`: 由 `npm test` 命令运行。
- `prestop`, `stop`, `poststop`: 由 `npm stop` 命令运行。
- `prestart`, `start`, `poststart`: 由 `npm start` 命令运行。 
- `prerestart`, `restart`, `postrestart`: 由 `npm restart` 命令运行。 注意：如果没有提供重启脚本，`npm restart` 将运行 `stop` 和`start` 脚本。
- `preshrinkwrap`, `shrinkwrap`, `postshrinkwrap`: 由 `npm shrinkwrap` 命令运行。

参考文档：[npm docs](https://docs.npmjs.com/misc/scripts).

### `config`

```json
{
  "config": {
    "port": "8080"
  }
}
```

配置你的脚本的选项或参数。

## 依赖描述类字段

你的包很可能依赖其他包。你可以在你的 `package.json` 文件里指定那些依赖。

### `dependencies`

这些是你的包的开发版和发布版都需要的依赖。

```json
{
  "dependencies": {
    "package-1": "^3.1.4",
    "package-2": "file:./path/to/dir"
  }
}
```

> 你可以指定一个确切的版本、一个最小的版本 (比如 `>=`) 或者一个版本范围 (比如 `>= ... <`)。  
> 包也可以指向本地的一个目录文件夹。  

### `devDependencies`

这些是只在你的包开发期间需要，但是生产环境不会被安装的包。

```json
{
  "devDependencies": {
    "package-2": "^0.4.2"
  }
}
```

### `peerDependencies`

平行依赖允许你说明你的包和其他包版本的兼容性。

```json
{
  "peerDependencies": {
    "package-3": "^2.7.18"
  }
}
```

### `peerDependenciesMeta`

添加可选设置以消除丢失的对等依赖性警告，[#6671](https://github.com/yarnpkg/yarn/pull/6671) 。

```json
{
  "peerDependenciesMeta": {
    "node-sass": {
      "optional": true
    },
    "sass": {
      "optional": true
    },
    "fibers": {
      "optional": true
    }
  }
}
```

### `optionalDependencies`

可选依赖可以用于你的包，但不是必需的。如果可选包没有找到，安装还可以继续。

```json
{
  "optionalDependencies": {
    "package-5": "^1.6.1"
  }
}
```

### `bundledDependencies`

打包依赖是发布你的包时将会一起打包的一个包名数组。

```json
{
  "bundledDependencies": [
    "package-4"
  ]
}
```

## 系统

你可以提供和你的包关联的系统级的信息，比如操作系统兼容性之类。

### `engines`

指定使用你的包客户必须使用的版本，这将检查 `process.versions` 以及当前 `yarn` 版本。

```json
{
  "engines": {
    "node": "^10.13.0 || ^12.13.0 || ^14.15.0 || >=15.0.0",
    "node": ">=4.4.7 <7.0.0",
    "zlib": "^1.2.8",
    "yarn": "^0.14.0"
  }
}
```

此检查遵守正常的 [semver](http://semver.org/lang/zh-CN/) 规则，但有一个例外。 它允许预发布版本匹配未明确指定预发布的 [semver](http://semver.org/lang/zh-CN/)。 例如，`1.4.0-rc.0` 匹配 `>=1.3.0`，但它与典型的 `semver` 检查不匹配。

### `os`

此选项指定你的包的操作系统兼容性，它会检查 `process.platform`。

```json
{
  "os": ["darwin", "linux"],
  "os": ["!win32"]
}
```


### `cpu`

使用这个选项指定你的包将只能在某些 CPU 体系架构上运行，这会检查 `process.arch`。

```json
{
  "cpu": ["x64", "ia32"],
  "cpu": ["!arm", "!mips"]
}
```

## 发布

### `private`

```json
{
  "private": true
}
```

如果你不想你的包发布到包管理器(npm 或者 私有包管理)，设置为 `true`。

### `publishConfig`

这些配置值将在你的包发布时使用。比如，你可以给包打标签。

```json
{
  "publishConfig": {
    "registry": "https://registry.npm.taobao.org"
  }
}
```

这是一组将在发布时使用的配置值。 如果要设置标记，注册表或访问权限，则特别方便，以便确保给定的包未标记为 `latest`，发布到全局公共 `registry` 或默认情况下，作用域模块(@scoped)是私有的。

可以覆盖任何配置值，但只有 `tag`，`registry` 和 `access` 可能对于发布而言很重要，[npm-config](https://docs.npmjs.com/misc/config#config-settings)。

## Yarn

### `flat`

如果你的包只允许给定依赖的一个版本，你想强制和命令行上 [yarn install --flat](#) 相同的行为，把这个值设为 `true`。

```json
{
  "flat": true
}
```

请注意，如果你的 `package.json` 包含 `"flat": true` 并且其它包依赖你的包 (比如你在构建一个库，而不是应用)， 其它那些包也需要在它们的 `package.json` 加上 `"flat": true`，或者在命令行上用 `yarn install --flat` 安装。

### `resolutions`

```json
{
  "resolutions": {
    "transitive-package-1": "0.0.29",
    "transitive-package-2": "file:./local-forks/transitive-package-2",
    "dependencies-package-1/transitive-package-3": "^2.1.1"
  }
}
```

允许您覆盖特定嵌套依赖项的版本。 有关完整规范，请参见[选择性版本解析 RFC](https://github.com/yarnpkg/rfcs/blob/master/implemented/0000-selective-versions-resolutions.md)。

注意，`yarn install --flat` 命令将会自动在 `package.json` 文件里加入 `resolutions` 字段。

## Contributors

As always, thanks to our amazing contributors!

<a href="https://github.com/jaywcjlove/package.json/graphs/contributors">
  <img src="https://jaywcjlove.github.io/package.json/CONTRIBUTORS.svg" />
</a>

Made with [action-contributors](https://github.com/jaywcjlove/github-action-contributors).

## License

Licensed under the MIT License.

<!--idoc:config:
footer: |
  Released under the MIT License. Copyright © 2022 Kenny Wong<br />
  Generated by <a href="https://github.com/jaywcjlove/idoc" target="_blank">idoc</a> v{{idocVersion}}
-->
