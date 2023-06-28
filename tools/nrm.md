## 前言 

使用这个插件主要是 方便切换各种 `npm` 镜像源

- [官网](https://github.com/Pana/nrm)

`nrm` 内置可切换 `npm` 源：

- [npm](https://registry.npmjs.org/)
- [cnpm](http://r.cnpmjs.org/)
- [taobao](https://registry.npm.taobao.org/) 
- [npmMirror](https://skimdb.npmjs.com/registry/)
- [nj](https://registry.nodejitsu.com/)
- [rednpm](http://registry.mirror.cqupt.edu.cn/)
- [edunpm](http://registry.enpmjs.org/)


## 安装

全局安装：

```javascript
npm i -g nrm
```

## 使用

#### 1. 列出可用源

```javascript
nrm ls

* npm -------- https://registry.npmjs.org/
  yarn ------- https://registry.yarnpkg.com/
  cnpm ------- http://r.cnpmjs.org/
  taobao ----- https://registry.npm.taobao.org/
  nj --------- https://registry.nodejitsu.com/
  npmMirror -- https://skimdb.npmjs.com/registry/
  edunpm ----- http://registry.enpmjs.org/
```

#### 2. 切换

```javascript
nrm use taobao

Registry has been set to: http://registry.npm.taobao.org/
```

#### 3. 增加

可以通过这命令添加私有源

```
nrm add  <registry> <url> [home]
```

#### 4. 删除

```
nrm del <registry>
```
