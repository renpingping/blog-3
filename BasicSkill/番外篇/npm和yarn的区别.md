### 包下载的前置知识

一个包的版本号基本由三位数字构成 `x.x.x`, 它们分别是 Major(主版本号), Minor(次版本号), Patch(修订号)。

* `*`: 升级 `Major` + `Minor` + `Patch`;
* `^`: 升级 `Minor` + `Patch`;
* `~`: 升级 `Patch`;

比如 `^4.3.1 表示 4.x.x` 的最新版, `~4.3.1 表示 4.3.x` 的最新版

### npm

`npm` 经历了 3 次升级:

* `npm1`: 嵌套结构

缺点: 相同的包会被重复下载多次, 比如下列 a、b、c 都依赖了 xxx 这个包, xxx 这个包被下载了 3 次。

```text
-node-modules
  - a
    - node-modules
      - xxx @1.0.0
  - b
    - node-modules
      - xxx @1.0.0
  - c
    - node-modules
      - xxx @1.0.1
```

* `npm3`: 扁平结构

所依赖的包的依赖也安装于根目录, 如果所依赖的包的依赖在根目录已存在但是版本不一致, 则在所依赖的包下面单独加上依赖的包。例子如下:

```
-node-modules
- a
- b
- xxx @1.0.0
- c
  - node-modules
    - xxx @1.0.1
```

* `npm5`: 扁平结构 + package-lock.json

加上锁文件, 确保在不同电脑上安装的包的版本号相同。

### yarn

* 更快, 离线缓存
* yarn.lock 文件

#### yarn 和 npm5 的区别

##### 同

* `npm` 通过 `package-lock.json` 推断出 `node_modules` 目录
* `yarn` 也能通过 `yarn.lock` 文件推断出 `node_modules` 顶层目录的。就是要通过稍微计算, 比如下方:

```
// yarn 的 yarn.lock 类似如下
- a
- b
- c
  - xxx@1.0.1
- xxx@1.0.1
- xxx@1.0.0
```

这个例子中, 我们能得出顶层目录如下:

```
// node_modules
- a
- b
- c
- xxx@1.0.0
```

##### 异

* 依赖于 `yarn` 的缓存机制(类似 `cnpm` 的引用机制？), `yarn` 的安装速度更快, 甚至能离线加载；

也就是说 `yarn` 结合了 `npm` 和 `cnpm` 的优点。

### 写法

```
yarn add xx -D
```

-------------- 分割线 ---------------

### yarn 使用心得

在本地开发一个独立的包, 可以通过 `yarn link` 进行链接开发。

### 区分几个 package.json 字段

* dependencies: 业务依赖
* devDependencies: 开发依赖
* pureDependencies: 在 dependencies 的基础上加上 warning
