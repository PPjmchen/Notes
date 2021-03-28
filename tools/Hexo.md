# Hexo

## 安装

### 安装Node.js

Node.js 为大多数平台提供了官方的 [安装程序](https://nodejs.org/en/download/)。对于中国大陆地区用户，可以前往 [淘宝 Node.js 镜像](https://npm.taobao.org/mirrors/node) 下载。

其它的安装方法：

- Windows：通过 [nvs](https://github.com/jasongin/nvs/)（推荐）或者[nvm](https://github.com/nvm-sh/nvm) 安装。
- Mac：使用 [Homebrew](https://brew.sh/) 或 [MacPorts](http://www.macports.org/) 安装。
- Linux（DEB/RPM-based）：从 [NodeSource](https://github.com/nodesource/distributions) 安装。
- 其它：使用相应的软件包管理器进行安装，可以参考由 Node.js 提供的 [指导](https://nodejs.org/en/download/package-manager/)

### 安装 Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```
$ npm install -g hexo-cli
```

### 进阶安装和使用

对于熟悉 npm 的进阶用户，可以仅局部安装 `hexo` 包。

```
$ npm install hexo
```

安装以后，可以使用以下两种方式执行 Hexo：

1. `npx hexo <command>`

2. 将 Hexo 所在的目录下的 `node_modules` 添加到环境变量之中即可直接使用 `hexo <command>`：

   ```
   echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.profile
   ```

## 建站

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```