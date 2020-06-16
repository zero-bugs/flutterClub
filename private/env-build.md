# Flutter开始使用的一些配置

**flutter学习入门**



开学学习flutter有半年了，始终没有大的突破，主要原因是懒。程序员怎么能懒呢^_^


## 环境搭建部分

*Flutter构建是基于Gradle，跟我们经常使用的Maven和ant很不一样，这部分增加了学习成本。Gradle是面向编程式配置，而不是面向xml配置，build everything。*

![Pandao editor.md](https://pandao.github.io/editor.md/images/logos/editormd-logo-180x180.png "Pandao editor.md")



## Gradle下载地址
**** 地址：https://gradle.org/releases/ ****
**** 本次使用的版本是Gradle5.6，下载压缩包，然后解压到指定目录（实际开发时可以放到D盘自定义目录D:\DevTool\）。****
**** 环境变量设置：新建GRADLE_HOME=D:\DevTool\gradle-6.5，然后加入Path=Path;%GRADLE_HOME%\bin，调出任务终端查看版本号****

	\> gradle -v
	------------------------------------------------------------
	Gradle 6.5
	------------------------------------------------------------

	Build time:   2020-06-02 20:46:21 UTC
	Revision:     a27f41e4ae5e8a41ab9b19f8dd6d86d7b384dad4

	Kotlin:       1.3.72
	Groovy:       2.5.11
	Ant:          Apache Ant(TM) version 1.10.7 compiled on September 1 2019
	JVM:          1.8.0_181 (Oracle Corporation 25.181-b13)
	OS:           Windows 10 10.0 amd64>`


## Gradle配置文件
**** Gradle不需要配置全局配置文件，基于单个项目设置脚本。在项目的gradle.properties文件中配置相应****
**** AndroidStudio中的Gradle与独立插件版本对应关系： https://developer.android.com/studio/releases/gradle-plugin#updating-gradle ****
### gradle.properties
	org.gradle.jvmargs=-Xmx1536M
	org.gradle.daemon=true
	org.gradle.parallel=true

	android.enableR8=true
	android.useAndroidX=true
	android.enableJetifier=true

	systemProp.http.proxyHost=
	systemProp.http.proxyPort=
	systemProp.http.proxyUser=
	systemProp.http.proxyPassword=
	systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost

	systemProp.https.proxyHost
	systemProp.https.proxyPort=
	systemProp.https.proxyUser=
	systemProp.https.proxyPassword=
	systemProp.https.nonProxyHosts=*.nonproxyrepos.com|localhost

### build.gradle
	buildscript {
		ext.kotlin_version = '1.3.50'
		repositories {
			google()
			jcenter()
			//国内配置
	//        maven{ url 'https://maven.aliyun.com/repository/google' }
	//        maven{ url 'https://maven.aliyun.com/repository/jcenter' }
	//        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public' }
		}

		dependencies {
			classpath 'com.android.tools.build:gradle:3.5.3'
			classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
		}
	}

	allprojects {
		repositories {
			google()
			jcenter()
			//国内配置
	//        maven{ url 'https://maven.aliyun.com/repository/google' }
	//        maven{ url 'https://maven.aliyun.com/repository/jcenter' }
	//        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public' }
		}
	}

## Git配置
### 下载地址:https://git-scm.com/download/win/
### 学习地址：https://www.liaoxuefeng.com/wiki/896043488029600
##### git配置
代理配置

	git config --global proxy.http http://userName:passWd@host:port
	git config --global proxy.https http://userName:passWd@host:port

配置查询

	git config --global proxy.http
	git config --global proxy.https

代码管理配置

	git config --global alias.st status
	git config --global alias.co checkout
	git config --global alias.ci commit
	git config --global alias.br branch

##### git常用命令

	git last
	git checkout
	git branch
	git status
	git commit
	git add

#####Flutter用到的命令

	C:\src>git clone https://github.com/flutter/flutter.git -b stable



## Flutter配置
### 学习地址:https://flutterchina.club/
**** 在国内环境下，可以使用清华大学镜像地址：https://mirrors.tuna.tsinghua.edu.cn/help/flutter/****

清华 TUNA 协会
定时与 Flutter 社区 Storage 镜像同步，Pub API 采取定时主动抓取策略，镜像配置了完善的失败回源策略（推荐）。

	PUB_HOSTED_URL:https://mirrors.tuna.tsinghua.edu.cn/dart-pub
	FLUTTER_STORAGE_BASE_URL:https://mirrors.tuna.tsinghua.edu.cn/flutter

CNNIC
基于 TUNA 协会的镜像服务，数据策略与 TUNA 一致，通过非教育网的域名访问。

	PUB_HOSTED_URL:http://mirrors.cnnic.cn/dart-pub
	FLUTTER_STORAGE_BASE_URL:http://mirrors.cnnic.cn/flutter

腾讯云开源镜像站
定时（每天凌晨）与 TUNA 协会镜像同步，数据有延迟，访问速度有待反馈。

	PUB_HOSTED_URL:https://mirrors.cloud.tencent.com/dart-pub
	FLUTTER_STORAGE_BASE_URL:https://mirrors.cloud.tencent.com/flutter


### End
