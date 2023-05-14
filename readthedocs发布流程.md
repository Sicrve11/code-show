# readthedocs 介绍篇
## 1.简单介绍
readthedocs是一个类似于github的免费文档托管网站，相比于github，这个网站使用html文件，因此适合展示某个软件包的安装流程、用法、教程文档等。readthedocs中常规的文档格式是左侧为目录栏，右侧为对应的页面，详见（https://prost-doc.readthedocs.io/en/latest/index.html）

## 2.存在的必要性
如果单独使用github来放置一个项目，则除了封装好的软件外，图片、数据等等都放进github中，而这些文件并不是读者需要下载的，这无疑会使github软件包变得更加的臃肿，不利于通过git或clone的方式来快速获取软件包。


# readthedocs 配置篇
## 1.需要写html吗？
readthedocs虽然使用html文件，但是其并不需要具有html等前端基础（makeup），而是可以使用.rst文件（reStructuredText）搭配makedown文件使用。makedown文件是面向无前端技术人员，但有希望有一定格式的文件格式，使作者更加关注内容而不是如何修饰。readthedocs在python中提供软件包，可以快速将连接好的rst文件和md文件构建为html文件。

构建流程如下:

a.首先需要配置好Python环境，同时安装最新版本的 Sphinx 及依赖，
	
	Python - conda即可

	pip3 install -U Sphinx
	pip3 install sphinx-autobuild
	pip3 install sphinx_rtd_theme
	pip3 install recommonmark
	pip3 install sphinx_markdown_tables

b.创建项目
在linux的bash中或者windows的anaconda的powershell下，先将目录更换为指定项目文件夹下，执行下面语句可以创建项目：
	
	sphinx-quickstart

    [y/n] # y/n都可以

接着输入项目名称、作者信息、项目语言等
构建成功后会得到一个文件夹，里面包含了以下文件：

	Makefile：可以看作是一个包含指令的文件，在使用 make 命令时，可以使用这些指令来构建文档输出。
	make.bat：Windows 用命令行。

	_static：静态文件目录，比如图片等。
	_templates：模板目录。

	conf.py：存放 Sphinx 的配置，包括在 sphinx-quickstart 时选中的那些值，可以自行定义其他的值。
	index.rst：文档项目起始文件

	build：生成的文件的输出目录。

其中，
Makefile，make.bat是执行文件；
_static、_templates是材料的目录；
conf.py是Sphinx 的配置文件，可以在里面修改配置参数，如更换主题，md文件扩展等；
index.rst即为文档起始文件，可以在里面连接md文件，自由配置；

然后，执行下面语句即可在build文件夹下生成html文件。

    make html（./make html）
    # 或者使用下面语句直接构建加激活
    sphinx-autobuild source build/html


【使用 sphinx-autobuild source build/html 可以直接激活，在浏览器中输入http://127.0.0.1:8000可以看到当前构建的readthedocs的效果】

conf.py文件的配置具体可以参考网上的资源：

	https://zhuanlan.zhihu.com/p/264647009
	https://zhuanlan.zhihu.com/p/473812418
	https://zhuanlan.zhihu.com/p/112919704

配置好conf.py文件（md文件扩展，主题等），即可添加各种“.md”文件，如每一个md文件中介绍不同的内容：安装、用法、教程等等。然后在index.rst中将这些md文件连接起来，形成目录和单独页面。【使用 sphinx-autobuild source build/html 可以在修改本地文件后自动重建】

index.rst的目录可以参考下面这个结构：

    Welcome to PROST
    =========================================

    **PROST** is ....   

    .. image:: ./_images/figure/figure1.png
       :align: center
       :alt: PROST


    Using **PROST** you can do: 

    - xx.
    - yy. 


    .. toctree::
       :maxdepth: 1
       :caption: Contents:

       Installation
       Easy Start
       How to use PROST
       Tutorials
       Api


    ==========================================


其中：
这一段可以添加图片

    .. image:: ./_images/figure/figure1.png
       :align: center
       :alt: PROST

这一段即为目录结构，深度为1，目录项有：Installation、Easy Start、How to use PROST、Tutorials、Api

    .. toctree::
       :maxdepth: 1
       :caption: Contents:

       Installation
       Easy Start
       How to use PROST
       Tutorials
       Api

【注意目录项前是三个空格，同时要与:caption: Contents:空一行】
配置好后，下面就是如何上传至readthedocs网站中。


# readthedocs 上传篇
对于一个软件包（Python/R等），由于可能会用到很多现成的框架，交叉使用后难免会遇到环境配置复杂，跨环境使用等情况。上传readthedocs网站时，readthedocs需要构建python环境并安装相应的软件包，如果结构过于复杂并且不符合sphinx标准，容易上传失败。 

因此，这里建议单独生成一个小项目，里面只有项目的各种介绍文档等，大致流程如下：

    1.创建github代码仓库；
    2.设置本地文件夹，包含配置文件和构建好的readthedocs文件；
    3.上传github；
    4.在readthedocs网站中使用自己的github登录，将刚刚创建好的项目导入、构建即可。


下面介绍一下本地文件夹中如何配置，具体可以参考【readthedocs-example.zip】文件中的内容：
首先创建一个文件夹，用于存储所有文件，如【readthedocs-example】
在该目录下，创建【source】文件夹，并将上述配置好的文件挪到【source】文件夹中（除了Makefile、make.bat），然后添加README.md、requirements.txt、.readthedocs.yaml、.gitignore这四个文件

其中，

    README.md		github代码仓库所需要的项目介绍文件	
    gitignore		上传github时忽略的内容（里面写【build/】即可，表示不把本地构建好的上传，而是在readthedocs网站中构建，这样上传好的github文件中也没有build这个文件）
    requirements.txt	构建时所需要的包（主要是sphinx包以及md文件扩展包）参考如下：
	    sphinx==5.1.1
	    recommonmark==0.7.1
	    sphinx-markdown-tables==0.0.17

    .readthedocs.yaml	readthedocs网站中在构建html时的环境配置（系统版本、语言、配置路径、安装扩展包的列表等）这个文件readthedocs中有提供模板，可以参照下面这段：
        '''
        # .readthedocs.yaml
        # Read the Docs configuration file
        # See https://docs.readthedocs.io/en/stable/config-file/v2.html         for details

        # Required
        version: 2

        # Set the version of Python and other tools you might need
        build:
          os: ubuntu-22.04
          tools:
            python: "3.11"
            # You can also specify other tool versions:
            # nodejs: "19"
            # rust: "1.64"
            # golang: "1.19"

        # Build documentation in the docs/ directory with Sphinx
        sphinx:
           configuration: source/conf.py

        # If using Sphinx, optionally build your docs in additional         formats such as PDF
        # formats:
        #    - pdf

        # Optionally declare the Python requirements required to build      your docs
        python:
           install:
           - requirements: ./requirements.txt
        '''
        
最后在readthedocs网站中登录自己的github账号，然后将这个项目导入即可。

