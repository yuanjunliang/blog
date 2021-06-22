# 搭建Swarm单节点
[TOC]

本文档搭建环境: ubuntu 18.04系统

## 安装Bee Clef

```
wget https://github.com/ethersphere/bee-clef/releases/download/v0.4.13/bee-clef_0.4.13_amd64.deb
sudo dpkg -i bee-clef_0.4.13_amd64.deb
```

## 检查Bee-clef是否安装成功

`Bee-clef`安装完会立即启动，通过一下命令检查`Bee-clef`是否正常启动

```
systemctl status bee-clef
```

如果返回一下信息，说明`bee-clef`安装并启动成功

```
● bee-clef.service - Bee Clef
   Loaded: loaded (/lib/systemd/system/bee-clef.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2021-06-22 09:28:49 UTC; 2min 13s ago
     Docs: https://docs.ethswarm.org
 Main PID: 3525 (bash)
    Tasks: 10 (limit: 4657)
   CGroup: /system.slice/bee-clef.service
           ├─3525 bash /usr/bin/bee-clef-service start
           ├─3545 clef --stdio-ui --keystore /var/lib/bee-clef/keystore --configdir /var/lib/bee-clef --chainid 5 --rules /etc/bee-c
           └─3546 tee /var/lib/bee-clef/stdout
```

查看`bee-clef`服务日志

```
sudo journalctl -f -u bee-clef.service
```
`注`: ubuntu18.04版本会出现以下报错
如果报错`No journal files were found.`

解决办法: 安装[journalctl](https://command-not-found.com/journalctl)
```
apt-get install systemd
```

## 安装`Bee`

```
wget https://github.com/ethersphere/bee/releases/download/v1.0.0/bee_1.0.0_amd64.deb
sudo dpkg -i bee_1.0.0_amd64.deb
```

## 配置`Bee`

```
sudo vi /etc/bee/bee.yaml
```

首先配置一下参数

```
full-node: true
nat-addr: "IP:1634"  // 这里IP换成服务器的公网IP
swap-endpoint: https://goerli.infura.io/v3/<<your-api-key>> // 这里替换成自己的infura节点
debug-api-enable: true  // 打开调试模式
debug-api-addr: 127.0.0.1:1635  // 调试API接口地址
```

- 重启`Bee`
```
sudo systemctl restart bee
```
- 查看节点日志

```
sudo journalctl --lines=100 --follow --unit bee
```

会返回一下内容

```
Jun 22 13:29:18 vultr.guest systemd[1]: Started Bee - Ethereum Swarm node.
Jun 22 13:29:18 vultr.guest bee[18569]: Welcome to Swarm.... Bzzz Bzzzz Bzzzz
Jun 22 13:29:18 vultr.guest bee[18569]:                 \     /
Jun 22 13:29:18 vultr.guest bee[18569]:             \    o ^ o    /
Jun 22 13:29:18 vultr.guest bee[18569]:               \ (     ) /
Jun 22 13:29:18 vultr.guest bee[18569]:    ____________(%%%%%%%)____________
Jun 22 13:29:18 vultr.guest bee[18569]:   (     /   /  )%%%%%%%(  \   \     )
Jun 22 13:29:18 vultr.guest bee[18569]:   (___/___/__/           \__\___\___)
Jun 22 13:29:18 vultr.guest bee[18569]:      (     /  /(%%%%%%%)\  \     )
Jun 22 13:29:18 vultr.guest bee[18569]:       (__/___/ (%%%%%%%) \___\__)
Jun 22 13:29:18 vultr.guest bee[18569]:               /(       )\
Jun 22 13:29:18 vultr.guest bee[18569]:             /   (%%%%%)   \
Jun 22 13:29:18 vultr.guest bee[18569]:                  (%%%)
Jun 22 13:29:18 vultr.guest bee[18569]:                    !
Jun 22 13:29:18 vultr.guest bee[18569]: DISCLAIMER:
Jun 22 13:29:18 vultr.guest bee[18569]: This software is provided to you "as is", use at your own risk and without warranties of any kind.
Jun 22 13:29:18 vultr.guest bee[18569]: It is your responsibility to read and understand how Swarm works and the implications of running this software.
Jun 22 13:29:18 vultr.guest bee[18569]: The usage of Bee involves various risks, including, but not limited to:
Jun 22 13:29:18 vultr.guest bee[18569]: damage to hardware or loss of funds associated with the Ethereum account connected to your node.
Jun 22 13:29:18 vultr.guest bee[18569]: No developers or entity involved will be liable for any claims and damages associated with your use,
Jun 22 13:29:18 vultr.guest bee[18569]: inability to use, or your interaction with other nodes or the software.time="2021-06-22T13:29:18Z" level=warning msg="your node is outdated, please check for the latest version"
Jun 22 13:29:18 vultr.guest bee[18569]: version: 1.0.0-2572fa48 - planned to be supported until 1 April 1970, please follow http://ethswarm.org/
Jun 22 13:29:18 vultr.guest bee[18569]: time="2021-06-22T13:29:18Z" level=info msg="swarm public key 024565209894fa36f2a83cafee42f951ec51d16e60d62cce21dac48a80af841a93"
Jun 22 13:29:18 vultr.guest bee[18569]: time="2021-06-22T13:29:18Z" level=info msg="pss public key 030c15d7be3df18193c33c2304eae43a2f75b3d072c39e1f4f567e205aad690c29"
Jun 22 13:29:18 vultr.guest bee[18569]: time="2021-06-22T13:29:18Z" level=info msg="using ethereum address 0e08438ab1d8c432097eb897187e5adad5cb25b3"
Jun 22 13:29:18 vultr.guest bee[18569]: time="2021-06-22T13:29:18Z" level=info msg="version: 1.0.0-2572fa48"
Jun 22 13:29:18 vultr.guest bee[18569]: time="2021-06-22T13:29:18Z" level=info msg="debug api address: 127.0.0.1:1635"
Jun 22 13:29:20 vultr.guest bee[18569]: time="2021-06-22T13:29:20Z" level=info msg="using default factory address for chain id 5: 73c412512e1ca0be3b89b77ab3466da6a1b9d273"
Jun 22 13:29:21 vultr.guest bee[18569]: time="2021-06-22T13:29:21Z" level=info msg="no chequebook found, deploying new one."
Jun 22 13:29:21 vultr.guest bee[18569]: time="2021-06-22T13:29:21Z" level=warning msg="cannot continue until there is sufficient ETH (for Gas) and at least 1 BZZ available on 0e08438ab1d8c432097eb897187e5adad5cb25b3"
Jun 22 13:29:21 vultr.guest bee[18569]: time="2021-06-22T13:29:21Z" level=warning msg="learn how to fund your node by visiting our docs at https://docs.ethswarm.org/docs/installation/fund-your-node"
```

其中有一句信息`cannot continue until there is sufficient ETH (for Gas) and at least 1 BZZ available on 0e08438ab1d8c432097eb897187e5adad5cb25b3`

我们需要给节点地址`0e08438ab1d8c432097eb897187e5adad5cb25b3`至少转账1BZZ和足够支付手续费的ETH，节点才能正常启动

## 水龙头获取`BZZ`和`ETH`

- `BZZ`的获取: 通过官方提供的[水龙头](https://docs.ethswarm.org/docs/installation/fund-your-node)
- `ETH`的获取: 推荐[https://faucet.goerli.mudit.blog/](https://faucet.goerli.mudit.blog/),一次可以获取32个ETH

`如果测试网繁忙，BZZ可能需要很久，甚至领不到`

领导测试的`ETH`和`BZZ`以后，直接转账1BZZ和1ETH给节点账号: `0e08438ab1d8c432097eb897187e5adad5cb25b3`（这里要查看你自己的节点账号是什么，ETH不要求必须是1个，只要足够手续费即可，BZZ需要至少1个）

当节点接受到`ETH`和`BZZ`以后会自动启动节点，这个过程可能需要一段时间

## 检查节点启动状态

查看节点日志

```
sudo journalctl --lines=100 --follow --unit bee
```

如果上面的`ETH`和`BZZ`到账以后，通过查看日志可以看到以下启动过程

```
time="2021-06-22T13:47:01Z" level=info msg="deploying new chequebook in transaction afaa730a53d43dd01bd6dbe4e322bb4da3a33fee81f20492c6718523fdf981d1"
Jun 22 13:47:33 vultr.guest bee[19025]: time="2021-06-22T13:47:33Z" level=info msg="deployed chequebook at address 17d925a5ece2a53d0f19b9d2204754ba8d795992"
Jun 22 13:47:33 vultr.guest bee[19025]: time="2021-06-22T13:47:33Z" level=info msg="depositing 10000000000000000 token into new chequebook"
Jun 22 13:47:34 vultr.guest bee[19025]: time="2021-06-22T13:47:34Z" level=info msg="sent deposit transaction 4267a77a0db10f61087280c29a280b2c80de98f3d646c3d827db3cc41e4b7f59"
Jun 22 13:47:51 vultr.guest bee[19025]: time="2021-06-22T13:47:51Z" level=info msg="successfully deposited to chequebook"
Jun 22 13:47:51 vultr.guest bee[19025]: time="2021-06-22T13:47:51Z" level=info msg="using the chequebook transaction hash afaa730a53d43dd01bd6dbe4e322bb4da3a33fee81f20492c6718523fdf981d1"
Jun 22 13:47:52 vultr.guest bee[19025]: time="2021-06-22T13:47:52Z" level=info msg="using the next block hash from the blockchain 6b44106ea32f1b4a42bf6ba5c88d5060afc96be47be0ec34c27ca59f946a8e12"
```

- 检查节点状态

```
curl localhost:1633
```
返回以下信息，说明节点运行正常
```
Ethereum Swarm Bee
```

## 节点常用操作命令

```
# 启动节点
sudo systemctl start bee
# 停止服务
sudo systemctl stop bee
# 重启节点
sudo systemctl restart bee
# 卸载Bee和Bee-clef
sudo apt-get remove bee
sudo apt-get remove bee-clef
```