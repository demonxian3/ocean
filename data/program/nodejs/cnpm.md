# Cnpm 

[淘宝npm](https://npm.taobao.org/)

## windows环境安装Cnpm

- 配置环境变量

``` sh
npm config set prefix "D:\Program Files\nodejs\node_global"
npm config set cache "D:\Program Files\nodejs\node_cache"
```

- 查看配置

``` sh
npm config list
```
- 安装cnpm

``` sh
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

- 添加环境变量

> 在 %path% 变量中新加cnpm路径即可


- 查看版本

``` sh
cnpm -v
```

## 修改镜像地址

若不想用`cnpm`也可以直接修改源地址

- 临时配置阿里镜像

``` sh
npm --registry https://registry.npm.taobao.org install express
```

- 持久配置阿里镜像

``` sh
npm config set registry https://registry.npm.taobao.org
```
