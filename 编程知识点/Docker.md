# docker 核心概念

docker image
docker container
端口转发
卷挂载


# docker实现原理
https://www.cnblogs.com/crazymakercircle/p/15400946.html#autoid-h2-3-0-4
命令空间，
基于linux namespace技术，每启动一个容器，其实是启动一个带有网络隔离，进程隔离，挂载点隔离等参数的linux进程，
cgroups，control groups

UnionFS

# docker多阶段构建
FROM指令是一个阶段
以发布一个`nodejs`服务为例，需要2个步骤: 安装依赖`npm install` ，构建代码 `npm run build`,启动应用 `npm run start`。如果整个过程放到一个docker镜像中，最后构建出来的镜像包含源代码和node_modules, 会导致2个问题：镜像的体积很大，泄露源代码风险。
程序运行需要的是打包后的代码，不需要源代码和`node_modules`(如果是nodejs 不需要devDependencies)，我们可以将整个过程分成2个阶段，第一个阶段安装依赖和构建，第二个阶段只需要把第一个阶段的产物拷贝出来，然后启动应用即可。


# docker compose


# docker network

# Dockerfile 原理
dockerfile核心概念：镜像分层（image layers）
