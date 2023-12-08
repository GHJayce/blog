---
title: Hugo Theme Stack图库是怎么工作的？怎样才能支持外链图片？
slug: static-site-generator/hugo/hugo-theme-stack-gallery-study
date: 2023-12-05T11:45:25+08:00
image: https://ghbjayce.github.io/asset/blog/95b65eb883dd0529MjAyMzEyMDUgMTUyMzI1.png
categories:
  - 源码阅读
tags:
  - Hugo
  - 静态站点生成器
  - hugo-theme-stack
  - TypeScript
---
## 阅读说明
为了方便文章描述，文中词汇的含义、未提及的场景如下：
- 内链图片：和站点同一个域名下的图片。
- 外链图片：和站点非同一个域名下的图片，通常以http开头的链接。
- 内联图片：文字和图片处于同一行。
- 图像组件：将图片处理成在页面中可以进行交互的组件。
- 图片交互效果：经过图像组件处理以后，在页面进行交互的效果。
- 文中的相对目录路径，如没有特殊说明，均以一个hugo项目为准，可以参考[我的项目结构](https://github.com/GHBJayce/ghbjayce.github.io)。

文字非常多，几乎没有图片，有干货和细节，请耐心阅读。

[我只想看改动小且有效的解决方案](#在原基础上支持)。

## 背景
在此之前使用了一段时间[CaiJimmy/hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)的主题，发现它只支持内链图片生成图像组件，并不支持外链图片，意味着外链图片没有办法在页面内点击图片进行一些交互。

比如我想放大某张外链图片，我得对着图片右键，然后在新的页面中打开，用浏览器的方式对这张图片进行放大，非常的不方便。

虽然瑕不遮瑜，但在外链需求特别多，又不想使用内链方式（不想放在项目中增加项目的大小）的情况下还是会很抓狂，所以在早些时候，尝试寻找了一些解决办法，于是找到了这个讨论：[引用于 cdn 的图片 没法生成图册吗？Discussion #659 · GitHub](https://github.com/CaiJimmy/hugo-theme-stack/discussions/659)。

看来不止我一个人有着同样的需求。

不巧的是，作者没有直接给出具体的解决办法，只是提供了解决思路，对hugo模板语法、工作机制不熟的我看的是一头雾水，提问的老哥看起来好像是解决了问题，怎么不分享一下啊，好东西不能藏着啊喂。

于是打算白嫖的发问一下，白嫖失败。

那么就跟着作者的提示，着手捣鼓一下：
- `layouts/_default/_markup/render-image.html`模板文件，[hugo markdown渲染的钩子之一](https://gohugo.io/templates/render-hooks/)。
- [resource.GetRemote相关语法文档](https://gohugo.io/hugo-pipes/introduction/#get-resource-with-resourcesget-and-resourcesgetremote)。

### 初步解决方案
就如我在讨论中提到的，我在`layouts/_default/_markup/render-image.html`钩子模板做了以下的修改：

```html
{{- $Permalink := .Destination | relURL | safeURL -}}
{{- $image := "" -}}
{{- if hasPrefix $Permalink "http" -}}
	{{- $image = resources.GetRemote $Permalink -}}
{{- else -}}
	{{- $image = .Page.Resources.GetMatch (printf "%s" (.Destination | safeURL)) -}}
{{- end -}}

{{- $alt := .PlainText | safeHTML -}}
{{- $Width := 0 -}}
{{- $Height := 0 -}}
{{- $Srcset := "" -}}

...
```

使用一段时间之后发现了以下的问题：
- Q1：外链的图片在启动服务(`hugo server`)的时候会将资源下载到`resources/_gen/images`目录下，然后观察渲染以后的img，链接的正是该目录下的图片，外链了个寂寞，还是会增加项目的大小 :(。
- Q2：如果遇到图片下载超时，会出现服务启动不了的问题。
- Q3：如果外链图片比较多，可能会导致启动服务缓慢。

> 这些问题的描述会有不恰当的地方，下面会有[说明](#作用)。

因为影响还是比较大，还不如不改呢，所以我去除了这些改动，暂时放下了这个糟心的问题。

### 再次捣鼓
正好最近有空，于是打算对hugo-theme-stack图库的实现研究一番，好好解决这个糟心的问题。

并将研究过程、当中发现到的一些细节记录下来，写下这篇文章。

## 研究
### 环境说明
- hugo v0.113.0
- hugo-theme-stack v3.21.0
- 图像库组件PhotoSwipe v4.1.3

### 分析
从头开始，要解决图像库支持外链图片，那么就要知道：
1. hugo是怎么解析markdown中的image语法的？
2. 用的是哪个图像组件？怎么做到图片处理成组件的？在什么时候做了介入处理？

在上文中我们知道，[CaiJimmy/hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)主题中的`layouts/_default/_markup/render-image.html`对markdown渲染的img标签做了介入处理。

为了确定使用的是哪个图像组件，看它渲染以后的img标签是怎样的，那就要做一个内链图片来观察下。

### 梳理过程
#### img 处理成图像组件
通过[Writing | Stack](https://stack.jimmycai.com/writing/markdown)和[Page bundles | Hugo](https://gohugo.io/content-management/page-bundles/)得知内链图片和文章要放在同级目录。

目录结构如下：
```
content/
├── posts
│   ├── my-post
│   │   ├── content1.md
│   │   ├── image1.jpg
│   │   ├── image2.jpg
│   │   └── index.md
|   └── _index.md
```

index.md内容如下：
```markdown
---
title: Test
description: something
date: 2023-12-07
slug: test
image: image2.jpg
---  

![img1](image1.jpg) ![img2](image2.jpg)
  

External images:

![img3](https://cn.bing.com/th?id=OHR.CheetahDay_EN-CN5641048727_1920x1080.webp&qlt=50) ![img4](https://cn.bing.com/th?id=OHR.ManateeMama_EN-US7376333243_UHD.jpg&pid=hp&w=1920&h=1080&rs=1&c=4)

```

> 你会发现content1.md在页面上看不到生成的这篇文章。
> 1. 当你将my-post里面的index.md改成content2.md的时候，content1.md和content2.md生成的文章都能够在页面中看到了，为什么？
> 2. 而如果在my-post里新建一个_index.md，content1.md和index.md生成的文章也可以在页面中看到了，为什么？
> 3. 然后1和2中content2.md和index.md中的内链图片都失效了，为什么？
> 
> 这不是本文的重点，避免在这上面浪费时间，**挖个坑，后续会写一篇文章专门介绍它**。

打开浏览器，我们看到内链图片和外链图片分别解析如下：

```html
<div class="gallery">
  <figure class="gallery-image" style="flex-grow: 66; flex-basis: 160px;">
    <a href="http://xxxx/p/test/image1.jpg" target="_blank">
      <img src="/p/test/image1.jpg" width="667" height="1000" loading="lazy"
      alt="img1" class="gallery-image" data-flex-grow="66" data-flex-basis="160px">
    </a>
    <figcaption>
      img1
    </figcaption>
  </figure>
  <figure class="gallery-image" style="flex-grow: 149; flex-basis: 359px;">
    <a href="http://xxxx/p/test/image2.jpg" target="_blank">
      <img src="/p/test/image2.jpg" width="1000" height="667" loading="lazy"
      alt="img2" class="gallery-image" data-flex-grow="149" data-flex-basis="359px">
    </a>
    <figcaption>
      img2
    </figcaption>
  </figure>
</div>


<img src="https://cn.bing.com/th?id=OHR.CheetahDay_EN-CN5641048727_1920x1080.webp&amp;qlt=50" loading="lazy" alt="img3">
<img src="https://cn.bing.com/th?id=OHR.ManateeMama_EN-US7376333243_UHD.jpg&amp;pid=hp&amp;w=1920&amp;h=1080&amp;rs=1&amp;c=4" loading="lazy" alt="img4">
```

通过在项目中搜索`gallery-image`，找到了`themes/hugo-theme-stack/assets/ts/gallery.ts`，一个TypeScript文件，从里面的代码看出使用的是[PhotoSwipe](https://github.com/dimsemenov/PhotoSwipe)图像组件。

接着就是`createGallery`函数，看了下里面的代码的作用，猜测90%的可能性就是**这里将img图片处理成图像组件的，也就是上面的HTML结构**。

#### 处理时机
继续，是`constructor(...)`里面调用了这个函数，PHPer表示很熟悉，是一个构造函数。

继续往上找，是`themes/hugo-theme-stack/assets/ts/main.ts`的`init()`方法中调用了上面的构造函数，在底部看到了`window.addEventListener('load')`调用了`init()`，这就是**将图片处理成图像组件的时机**。

紧接着找到了`themes/hugo-theme-stack/layouts/partials/footer/components/script.html`，这里引入了`main.ts`，结合了页面渲染以后的结构进行比对，实锤了。

> partials是hugo的模板结构目录，[相关文档](https://gohugo.io/templates/partials/)。

至此，猜测得到了证实，从markdown到img再到PhotoSwipe图像组件的实现过程，梳理完毕。

## 解决

### 分析 render-image.html

1. 模板语法、变量和函数代表的含义？
2. 做了哪些事情？

#### 源文件

以下是v3.21.0，`layouts/_default/_markup/render-image.html`的源码内容，贴上来方便查看。

[源文件仓库在这](https://github.com/CaiJimmy/hugo-theme-stack/blob/bdb9e7fc007a665fadb28055bee6e387cd68719b/layouts/_default/_markup/render-image.html)。

```html
{{- $image := .Page.Resources.GetMatch (printf "%s" (.Destination | safeURL)) -}}
{{- $Permalink := .Destination | relURL | safeURL -}}
{{- $alt := .PlainText | safeHTML -}}
{{- $Width := 0 -}}
{{- $Height := 0 -}}
{{- $Srcset := "" -}}

{{/* SVG and external images won't work with gallery layout, because their width and height attributes are unknown */}}
{{- $galleryImage := false -}}

{{- if $image -}}
	{{- $notSVG := ne (path.Ext .Destination) ".svg" -}}
	{{- $Permalink = $image.RelPermalink -}}

	{{- if $notSVG -}}
		{{- $Width = $image.Width -}}
		{{- $Height = $image.Height -}}
		{{- $galleryImage = true -}}

		{{- if (default true .Page.Site.Params.imageProcessing.content.enabled) -}}
			{{- $small := $image.Resize `480x` -}}
			{{- $big := $image.Resize `1024x` -}}
			{{- $Srcset = printf `%s 480w, %s 1024w` $small.RelPermalink $big.RelPermalink -}}
		{{- end -}}
	{{- end -}}
{{- end -}}

<img src="{{ $Permalink }}"
	{{ with $Width }}width="{{ . }}"{{ end }}
	{{ with $Height }}height="{{ . }}"{{ end }}
	{{ with $Srcset }}srcset="{{ . }}"{{ end }}
	loading="lazy"
	{{ with $alt }}
		alt="{{ . }}"
	{{ end }}
	{{ if $galleryImage }}
		class="gallery-image" 
		data-flex-grow="{{ div (mul $image.Width 100) $image.Height }}"
		data-flex-basis="{{ div (mul $image.Width 240) $image.Height }}px"
	{{ end }}
>
```

#### 语法

1. `{{- -}}`和`{{ }}`的写法有什么不同？
	- 以if为例
		- `{{- if ... -}}`：在该语法中，横杠 `-` 的作用是去除 `if` 语句前后的空白字符（包括换行符、空格等），以保持输出结果更加紧凑。
		- `{{ if ... }}`：在标准的 `if` 语句语法中，并没有 `-` 符号。这意味着在渲染过程中，会保留 `if` 语句前后的空白字符。
2. `.Page.Resources.GetMatch`有什么用？
	- [.Page.Resources](https://gohugo.io/methods/page/resources/) 是返回页面资源集合。
	- [resource.GetMatch](https://gohugo.io/functions/resources/getmatch/) 简单点说就是寻找某个匹配规则的资源文件。
3. [.Destination](https://gohugo.io/templates/render-hooks/#image-markdown-example) 获取`[](xxx)`中的xxx链接。
4. `|` 管道操作符。
5. [safeURL](https://gohugo.io/functions/safe/url/) 将字符串处理成安全的URL字符串。
6. [relURL](https://gohugo.io/functions/urls/relurl/) 返回相对URL，例如`image1.jpg`得到`/p/test/image1.jpg`。
7. [.PlainText](https://gohugo.io/templates/render-hooks/#context-passed-to-render-link-and-render-image) 获取`[](xxx "text")`中的text。
8. [safeHTML](https://gohugo.io/functions/safe/html/) 将字符串处理成安全的HTML字符串。
9. `ne` 比较两个值是否相等。
10. [path.Ext](https://gohugo.io/functions/path/ext/) 获取后缀，例如`/p/test/image1.jpg`得到`image1.jpg`。
11. `.Page.Site.Params.imageProcessing.content.enabled`获取`hugo.xxx`中的配置项，[.Page.Site.Params](https://gohugo.io/methods/site/params/)。
12. [default](https://gohugo.io/functions/compare/default/) 第二个参数未设置时，使用第一个参数。
13. [with](https://gohugo.io/functions/go-template/with/) 表达式为true时进入代码块，可以使用`.`代替表达式。
14. [div](https://gohugo.io/functions/math/div/) 将一个数字除以一个或多个数字。
15. [mul](https://gohugo.io/functions/math/mul/) 将多个数字进行相乘返回积。

#### 作用
结合以上，`render-image.html`主要做了以下几件事：
- E1：从页面资源集合中找到名称一致的图片。
- E2：如果`hugo.yaml`文件中`imageProcessing.content.enabled`的配置开启了，[Resize](https://gohugo.io/methods/resource/resize/)会对这张图片进行裁剪，裁出480宽、1024宽的两张图片并放到了`resources/_gen/images/post/your_post_slug_name/`目录下。
- E3：`$galleryImage`是能否生成图像库组件的关键，它为`img`添加了关键的属性。

这里需要解释和纠正一下[初步解决方案](#初步解决方案)中，我对Q1的描述：
- E2回答了为什么`resources/_gen/images/`目录下会有图片的原因。
- `hugo server`启动的时候因为`resources.GetRemote`的关系，它会去访问这张图片，而且在首次访问成功后会对这个图片进行缓存（但至于缓存在哪里，我也想知道，如果你知道的话请告诉我），因为在首次之后再执行`hugo server`会非常快，得到证实。
- 很抱歉我在早期的解决反馈中做了一些过于草率的结论。

### 解决方案
#### 在原基础上支持
我在源文件的基础上，增加了`resources.GetRemote`获取外链图片的部分，并且在`hugo.yaml`中增加了一个`params.render.image.extranelLink.enabled`开关进行控制，是否启用外链图片的逻辑。

直接给出我的改动：

1、`layouts/_default/_markup/render-image.html`

```html
{{- $Permalink := .Destination | relURL | safeURL -}}
{{- $image := "" -}}

{{- if and (hasPrefix $Permalink "http") (default false .Page.Site.Params.render.image.extranelLink.enabled) -}}
	{{- with resources.GetRemote $Permalink -}}
		{{- with .Err -}}
			{{- $warnMsg := printf "%s" $Permalink -}}
			{{- warnf "%s.\nPlease check the link: %s\n" . $warnMsg -}}
		{{- else -}}
			{{- $image = . -}}
		{{- end -}}
	{{- else -}}
	  {{- errorf "Unable to get remote resource %q, %s" $Permalink .Position -}}
	{{- end -}}
{{- else -}}
	{{- $image = .Page.Resources.GetMatch (printf "%s" (.Destination | safeURL)) -}}
	{{- if $image -}}
		{{- $Permalink = $image.RelPermalink -}}
	{{- end -}}
{{- end -}}
{{- $alt := .PlainText | safeHTML -}}
{{- $Width := 0 -}}
{{- $Height := 0 -}}
{{- $Srcset := "" -}}

{{/* SVG and external images won't work with gallery layout, because their width and height attributes are unknown */}}
{{- $galleryImage := false -}}

{{- if $image -}}
	{{- $notSVG := ne (path.Ext .Destination) ".svg" -}}

	{{- if $notSVG -}}
		{{- $Width = $image.Width -}}
		{{- $Height = $image.Height -}}
		{{- $galleryImage = true -}}

		{{- if (default true .Page.Site.Params.imageProcessing.content.enabled) -}}
			{{- $small := $image.Resize `480x` -}}
			{{- $big := $image.Resize `1024x` -}}
			{{- $Srcset = printf `%s 480w, %s 1024w` $small.RelPermalink $big.RelPermalink -}}
		{{- end -}}
	{{- end -}}
{{- end -}}

<img src="{{ $Permalink }}"
	{{ with $Width }}width="{{ . }}"{{ end }}
	{{ with $Height }}height="{{ . }}"{{ end }}
	{{ with $Srcset }}srcset="{{ . }}"{{ end }}
	loading="lazy"
	{{ with $alt }}
		alt="{{ . }}"
	{{ end }}
	{{ if $galleryImage }}
		class="gallery-image" 
		data-flex-grow="{{ div (mul $image.Width 100) $image.Height }}"
		data-flex-basis="{{ div (mul $image.Width 240) $image.Height }}px"
	{{ end }}
>
```

2、`hugo.yaml` 以下只显示改动的地方
```yaml
params:
	imageProcessing:
		content:
			enabled: false
	render:
		image:
			extranelLink:
				enabled: true
```

##### 效果
经过我几天简单测试和使用下来，在文章内使用内链图片和外链图片，均能够生成图像库组件的交互效果。

来看外链图片效果：

![img1](https://ghbjayce.github.io/asset/blog/593a79ef8bb8fcfeMjAyMzEyMDggMTIyMjE3.jpeg) ![img2](https://ghbjayce.github.io/asset/blog/bdebad2e7a5ec61eMjAyMzEyMDggMTIyMjM2.jpeg) ![img3](https://ghbjayce.github.io/asset/blog/7db5aca65803cb87MjAyMzEyMDggMTIyMjU0.jpeg)

##### 发现的问题
1、问题NQ1：

如果你发现图片并没有生成组件，可以检查下markdown语法里图片是不是完全独占一行的，如果不是，把它改成独占一行再去页面查看，出来了对不对。

这是因为`themes/hugo-theme-stack/assets/ts/gallery.ts`中`createGallery`函数里有这么一段代码：

```ts
let isNewLineImage = paragraph.classList.contains('no-text');
if (!isNewLineImage) continue;
```

结合注释和它解析之后的HTML结构得知，它不支持内链图片生成图像组件（无论是内链图片还是外链图片），因为这会影响到内联图片的效果。

这个问题在我改动之前就已经存在了，毕竟解析之后的结构和内联图片效果肯定是冲突的，很遗憾内联图片不支持图像组件的交互效果。

---

2、问题NQ2：

如果外链图片比较多的情况下，`hugo server`启动的时候确实会有时间性能的损耗，毕竟它要对每一张图片都进行访问。

**如果你的外链图片访问足够快的话，不存在任何问题。**

如果你像我一样外链图片访问可能会存在缓慢的情况下，那么在启动的时候可能会遇到超时的问题，导致启动失败，错误内容如下：

```bash
ERROR 2023/12/05 21:17:31 render of "page" failed: "/project/themes/hugo-theme-stack/layouts/_default/baseof.html:4:12": execute of template failed: template: _default/single.html:4:12: executing "_default/single.html" at <partial "head/head.html" .>: error calling partial: partial "head/head.html" timed out after 30s. This is most likely due to infinite recursion. If this is just a slow template, you can try to increase the 'timeout' config setting.
```

有三种解决办法：
1. 解决掉外链图片访问慢的问题。
2. 在平常写文章的时候，把`params.render.image.extranelLink.enabled`关闭，没错外链图片没有交互效果了，但你只是想写文章而已，写完以后可以再打开。
3. 不解决，它只会影响到启动的时候而已，因为访问图片的目的只是拿到图片的宽高并且加到img属性中，你大可以挑一个网络顺畅的时刻去做这件事，而且在此之后再启动时它会使用缓存，影响很小，且不会对构建以后有影响，顶多没有图片交互效果而已。

---

总结一下：
- NQ1：内联图片不支持图片交互效果，这在我改动之前就已经存在的问题。
- NQ2：`hugo server`启动时会有影响，但外链图片访问足够快的情况下，不成问题。
	- 平时写文章不用时可以关闭，需要用时再打开。
	- 解决掉图片访问缓慢的问题。
	- 不要忘了它在下次启动的时候会有缓存处理。

#### 更换组件
这个我就不展开了，因为解决方案一已经能够满足我的需求了，除非你想内联图片也需要图片交互效果，并且能够彻底解决掉启动缓慢的问题。

留到下次有这个需求时我再回来填坑。

> 届时我将会采用[fengyuanchen/viewerjs](https://github.com/fengyuanchen/viewerjs)的组件。

## 致谢
能看完这篇文章不容易，感谢你的耐心。