# jupyter-image-stacks
## 如果您通过github访问本项目，请注意
1. github上的仓库是由源仓库推送的镜像仓库，是一个镜像仓库
2. 我们的源仓库是 https://eoelab.org:1031/build-image-stacks/jupyter-image-stacks  
3. 我们的docker镜像仓库是 https://hub.docker.com/r/ben0i0d/jupyter   
4. 对于issue/PR，我们推荐在源仓库上提，这对于我们工作更方便，但是如果您在github上提，我们也会跟进处理  
## 项目梗概
### 用途
用于支撑我们基础计算设施与在线IDE-Jupyterhub的镜像构建项目，测试与工作场景是Rancher管理的基于RKE2的私有K8S集群，并完成汉化，扩展等工作
### 如何使用
#### Docker
镜像可以像jupyternotebook一样使用，容器端口为8888  
对于用后即抛地使用，可以用如下指令,注意这没有数据持久化，意味着你需要使用诸如Git等工具同步您的工作进度  
`docker run -p 8888:8888 ben0i0d/jupyter:<name>`  
对于需要长期使用，可以用如下指令，这将挂载一个目录到容器内,以提供数据持久化  
`docker run -it --rm -p 8888:8888 -v "${PWD}":/home/jovyan ben0i0d/jupyter:<name>`  
#### Jupyterhub
在singleuser内的profile指定镜像即可  
例如：
```
    - description: DL environment with GPU
      display_name: DL_GPU environment
      kubespawner_override:
        extra_resource_limits:
          nvidia.com/gpu: '1'
        image: ben0i0d/jupyter:dl-g
```
### 当前构建镜像清单
* Llinux：在无特权的情况下学习Linux系统   
* Python：对Python语言的基础支持  
* Scipy：Python的科学计算环境  
* Julia：对科学计算语言julia的支持    
* R：对科学计算语言R的支持    
* Haskell:对Haskell的支持  
* Java:对Java的支持
* Go:对Go的支持
* Rust:对Rust的支持
* CPP：对CPP（11,14,17）的支持，并且镜像包括了传统Linux必须的C、Cpp工具链（build-essential）  
注意:在Notebook中，您的语法结构需要做一定改动  
例如,Helloworld程序代码为：  
```
    #include <iostream>
    std::cout << "Hello, world!" << "\n";
```
* Pyspark: 提供Python对Apache Spark的支持  
* DeepLearning（With GPU）：只提供Python语言支持，集成TensorFlow,Pytorch,Transformer,Oneflow支持  
* Octave: 提供对Octave这一门科学计算语言的支持，其类似于matlab，但占用内存小，广受机器学习爱好者的喜爱  
注意:使用到package时候，要将其加载  
例如：在使用image中函数时，遇到了图片.png  
使用以下代码加载image  
```
pkg load image
```
* Sagemath：一个遵循GPL的开源数学工具  
### 镜像依赖关系
```mermaid
graph LR
A(Upstream)-->B(Python)-->C(Scipy)
A-->F(Julia)
A-->G(R)
A-->H(Sagemath)
A-->I(CPP)
A-->J(llinux)
A-->K(Haskell)
A-->L(Java)
A-->M(Go)
A-->N(Rust)
A-->O(Octave)
C(Scipy)-->D(Deeplearning)  
C(Scipy)-->E(Pyspark)  
```  
### 如何参与
项目内main分支Dockerfile是经过测试而发布的，如果您有测试或者新需求，请构建一个新分支，注意修改新分支内的CI配置文件，并且在提交合并请求时还原CI配置
## 上游
### 软件包上游与版本
Python 3.10  
Julia 1.9.0  
Java zulu17-jdk  
Tensorflow 默认情况下受支持的最新版  
pytorch 默认情况下受支持的最新版  
oneflow 默认情况下受支持的最新版  
cuda 11.6.2  
cudnn 8  
conda bfsu：https://mirrors.bfsu.edu.cn/help/anaconda/  
pip bfsu：https://mirrors.bfsu.edu.cn/help/pypi/  
apt ustc：https://mirrors.ustc.edu.cn/help/ubuntu.html  
npm npmmirror(AliYun): https://registry.npmmirror.com/  
apache tuna: https://mirrors.tuna.tsinghua.edu.cn/apache/  
julia-pkg ustc: https://mirrors.ustc.edu.cn/julia/  
hackage ustc: https://mirrors.ustc.edu.cn/hackage/  
Stackage ustc: https://mirrors.ustc.edu.cn/stackage/  
GO AliYun: https://mirrors.aliyun.com/goproxy/  
### 项目上游
jupyter团队项目 https://github.com/jupyter/docker-stacks  
**但是我们与上游差别较大，包括源，软件包，本地化与扩展等，因此如果您从本项目派生遇到问题，请不要到jupyter团队提问，这会加大他们的工作量**
### kernel
* Cpp: https://github.com/jupyter-xeus/xeus-cling
* Python：https://ipython.org/
* Go: https://github.com/gopherdata/gophernotes
* Haskell: https://github.com/gibiansky/IHaskell
* Java: https://github.com/SpencerPark/IJava
* Julia: https://github.com/JuliaLang/IJulia.jl
* R: http://irkernel.github.io/
* Rust: https://github.com/evcxr/evcxr
* Octave: https://github.com/Calysto/octave_kernel
## 必要的版权说明
对于派生自jupyter团队的代码，我们添加了如下的版权声明，我们保留并且支持jupyter开发团队版权
```
# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
```
对于派生自ihaskell-notebook的代码，我们添加了如下的版权声明，我们保留并且支持James Brock <jamesbrock@gmail.com>版权
```
# Copyright (c) James Brock.
# Distributed under the terms of the Modified MIT License.
```
