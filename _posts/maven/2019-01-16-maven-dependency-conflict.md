---
layout: post
title: Maven依赖冲突解决思路
date: 2019-01-16 17:20:02.000000000 +08:00
tags: 
  - Maven
  - Arthas
---

第一步，使用`mvn dependency:tree`查看依赖树，若要查找指定依赖包的依赖树情况，加上`-Dincludes=groupId:artifacetId:version`选项进行过滤：

```bash
$ mvn dependency:tree -Dverbose -Dincludes=com.alibaba:fastjson
```

备注：加上`-Dverbose`可以看到更全的、更详细的传递依赖。

第二步，使用`<exclusion></exclusion>`排除掉不想要的传递依赖包。

<hr />

想查看运行时使用的是哪个jar包版本，可以利用Arthas工具的`sc`命令：

在机器上下载arthas：`curl -O https://arthas.aliyun.com/arthas-boot.jar`

运行arthas：`java -jar arthas-boot.jar`

运行`sc`指令，查看JVM加载的类信息：
```bash
[arthas@1804]$ sc -d org.apache.commons.lang3.StringUtils
 class-info        org.apache.commons.lang3.StringUtils
 code-source       /home/admin/xx/target/xxx.jar/plugins/xx-client/lib/commons-lang3-3.4.jar
 name              org.apache.commons.lang3.StringUtils
 isInterface       false
 isAnnotation      false
 isEnum            false
 isAnonymousClass  false
 isArray           false
 isLocalClass      false
 isMemberClass     false
 isPrimitive       false
 isSynthetic       false
 simple-name       StringUtils
 modifier          public
 annotation
 interfaces
 super-class       +-java.lang.Object
 class-loader      +-xxx's ModuleClassLoader
 classLoaderHash   7c7a06ec

 class-info        org.apache.commons.lang3.StringUtils
 code-source       /home/admin/xx/target/xxx.jar/plugins/xx/lib/commons-lang3-3.4.jar
 name              org.apache.commons.lang3.StringUtils
 isInterface       false
 isAnnotation      false
 isEnum            false
 isAnonymousClass  false
 isArray           false
 isLocalClass      false
 isMemberClass     false
 isPrimitive       false
 isSynthetic       false
 simple-name       StringUtils
 modifier          public
 annotation
 interfaces
 super-class       +-java.lang.Object
 class-loader      +-mtop-uncenter's ModuleClassLoader
 classLoaderHash   6ce86ce1

 class-info        org.apache.commons.lang3.StringUtils
 code-source       /home/admin/xx/target/xx/BOOT-INF/lib/commons-lang3-3.5.jar
 name              org.apache.commons.lang3.StringUtils
 isInterface       false
 isAnnotation      false
 isEnum            false
 isAnonymousClass  false
 isArray           false
 isLocalClass      false
 isMemberClass     false
 isPrimitive       false
 isSynthetic       false
 simple-name       StringUtils
 modifier          public
 annotation
 interfaces
 super-class       +-java.lang.Object
 class-loader      +-com.xx.xx.boot.loader.LaunchedURLClassLoader@8807e25
                     +-sun.misc.Launcher$AppClassLoader@4e0e2f2a
                       +-sun.misc.Launcher$ExtClassLoader@2eafffde
 classLoaderHash   8807e25
```

可以看到`org.apache.commons.lang3.StringUtils`类被两个类加载器加载了，而且jar包版本都不一样。

<hr />

参考：
- <a href="https://maven.apache.org/plugins/maven-dependency-plugin/tree-mojo.html" target="_blank">dependency:tree</a>
- <a href="https://arthas.aliyun.com/doc/sc.html" target="_blank">Arthas sc</a>











