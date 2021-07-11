+++
title = "kuberntes 快速构建环境操作及流程"
draft = false
+++

<div class="ox-hugo-toc toc">
<div></div>

<div class="heading">Table of Contents</div>

- [kuberntes 快速构建环境操作及流程](#kuberntes-快速构建环境操作及流程)
    - [创建对应的namespace](#创建对应的namespace)
    - [添加 harbor project](#添加-harbor-project)
    - [jenkins 自动配置基础组件](#jenkins-自动配置基础组件)
    - [jenkins上添加相应的cmp job](#jenkins上添加相应的cmp-job)
        - [创建一个 list view](#创建一个-list-view)
        - [创建job](#创建job)
        - [修改job的配置](#修改job的配置)
        - [修改Jenkinsfile](#修改jenkinsfile)
    - [自测环境](#自测环境)

</div>
<!--endtoc-->



## kuberntes 快速构建环境操作及流程 {#kuberntes-快速构建环境操作及流程}


### 创建对应的namespace {#创建对应的namespace}

namespace不会自动创建，所以这个必须得通过手动创建来控制

```sh
kubectl create ns xxxxxx-project
```


### 添加 harbor project {#添加-harbor-project}

创建一个 `public` 的project，名字叫做 `xxxxxx-project`


### jenkins 自动配置基础组件 {#jenkins-自动配置基础组件}

jenkins地址: <http://jenkins.rctest.com/view/rightcloud-template/job/template-infra-create-k8s/>

{{< figure src="/ox-hugo/20190404_110330_ou9SQi.png" >}}

namespace 为 `xxxxxx-project`, 这里如果是第一次初始化，需要勾选 `INIT_DB`, 如果需要重制数据库，这勾选 `FORCE_INIT_DB` 。 region是选择k8s环境，项目环境为 `product`, 自测环境为 `self_env` 。

等待jenkins完成之后，包含基础组件的环境就准备好了


### jenkins上添加相应的cmp job {#jenkins上添加相应的cmp-job}

这个时候我们需要给jenkins添加cmp相关的job.


#### 创建一个 list view {#创建一个-list-view}

比如 `project-xxxxx`

{{< figure src="/ox-hugo/20190404_112734_QUVWm4.png" >}}


#### 创建job {#创建job}

创建对应的job， 从 `vue-project` 里面拷贝一份， 比如这里创建一个 `xxxxx-server`, `Copy from` 如下:

{{< figure src="/ox-hugo/20190404_110908_sslbiX.png" >}}


#### 修改job的配置 {#修改job的配置}

完成后，修改该job的配置:

-   修改 `parameter` 中 `namespace` 为 `xxxxxx-project` ，这个只是在第一次编译有效，后面的会根据 `Jenkinsfile` 设置的默认参数来适配
-   修改 `SCM` 里面的 `Repository URL` 为目标的gitlab地址
-   修改 `Branches to build` 为 `Jenkinsfile` 所在的分支
-   修改 `Script Path` 为对应的 `Jenkinsfile` 在repo中的路径 （一般不用改）

{{< figure src="/ox-hugo/20190404_113131_x0yvln.png" >}}

-   保存


#### 修改Jenkinsfile {#修改jenkinsfile}

找到 `Jenkinfile`

```groovy
@Library("jenkins-share-library@v3") _

stageK8SServerAuto()
```

到 `jenkins share lib` 里面找到对应的文件(这里的v3表示分支)， gitlab地址: <http://218.17.169.171:8082/cloudstar/jenkins-share-library>

```groovy
def call(defaultnamespace="") {
}
```

可以看到这里的参数有个 `defaultnamespace` ， 这里可以指定 `namespace` , 如果 `namespace` 的格式为 `xxxxxx-project` 的话，程序会自动判定为 `项目` ， 会自动将 `gitlab` 的地址设置为 `/ProjectGroup/xxxxxx` ，这个根据具体情况修改namespace就好了。

修改完成就可以编译了。

其他的项目也是通过同样的方式来修改。


### 自测环境 {#自测环境}

自测环境只需要创建namespace，发布基础组件就好了，当需要cmp的时候，通过jenkins job编译相应组件: <http://jenkins.rctest.com/view/rightcloud-self-test/job/rightcloud-self-env-manual-k8s/>

{{< figure src="/ox-hugo/20190404_114821_AxKV83.png" >}}

根据需要填写分支，勾选组件编译就好了.
