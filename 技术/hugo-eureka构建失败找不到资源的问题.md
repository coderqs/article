---
title: hugo-eureka 构建网站失败，找不到资源的问题解决
description: '本文主要是解决在使用 hugo 的 eureka 主题构建网站时出现的 `Error building site: POSTCSS: failed to transform "css/eureka.css" (text/css): resource "css/hugo-eureka/css/eureka.css_fc3f76d7bee2760c3a903059afc3d9b2" not found in file cache` 的错误，以及在解决过程中出现的其他问题。'
author: 清松
date: 2024-03-20T15:02:26+08:00
categories:
  - 技术
tags:
  - Hugo
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXQHSDGZNMVG4BW89P47
---
## 背景
本人在 Github Actions + Hugo 构建网站时，出现了错误
```
Error building site: POSTCSS: failed to transform "css/eureka.css" (text/css): resource "css/hugo-eureka/css/eureka.css_fc3f76d7bee2760c3a903059afc3d9b2" not found in file cache
```
这个错误也不算陌生，因为使用这个主题也得有一年了，这个问题也遇到过好多次，之前稀里糊涂的不知道怎么解决了就过去了，可这回没那么容易就过去，所以就记录一下过程和解决方法，希望能帮助到健忘的自己🤣

## 解决过程

问题看起来不难理解，就是找不到文件了嘛，但是为什么会找不到呢？我没搞过前端的和 go ，也不清楚它是怎么个生成逻辑，就只能一步一步找资料来分析了呗。
这种高频出现的问题一般都会有人在主题的 ISSUES 中反映，也确实找到了，从作者 Wangchucheng 的回答来看([issues#186](https://github.com/wangchucheng/hugo-eureka/issues/186))，这应该跟 Hugo 有关
> [@Flashpok](https://github.com/Flashpok)看来这是 Hugo 引起的问题，这里已经打开了一个问题：[gohugoio/hugo#9787](https://github.com/gohugoio/hugo/issues/9787)。您可能会在任何使用资源缓存的主题中遇到此问题，尤其是 PostCSS 主题。
>
> 临时解决方法是将[此处的](https://github.com/wangchucheng/hugo-eureka/tree/master/resources/_gen/assets/css)文件夹和文件复制到项目的`resources/_gen/assets/css/<base-path>/`. 然后 Hugo 就可以在 中找到资源缓存`resources/_gen/assets/css/<base-path>/css/...`。如果`baseURL`设置为`example.com/test/`，则`base-path`这里是`test`。

回答中的[gohugoio/hugo#9787](https://github.com/gohugoio/hugo/issues/9787)提到了需要使用新版重新生成缓存，作者 Wangchucheng 在主题的文档中也提到过

>如果您遇到类似错误`Error: Error building site: POSTCSS: failed to transform "css/eureka.css" (text/css): resource "css/css/eureka.css_09b9b8e200b8c676e85ddca87a9596ca" not found in file cache`，请尝试将Hugo更新到0.100.0或更高版本。

这么看起来应该很明确了，使用 hugo 的 最新版本(需要是支持 postcss 的 extended 版本) 就能解决问题，然而并没有。我是用的是 Github Actions + Hugo 的方式，在 `peaceiris/actions-hugo@v2` 中设置的 `hugo-version: latest` 和 `extended: true`。也在 Github Actions 日志中打印了 hugo 的版本信息
```
hugo v0.124.0-629f84e8edfb0b1b743c3942cd039da1d99812b0+extended linux/amd64 BuildDate=2024-03-16T15:44:32Z VendorInfo=gohugoio
```
确认是最新版本，但是问题还是存在。

### 其他方法
没有解决问题就继续找其他方法，在 Hugo 的论坛中又找到了相关的提问([Error building site: POSTCSS](https://discourse.gohugo.io/t/error-building-site-postcss/31766))，其中 jmooring 给出的建议是将 `config.yaml` 文件中的
``` ymal
build: 
	useResourceCacheWhen: always
```
替换成
``` yaml
build: 
	useResourceCacheWhen: fallback
```
运行测试一下，发现问题变了，变成了
```
Error: error building site: POSTCSS: failed to transform "/css/eureka.css" (text/css). Check your PostCSS installation; install with "npm install postcss-cli". See [https://gohugo.io/hugo-pipes/postcss/:](https://gohugo.io/hugo-pipes/postcss/:) binary with name "npx" not found
```
意思好像是没有安装 npm，在 workflow 的文件中添加加载 npm 相关的设置([GitHub Actions for Hugo #Workflow for autoprefixer and postcss-cli](https://github.com/peaceiris/actions-hugo/tree/main))
``` yaml
  - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          # The action defaults to search for the dependency file (package-lock.json,
          # npm-shrinkwrap.json or yarn.lock) in the repository root, and uses its
          # hash as a part of the cache key.
          # https://github.com/actions/setup-node/blob/main/docs/advanced-usage.md#caching-packages-data
          cache-dependency-path: '**/package-lock.json'
          run: |
	          npm ci
	          hugo --minify --gc
```
再运行，还是失败，问题变成了
```
npm ERR! code EUSAGE
npm ERR! 
npm ERR! The `npm ci` command can only install with an existing package-lock.json or
npm ERR! npm-shrinkwrap.json with lockfileVersion >= 1. Run an install with npm@5 or
npm ERR! later to generate a package-lock.json file, then try again.
npm ERR! 
npm ERR! Clean install a project
npm ERR! 
npm ERR! Usage:
npm ERR! npm ci
npm ERR! 
npm ERR! Options:
npm ERR! [--install-strategy <hoisted|nested|shallow|linked>] [--legacy-bundling]
npm ERR! [--global-style] [--omit <dev|optional|peer> [--omit <dev|optional|peer> ...]]
npm ERR! [--include <prod|dev|optional|peer> [--include <prod|dev|optional|peer> ...]]
npm ERR! [--strict-peer-deps] [--foreground-scripts] [--ignore-scripts] [--no-audit]
npm ERR! [--no-bin-links] [--no-fund] [--dry-run]
npm ERR! [-w|--workspace <workspace-name> [-w|--workspace <workspace-name> ...]]
npm ERR! [-ws|--workspaces] [--include-workspace-root] [--install-links]
npm ERR! 
npm ERR! aliases: clean-install, ic, install-clean, isntall-clean
npm ERR! 
npm ERR! Run "npm help ci" for more info
```
这是因为没有找到 `package-lock.json` 文件，这个文件在 themes 的目录中（即`themes/hugo-eureka/package-lock.json`），拷贝到当前目录中就行了。
``` yaml
  - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          # The action defaults to search for the dependency file (package-lock.json,
          # npm-shrinkwrap.json or yarn.lock) in the repository root, and uses its
          # hash as a part of the cache key.
          # https://github.com/actions/setup-node/blob/main/docs/advanced-usage.md#caching-packages-data
          cache-dependency-path: '**/package-lock.json'
          run: |
	          cp themes/hugo-eureka/package*.json .
	          npm ci
	          hugo --minify --gc
```
至此问题解决。

## 总结
1. 先更新成最新版本的 hugo 尝试下能否解决问题。
2. 如果 1 不能解决问题，将配置文件 `config.yaml` 中的 `useResourceCacheWhen` 从 `always` 修改成 `fallback`

## 扩展
### 访问构建好的网站发现没有样式
按 `F12` 查看一下`.css`文件的路径就会发现，路径变成了`https://domian.com/domain.com/xxx/xxx.css`这个样子，这个好像跟 PostCSS 使用 `baseUrl` 做路径的方式有关，具体也不是很了解，**只要将 `baseUrl` 改为 `/` 就可以解决问题了**。

## 参考资料
[Can not access assets when a path is set in baseURL #186](https://github.com/wangchucheng/hugo-eureka/issues/186)   
[Hugo can not find resource cache when setting a path in baseURL #9787](https://github.com/gohugoio/hugo/issues/9787)   
[Error building site: POSTCSS](https://discourse.gohugo.io/t/error-building-site-postcss/31766)   
[GitHub Actions for Hugo ### Workflow for autoprefixer and postcss-cli](https://github.com/peaceiris/actions-hugo/tree/main)   
[failed to transform "css/eureka.css" #228](https://github.com/wangchucheng/hugo-eureka/issues/228)  