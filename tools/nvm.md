# nvm管理node版本

> nvm-> Node Version Manager（Node版本管理器），用它可以方便的在机器上安装并维护多个Node的版本

- github地址`https://github.com/coreybutler/nvm-windows`

  下载后解压`nvm-setup.zip`, 安装 `nvm`

- 安装完成后 `win + r` 输入 `cmd` 打开cmd终端输入一下命令，查看`nvm`是否安装成功

  ```javascript
  nvm version
  ```

- 查看本地`Node.js`版本

  ```javascript
  nvm ls
  ```

- 查看可以下载的所有版本

  ```javascript
  nvm list available
  ```

- 安装想要的版本

  ```javascript
  nvm install 10.12.0
  ```

- 选择版本

  ```javascript
  nvm use 10.12.0
  ```

- 选择后查看`Node.js`版本

  ```javascript
  node -v
  ```

- 卸载已经安装的版本

  ```javascript
  nvm uninstall 10.12.0
  ```

- 用nvm管理node版本有一部分原因是因为npm install的时候有些依赖需要高版本的node版本

  所以，项目中建议把原来 `node_modules` 删除掉重新 `npm install` 一下