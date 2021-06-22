# Swarm监控大盘搭建

我们经常需要观察节点的运行状态以及一些基本的状态数据。我们可以通过登录节点调用节点API来查看，但比较繁琐。也可以直接通过官方提供的监控大盘更直观的监控


## 常用调试API


调试API接口文档地址:[https://docs.ethswarm.org/debug-api/](https://docs.ethswarm.org/debug-api/)

- 查看节点连接数

```
curl -s http://localhost:1635/peers | jq '.peers | length'
```

- 查看节点余额

```
curl localhost:1635/chequebook/balance | jq
```

## 搭建监控大盘

安装搭建环境: ubuntu18.04

### 安装Nodejs环境

- 安装`Nodejs`

```
apt install nodejs
```

- 安装`npm`

```
apt install npm
```

- 安装`Nodejs`版本管理工具`n`

```
npm install n -g
```

- 升级Nodejs到最新版本

```
n latest
```

安装完新建一个终端窗口，查看`Nodejs`版本

```
node -v
```

- 安装`yarn`

```
npm install yarn -g
```
(这一步可以省略，个人喜欢用yarn。yarn和npm一样)

### 下载监控大盘源码

使用的监控大盘: [bee-dashboard](https://github.com/ethersphere/bee-dashboard)

- 下载源码

```
git clone https://github.com/ethersphere/bee-dashboard.git
```

- 安装依赖

```
yarn install
```

- 修改`.env.development`配置文件

将以下两行的`localhost`替换成你自己服务的公网IP

```
REACT_APP_BEE_HOST=http://localhost:1633
REACT_APP_BEE_DEBUG_HOST=http://localhost:1635
```

- 启动监控大盘

```
npm start
```

运行起来以后，通过`http://IP:1633`就可以看到整个监控大盘页面了

在页面的`Settings`里，将下面的配置里的localhost换成你自己节点的公网IP即可

```
API Endpoint
http://localhost:1633
```

```
Debug API Endpoint
http://localhost:1635
```

`注意`: 这里要想能够查看到数据，需要节点的配置文件里把调试配置设为开启状态

```
debug-api-enable: true
debug-api-addr: 0.0.0.0:1635
```

如果节点是部署在远程服务器上，浏览器里直接通过配置IP的方式访问可能会出现跨域问题。在内网不会出现。如果出现，建议将监控大盘静态部署成域名访问的方式