# 				Linux系统安装软件



## 安装JDK

先上oracle官网下载jdk
[jdk8下载地址](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

![下载这种格式](https://img-blog.csdnimg.cn/20200616213817624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU3MTc5NQ==,size_16,color_FFFFFF,t_70)

  1. 使用xShell连接虚拟机服务器


  2. 在根目录下使用 ` mkdir -p user/jdk ` 命令创建一个jdk要放入的目录
  3. 使用 xftp连接虚拟机服务器，把下载好的jdk压缩包放入以上目录文件夹中
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616215116236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU3MTc5NQ==,size_16,color_FFFFFF,t_70)
  4. 进入/user/jdk目录中，执行`tar -xzvf jdk-8u251-linux-x64.tar.gz`
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200616215255653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU3MTc5NQ==,size_16,color_FFFFFF,t_70)
  5. 使用指定:`vim /etc/profile` 编辑profile文件配置java_home
   `export JAVA_HOME=/user/jdk/jdk1.8.0_251
   export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   export PATH=$JAVA_HOME/bin:$PATH`
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618221032691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU3MTc5NQ==,size_16,color_FFFFFF,t_70)

    6.   让profile文件生效
      `source /etc/profile`

     7.   查看使用是否成功
    `javac -version` 或者 `javac`
注：
	 如果出现这类
	 -bash: /usr/local/java/jdk1.8.0_171/bin/java: /lib/ld-linux.so.2: bad ELF interpreter: No such file问题

   运行这个命令解决：sudo yum install glibc.i686



## 安装git

### 源码编译安装(推荐使用此方法安装)

操作如下:

  1. 获取github最新的Git安装包下载链接，进入Linux服务器，执行下载，命令为：`wget https://github.com/git/git/archive/v2.27.0.tar.gz`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618211601778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU3MTc5NQ==,size_16,color_FFFFFF,t_70)

 注：如果![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618202947413.png)
 安装指令即可: `yum -y install wget`


  2. 压缩包解压，命令为：`tar -zxvf v2.27.0.tar.gz `
  3. 安装编译源码所需依赖，命令为:`yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker` 出现提示y即可
  4. 安装依赖时，yum自动安装了Git，需要卸载旧版本Git，命令为:`yum remove git`出现提示y即可，如果没有使用yum安装，跳过此步骤即可
  5. 进入解压后的文件夹，命令` cd git-2.27.0` ，然后执行编译，命令为:`make prefix=/usr/local/git all`
  6. 安装Git至/usr/local/git路径，命令为:`make prefix=/usr/local/git install`
  7. 打开环境变量配置文件，命令：`vim /etc/profile`在底部加上Git相关配置信息:
    PATH=$PATH:/usr/local/git/bin
    export PATH 
  8. 让profile文件生效
  `source /etc/profile`
  9. 输入命令`git --version`查看版本



### yum安装

1.安装
执行命令:`yum install git` 界面如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618201321976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU3MTc5NQ==,size_16,color_FFFFFF,t_70)
出现询问指令，选择 y
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618201440460.png)
安装完毕
 2.验证安装
 执行命令`git --version`查看版本
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618201619892.png)

  3. Git默认安装在/usr/libexec/git-core目录下，可输入指令，查看安装信息：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020061820184913.png)

 4.存在的问题

使用yum安装确实简单方便，但yum存在一个问题就是安装的版本不好控制，如上图所示，安装的版本为1.8.3，这个版本太老了。所以推荐源码编译安装
登录[github的Git版本发布界面](https://github.com/git/git/releases)，可以看到目前最新的版本为2.27，如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200618202112732.png?)