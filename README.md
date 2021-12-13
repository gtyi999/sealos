 # 【重要通知】

 [sealer](https://github.com/alibaba/sealer) [ˈsiːlər]密封罐意为把整个集群封装起来。

 阿里巴巴从去年五月份内部开始在某些场景使用sealos，内部fork的版本trident在sealos基础上作出了非常多的优化使系统更加稳定功能更加强大。
 21年开始重大创新的想法也彻底使sealos脱胎换骨，实现整个集群的 build share run, 我们希望这些能力也能够惠及更多开源社区的开发者们。



---



![](./arch.png)

[English Docs](/README_en.md)

# 支持的环境

## Linux 发行版, CPU架构

- Kylin arm64

## kubernetes 版本

1.20.6

## 要求和建议

- 操作系统要求
   - ssh 可以访问各安装节点
   - 各节点主机名不相同，并满足kubernetes的主机名要求。
   - 各节点时间同步
   - 网卡名称如果是不常见的，建议修改成规范的网卡名称， 如(eth.*|en.*|em.*)
   - kubernetes1.20+ 使用containerd作为cri. 不需要用户安装docker/containerd. sealos会安装1.3.9版本containerd。
   - kubernetes1.19及以下 使用docker作为cri。 也不需要用户安装docker。 sealos会安装1.19.03版本docker
 - 网络和 DNS 要求：
   - 确保 /etc/resolv.conf 中的 DNS 地址可用。否则，可能会导致群集中coredns异常。 
   - sealos 默认会关闭防火墙， 如果需要打开防火墙， 建议手动放行相关的端口。
 - 内核要求:
   - cni组件选择cilium时要求内核版本不低于5.4

# 🚀 快速开始

> 环境信息

主机名|IP地址
---|---
master0|192.168.0.2 
master1|192.168.0.3 
master2|192.168.0.4 
node0|192.168.0.5 



**kubernetes .0版本不建议上生产环境!!!**

> 只需要准备好服务器，在任意一台服务器上执行下面命令即可

```sh
# 下载并安装sealos, sealos是个golang的二进制工具，直接下载拷贝到bin目录即可,
$ chmod +x sealos && mv sealos /usr/bin 
# 下载离线资源包
# 安装一个三master的kubernetes集群
$ sealos init --passwd '123456' \
	--master 192.168.0.2  --master 192.168.0.3  --master 192.168.0.4  \
	--node 192.168.0.5 \
	--pkg-url /root/kube1.20.6.tar.gz \
	--version v1.20.6
```

> 参数含义

参数名|含义|示例
---|---|---
passwd|服务器密码|123456
master|k8s master节点IP地址| 192.168.0.2
node|k8s node节点IP地址|192.168.0.3
pkg-url|离线资源包地址，支持下载到本地，或者一个远程地址|/root/kube1.26.6.tar.gz
version|对应的版本|v1.20.6



> 清理集群

```shell script
sealos clean --all
```

# ✅ 特性

- [x] 支持ARM版本离线包，v1.20版本离线包支持containerd集成，完全抛弃docker
- [x] 99年证书, 支持集群备份，升级
- [x] 不依赖ansible haproxy keepalived, 一个二进制工具，0依赖
- [x] 离线安装，离线包包含所有二进制文件配置文件和镜像
- [x] 高可用通过ipvs实现的localLB，占用资源少，稳定可靠，类似kube-proxy的实现

