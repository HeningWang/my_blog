---
title: 如何在hexo博客中加入LaTeX语法支持？
date: 2020-05-07 13:14:21
tags: [hexo,markdown,LaTex,blog]
categories: [计算机]
---

hexo博客默认的编译器marked属于基本的markdown渲染器，只支持基本的markdown语法，不支持LaTeX数学公式的表达。若想支持LaTex语法，需要更换默认的渲染器，在这里我使用的是Pandoc。Pandoc是一款能够高质量转换文档格式的本地转换工具，其支持的语言十分丰富，而且转换后能够保持格式的完整性,被誉为文本格式转换届的瑞士军刀。

获取地址：https://www.pandoc.org/installing.html（内附不同操作系统的安装说明）

下载完成后可以在cmd中输入：

```
pandoc --version
```

查看pandoc是否正确安装。接着，我们卸载hexo默认的渲染器，改为Pandoc。

```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

在用node安装和卸载渲染器的过程中，我碰到了以下的报错信息：

```
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN fsevents@1.2.0 had bundled packages that do not match the required version(s). They have been replaced with non-bundled versions.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.0 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.0: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
```

这是由于fsevnets是MacOS系统下的插件，不支持windows和linux系统，但安装的结果显示为0，说明插件已经正确安装，不影响正常使用，忽略即可。

```
hexo clean
hexo g
```

执行以上命令清除hexo在publics文件夹中的缓存，重新生成后，即可正确地显示LaTex公式内容，以下是测试结果：

```
\begin{equation}
E=mc^2.
\end{equation}
```

以上代码可显示为：

\begin{equation}
E=mc^2.
\end{equation}

至此，LaTeX语法支持成功。