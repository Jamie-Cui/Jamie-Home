---
layout: post
title:  "如何登录 informatics gpu cluster 教程"
date:   2018-2-06 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

写给不知道怎么连接学校gpu cluster的学子～ 

登录dice打开一个terminal，或者使用windows，linux ssh到dice均可

`ssh s17*****@student.ssh.inf.ed.ac.uk`

<img src="{{site.url}}{{site.baseurl}}/img/1.png" alt="Drawing" style="width: 400px;"/>

接下来从登录设备登录到真正的dice上

`ssh student.login`

<img src="{{site.url}}{{site.baseurl}}/img/2.png" alt="Drawing" style="width: 400px;"/>

登录到mlp专属“服务器”上

`ssh mlp1` 或者 `ssh mlp2`

<img src="{{site.url}}{{site.baseurl}}/img/3.png" alt="Drawing" style="width: 400px;"/>

接下来的过程就是重新安装miniconda，将github里面的东西下载下来，如果知道怎么安的就可以跳过下面这些自己安装了。

首先下载老师写好的bash脚本

`wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh`

运行脚本，注意在运行脚本的时候最后一个问题会问是不是将miniconda的几个命令写入.bashrc这里一定要写yes不要回车，因为默认选项是no

`bash Miniconda3-latest-Linux-x86_64.sh`

重新将bash环境（progile）设置（？）

`source .bashrc`

新建miniconda中的mlp配置环境

`conda create -n mlp python=3`

激活mlp配置环境

`source activate mlp`

在mlp环境中安装git，因为正常pip安装和apt安装都需要root权限，而我们是没有权限的

`conda install git`

再配置一下下

```
cd ~/miniconda3/envs/mlp
mkdir -p ./etc/conda/activate.d
mkdir -p ./etc/conda/deactivate.d
echo -e '#!/bin/sh\n' >> ./etc/conda/activate.d/env_vars.sh
echo "export MLP_DATA_DIR=$HOME/mlpractical/data" >> ./etc/conda/activate.d/env_vars.sh
echo -e '#!/bin/sh\n' >> ./etc/conda/deactivate.d/env_vars.sh
echo 'unset MLP_DATA_DIR' >> ./etc/conda/deactivate.d/env_vars.sh
export MLP_DATA_DIR=$HOME/mlpractical/data
```

到这里我们的miniconda就是安装完了，借下来把mlpractical搞下来，切换到一个branch里面

```
cd ~
git clone https://github.com/CSTR-Edinburgh/mlpractical.git
cd mlpractical
git checkout mlp2017-8/semester_2_materials
```

接下来我们要修改一个脚本文件

`vim gpu_cluster_tutorial_training_script.sh`

之后把自己的学号改上去

<img src="{{site.url}}{{site.baseurl}}/img/4.png" alt="Drawing" style="width: 700px;"/>

ESC之后`:wq`保存文件，然后运行

`sbatch gpu_cluster_tutorial_training_script.sh`

之后我们就可以使用`squeue`来查看自己的任务进程了

<img src="{{site.url}}{{site.baseurl}}/img/5.png" alt="Drawing" style="width: 700px;"/>

如果有错误可以在目录下面的sample_error里面观看