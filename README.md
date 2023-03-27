# jupyter-image-stacks

## 我是谁
 用于支撑eoelab基础计算设施jupyterhub的镜像制作项目

## 目前支持清单
1. Python  
这包括Python支持，conda，pip
2. DataScience  
这包括Python与科学计算常用包支持，Julia支持，R与常用包支持  
Julia使用自己的PKG包管理工具，所以使用时注意对PKG换源并且安装包  
3. Sagemath  
这是一个遵循GPL的开源数学工具  
4. DeepLearning  
通常只提供Python语言支持，集成TensorFlow,Pytorch,Transformer,Oneflow支持  
**对于DL与DataScience，提供GPU支持，包括CUDA等**  
5. MATLAB  
一个工科常用的数学工具，但是需要用户自己具有许可证  
**这个镜像可能会在下一版本中被移除支持**  
## 如何使用
使用docker作为运行时平台  
更多的使用细节可以查看jupyter团队项目，需要额外关注的就是该项目内镜像默认容器端口都是8888  
https://github.com/jupyter/docker-stacks  
## 如何提交意见或参与
如果您有更好的思路，可以在本项目中提出issue  
## 注意
1. 默认情况下，项目内main分支所有Dockerfile是经过测试而发布的，test分支是在测试的镜像，欢迎进入测试
2. 本项目默认落地场景是我们自建的K8S，K8S底层是RKE，也就是docker
