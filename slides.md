---
layout: 'cover'
theme: default
---

# slides制作 - slidev

---

# Before Slidev

- 由于我的主力知识库软件是Notion,而制作slides时往往需要使用知识库中的内容,我过去的制作流程有两种:
    1. 基于PowerPoint
        1. 从Notion上逐block复制内容到PowerPoint,并为这些内容:
        2. 恢复原有的视觉效果,如代码块效果
        3. 调整尺寸
        4. 逐页进行排版
    2. 基于Notion
        1. 将一份完整Notion原稿的内容分为多个页面
        2. 调整每个页面的布局
        3. 将这些页面组织起来,方法包括:
            1. 为页与页之间添加”上一页”,”下一页”链接
            2. 对每个页面进行截图,并将这些图片导入到PowerPoint产生一份纯图ppt

---

# Before Slidev

- 流程1的问题在于
    - PowerPoint不是我熟悉的软件
    - 大量的复制粘贴工作
    - 富文本(代码块,LaTeX,图片,网页embed)要素的重建很繁琐
    - PowerPoint中,代码块和LaTeX的展示不尽人意
    - PowerPoint中,形式和内容不是分离的,放大了排版强迫症的危害
    - .ppt的版本控制也让人恼火
- 流程2的问题在于
    - 当内容总量能够适配一页时,文字总是灾难性地太小,迫使我使用h3|h2来编写正文内容
    - Notion页面间是网状关系,通过”上一页”,”下一页”构造出的线性关系非常生硬
    - Notion无法支持一些展示时常用的功能,如演讲者视图,也无法支持动画

---

# Why Slidev?

## 作者提到的

- 让开发者通过熟悉的技术生产slides
- 避免排版强迫症
- 对代码片段的支持,包括语法高亮(对几乎所有语言),以及通过Monaco编辑器进行现场编码
- 由于基于Web技术,可以被在线部署

## 我关心的

- 从markdown生成 & 基于web,这可以很好地地与notion结合,避免编写重复内容
- 更好的版本控制
- 同时,Slidev的功能已经足够覆盖我对PPT的核心需求
    - 导出PDF - 用于提交报告,归档
    - 部署到在线站点 - 用于分享,尤其是通过GitHub Pages
    - 演讲者视图,camera和recording - 用于在线分享,尤其是学术会议,技术论坛

---

# installation

- 首先,下载和安装稳定版本的node.js,你将获得`npm`命令
    
    [https://nodejs.org/dist/v18.18.1/node-v18.18.1.tar.xz](https://nodejs.org/dist/v18.18.1/node-v18.18.1.tar.xz)
    
- 然后,你可以通过两种方式开始使用Slidev
    - 从模板开始
        - 通过`npm init slidev@latest`获得官方模板
        - 通过修改模板中的内容来产生自己的slides
    - 全局安装
        - 通过`npm i -g @slidev/cli`安装slidev/cli,你将获得`slidev`命令
        - 之后,在任何一个包含markdown(.md)文件的目录下,可以使用下面的命令产生slides
            - `slidev [entry]` - 启动本地服务器,在特定端口上展示从entry生成的slides
            - `slidev build [entry]` - 建立可托管的单页应用(single page application, SPA)
            - `slidev export [entry]` - 将slides导出为PDF或其它格式

---
layout: 'cover'
---

# 制作slides

---

- 一个典型的”Slidev工程”目录结构如下

```
project
│   slides.md
└───public
│   │   image_0.png
│   │   ...
│   │   image_n.png
│   │
│   └───subfolder1
│       │   file111.txt
│       │   file112.txt
│       │   ...
│
└───folder2
    │   file021.txt
    │   file022.txt

```

---

## 基本排版要素

### 分页和布局

- Slidev使用`---`进行分页,在每个页面之前,可以插入[扉页块 (front matter)](https://jekyllrb.com/docs/front-matter/),为后续页面指定布局和其它元数据
- 一个典型的扉页块如下,它使用[YAML](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started)格式
    
    ```yaml
    ---
    layout: 'center' # 布局类型
    background: './images/background-1.png' # 背景图片
    class: 'text-white'
    ---
    ```
    
- 与PowerPoint类似地,Slidev内建了多种布局类型,详见[Layouts | Slidev](https://sli.dev/builtin/layouts.html#end)
- 一定程度上,布局(layout)不是一个准确的称呼,因为一些不同的布局的差异并不在于内容的分区排布,而在于内容的样式,如cover和quote
- 从内容排布的角度,Slidev主要分为单栏和双栏布局,相比于PowerPoint无疑要更加贫瘠,但:
    - 对我来说,这反而能够减少排版强迫症带来的焦虑
    - 对于熟悉Web技术的开发者,它们可以通过Slidev渲染复杂的自定义布局

## 基本内容要素

- Slidev支持各种markdown基本要素,并为部分要素进行了增强

---
layout: two-cols-header
---

### 代码块

- 相比markdown,Slidev为代码块额外提供了语法高亮(基于Prism|Shiki)和在线编辑(基于Monaco)功能
- 我们在页面左侧提供了一个带有动画的代码块,在页面右侧提供了一个可以在线编辑的代码块,两者均使用Scala语法高亮

::left::

<v-clicks>

```markdown
```scala{1|2,3|all}
case class FlopocoDiv(
    exponentSize: Int,
    mantissaSize: Int,
    override val family: XilinxDeviceFamily,
    override val targetFrequency: HertzNumber
) extends BlackBox{}
```
```

```scala{1|2,3|all}
case class FlopocoDiv(
    exponentSize: Int,
    mantissaSize: Int,
    override val family: XilinxDeviceFamily,
    override val targetFrequency: HertzNumber
) extends BlackBox{}
```

</v-clicks>

::right::

<v-clicks>

```markdown
```scala{xxx} xxx应当替换为monaco
case class FlopocoDiv(
    exponentSize: Int,
    mantissaSize: Int,
    override val family: XilinxDeviceFamily,
    override val targetFrequency: HertzNumber
) extends BlackBox{}
```
```

```scala{monaco}
case class FlopocoDiv(
    exponentSize: Int,
    mantissaSize: Int,
    override val family: XilinxDeviceFamily,
    override val targetFrequency: HertzNumber
) extends BlackBox{}
```

</v-clicks>

---

### 静态资源

- 与markdown一样,你可以使用本地或远程的图片
    - 远程图片
        - `![Remote Image](https://sli.dev/favicon.png)`
    - 本地图片,你需要将图片放到`public`文件夹中
        - `![Local Image](/pic.png)`
    - 如果你想使用自定义的尺寸或样式，可以使用 `<img>` 标签
        - `<img src="/pic.png" class="m-40 h-40 rounded shadow" />`

![Local Image](/chainsaw.png)

---

### 备注

- 每张slide的最后一个注释块将被视为备注,在演讲者视图中被展示

<!-- 这是一条备注 -->

---

### 图标

- Slidev 允许你在Markdown中**直接**访问几乎所有的开源的图标集(基于`[vite-plugin-icons](https://github.com/antfu/vite-plugin-icons)`和[Iconify](https://iconify.design/))
- 图标 ID 遵循 [Iconify](https://iconify.design/) 的命名规则 `{collection-name}-{icon-name}`。例如：
    - 使用 [Material Design Icons](https://github.com/Templarian/MaterialDesign)，其规则为 `<mdi-account-circle />` -<mdi-account-circle />
    - 使用 [Carbon](https://github.com/carbon-design-system/carbon/tree/main/packages/icons)，其规则为 `<carbon-badge />` - <carbon-badge />
    - 使用 [Unicons Monochrome](https://github.com/Iconscout/unicons)，其规则为 `<uim-rocket />` - <uim-rocket />
    - 使用 [Twemoji](https://github.com/twitter/twemoji)，其规则为 `<twemoji-cat-with-tears-of-joy />` - <twemoji-cat-with-tears-of-joy />
    - 使用 [SVG Logos](https://github.com/gilbarbara/logos)，其规则为 `<logos-vue />` - <logos-vue />

---

### LaTex

- 与Notion一样,Slidev基于KaTeX

---

# 与Notion结合的工作流

## 生产内容

## 转换为slides

## 展示

## 存档

## 部署到GitHub Pages

# 已知的bugs

- 下面的,出现在文档中演示分页的代码无法被正确地分页

```
---
layout: cover
---# Slidev

This is the cover page.

---
layout: center
background: './images/background-1.png'
class: 'text-white'
---

# Page 2

This is a page with the layout `center` and a background image.

---

# Page 3

This is a default page without any additional metadata.
```