+++
title = "使用Hugo+papermod+giscus创建你的个人博客"
date = "2026-02-06T18:48:09+08:00"

categories = ["其它学习"]
tags = ["博客创建", "笔记"]

+++

## 前言

本博客旨在记录搭建个人博客的全部过程，用了以下技术

- Hugo是一个静态网站生成器，简单来说就是将Markdown文件，根据主题的不同，将Markdown文件中的不同内容放置到主题中的不同位置，再通过提前设计的主题中的CSS或者JS文件进行渲染，得到了HTML文件，供给浏览器使用
- Papermod是一种常用的博客主题，其中包括多个可直接复用的HTML模板，模板内容包括404、博客、主页等，同时HTML模板还有设计好的配套的渲染
- Giscus是一个基于Github discussion的开源评论系统，只要将个人博客项目放到Github上进行存储，就能通过该系统为博客设计评论系统，而评论的来源则是Github中项目的issue这种讨论部分

我的博客参考了以下博客进行配置：

[Hugo + PaperMod 从零搭建教程](https://suxilan.github.io/notes/start-hugo-papermod/)

[折腾 Hugo PaperMod 主题 - 她和她的猫](https://her-cat.com/posts/2025/10/08/hugo-paper-mod/)

[给Hugo PaperMod增加giscus评论系统 | 老刘博客](https://iliu.org/posts/add-comment-system-to-hugopapermo/)

## Hugo + PaperMod搭建博客

### 1.安装必要工具

#### 1.1 安装Hugo

去这个网站中[Releases · gohugoio/hugo](https://github.com/gohugoio/hugo/releases)找到和自己电脑符合的最新版本，我下载的是hugo_extended_0.155.1_windows-amd64.zip。下载后解压该zip文件，并将该文件解压所在文件夹添加到系统环境变量中即可：

![image-20260224161519355](/images/image-20260224161519355.png)

做完这一切后，使用如下命令：

```powershell
hugo version
```

进行测试，得到版本信息即安装成功

#### 1.2 安装Git

参考网站[Git 详细安装教程（详解 Git 安装过程的每一个步骤）_git安装-CSDN博客](https://blog.csdn.net/mukes/article/details/115693833)

安装完成后，使用如下命令进行配置：

```powershell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

### 2. 创建Hugo站点

Hugo站点即一个自己的个人博客的设置内容，使用如下命令进行创建：

```
cd D:/hugo
# 因为这里采用的是github进行博客内容的存储和评论，因此名字直接叫这个
hugo new site HAutumnMaple.github.io
cd HAutumnMaple.github.io
```

创建成功后，文件夹中就有了一个基础的项目结构：

![QQ20260224-171852](/images/QQ20260224-171852.png)

其中：

```
HAutumnMaple.github.io/
├── archetypes/       # 文章模板，即创建文章时默认信息
├── assets/           # 静态资源（CSS、JS等），可后续添加内容设计个性化博客
├── content/          # 文章目录，使用命令创建的文章都将放置在这里
├── data/             # 数据文件
├── i18n/             # 多语言文件
├── layouts/          # 自定义布局，可以添加一些html进行个性化设置
├── static/           # 静态文件（图片等）
├── themes/           # 主题目录
└── hugo.yaml         # 配置文件
```

### 3. 配置PaperMod主题

#### 3.1 初始化 Git 仓库

```powershell
git init
```

#### 3.2 添加主题为 Git Submodule

使用submodule，方便更新：

```powershell
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

或者直接克隆：

```powershell
git clone https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1
```

#### 3.3 未来更新Submodule

```powershell
git submodule update --remote --merge
```

### 4. 配置hugo.yaml

因为个人看yaml文件习惯，因此直接将toml文件替换成yaml文件，我的配置信息如下：

```yaml
baseURL: "https://HAutumnMaple.github.io/"
languageCode: "zh-cn"
title: "HAutumnMaple的博客"
theme: "PaperMod"

enableRobotsTXT: true
enableEmoji: true
hasCJKLanguage: true
disableHLJS: false

pagination:
  pagerSize: 10

languages:
  zh:
    languageName: "中文"
    weight: 1

# 为了统一管理，这里我设计了两个文章分类
taxonomies:
  category: "categories"
  tag: "tags"

outputs:
  home:
    - "HTML"
    - "RSS"
    - "JSON"

# 这里是个人设计的用于添加新的页面，都可以自己设计，我这里直接能用默认就用默认了
menu:
  main:
    - name: "首页"
      url: "/"
      weight: 1
    - name: "归档"
      url: "/archives/"
      weight: 2
    - name: "分类"
      url: "/categories/"
      weight: 3
    - name: "标签"
      url: "/tags/"
      weight: 4
    - name: "关于"
      url: "/about/"
      weight: 5
    - name: "搜索"
      url: "/search/"
      weight: 6

markup:
  highlight:
    noClasses: false
    lineNos: true
    lineNumbersInTable: true
    tabWidth: 4

params:
  author: "HAutumnMaple"
  description: "古之立大事者，不惟有超世之才，亦必有坚韧不拔之辈。#好心情持续营业 坏心情已经打烊#"

  defaultTheme: "auto"
  ShowReadingTime: true
  ShowCodeCopyButtons: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: true
  ShowShareButtons: true
  ShowWordCount: true
  hideSummary: true	# 因为我个人不爱写摘要，如果爱写摘要的同志就把这个关掉即可

  comments: true
  commentsDefault: true

  homeInfoParams:
    Title: "HAutumnMaple"
    Content: |
      你好，我是 HAutumnMaple 👋  
      一名计算机相关专业学习者，
      欢迎来到我的博客
      我将在这里记录我的生活

  socialIcons:
    - name: "github"
      url: "https://github.com/HAutumnMaple"
      title: "访问我的 GitHub"

    - name: "email"
      url: "mailto:huqidong25@mails.ucas.ac.cn"
      title: "发送邮件给我"
```

这里我添加了一些所需要的页面如搜索、关于、归档等，都是使用在Papermod的默认模板（在相对路径.\themes\PaperMod\layouts\_default中），使用如下命令进行生成：

```powershell
hugo new search.md
```

然后在about.md文件中配置如下内容：

```markdown
---
title: "搜索"
layout: "search"
url: "/search/"

---
```

layout选择默认模板中html的名称，url则与yaml配置文件中内容相匹配即可。

### 5. 验证博客建立是否成功

现在博客的80%的工作就已经完成了，现在可以使用如下命令创建自己的第一篇文章并且验证：

```powershell
hugo new posts/hello.md
hugo server -D
```

访问命令行给出的具体网址即可验证，一般都在localhost:1313上

## 基于Giscus构建博客的评论系统

因为看到大部分的个人博客都有评论系统，因此心血来潮还是决定弄一个

### 1. 创建Github项目，并且把博客内容上传

创建一个公开的Github项目

```powershell
git add .
git commit -m "construct project"
git remote add origin xxx # 你创建的项目名称
git push origin main
```

### 2. 在Github上使用Giscus进行配置

在项目中找到setting设置，然后勾选，如下图所示

![image-20260224181204249](/images/QQ20260224-181235.png)

然后使用该网站[GitHub Apps - giscus](https://github.com/apps/giscus)进行下载并且选择该仓库进行配置：

![QQ20260224-190018](/images/QQ20260224-190018.png)

随后点击该链接[giscus](https://giscus.app/zh-CN)进行配置，根据推荐配置得到配置代码：

```html
<script src="https://giscus.app/client.js"
        data-repo="[在此输入仓库]"
        data-repo-id="[在此输入仓库 ID]"
        data-category="[在此输入分类名]"
        data-category-id="[在此输入分类 ID]"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="zh-CN"
        crossorigin="anonymous"
        async>
</script>
```

复制该代码，在文件夹./layout/partials/下新建一个文件comments.html，然后把该代码粘贴进去保存，并且在hugo.yaml中设置comments： true即可，相当于在每个文章的HTML文件中附加了该评论区进行生成。

## 优化阅读体验

在我使用PaperMod主题进行博客撰写和阅读的时候，我发现默认的文字、代码块的渲染都差强人意。因此，我这里在网络上找了下别人的个性化设置，直接搬过来用了，感谢各位大佬的开源支持

 ### 1. 字体优化

创建download-fonts.py脚本，自动下载字体并生成本地 CSS：

```python
#!/usr/bin/env python3
"""
下载 Google Fonts 到本地目录
使用方法: python3 download-fonts.py
"""

import os
import re
import requests
from pathlib import Path
from urllib.parse import urlparse

# 配置
GOOGLE_FONTS_URL = "https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap"
STATIC_DIR = Path("static")
FONTS_DIR = STATIC_DIR / "fonts"  # 字体文件放在 static 目录，Hugo 会自动复制
CSS_DIR = Path("assets") / "css" / "extended"  # CSS 放在 assets 目录
OUTPUT_CSS = CSS_DIR / "fonts.css"

# 创建必要的目录
FONTS_DIR.mkdir(parents=True, exist_ok=True)
CSS_DIR.mkdir(parents=True, exist_ok=True)

def download_file(url, dest_path):
    """下载文件到指定路径"""
    print(f"下载: {url}")
    response = requests.get(url, timeout=30)
    response.raise_for_status()
    
    with open(dest_path, 'wb') as f:
        f.write(response.content)
    print(f"保存到: {dest_path}")
    return dest_path

def get_google_fonts_css():
    """获取 Google Fonts CSS"""
    print(f"获取 Google Fonts CSS: {GOOGLE_FONTS_URL}")
    
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
    }
    
    response = requests.get(GOOGLE_FONTS_URL, headers=headers, timeout=30)
    response.raise_for_status()
    
    return response.text

def extract_font_urls(css_content):
    """从 CSS 中提取字体文件 URL"""
    pattern = r'url\((https://[^)]+)\)'
    urls = re.findall(pattern, css_content)
    return urls

def download_fonts(css_content):
    """下载所有字体文件并替换 CSS 中的 URL"""
    font_urls = extract_font_urls(css_content)
    
    if not font_urls:
        print("❌ 未找到字体文件 URL")
        return css_content
    
    print(f"找到 {len(font_urls)} 个字体文件")
    
    for idx, url in enumerate(font_urls, 1):
        parsed = urlparse(url)
        url_path = parsed.path
        ext = os.path.splitext(url_path)[1] or '.woff2'
        
        filename = f"inter-{idx}{ext}"
        local_path = FONTS_DIR / filename
        
        try:
            download_file(url, local_path)
            relative_url = f"/fonts/{filename}"
            css_content = css_content.replace(url, relative_url)
        except Exception as e:
            print(f"❌ 下载失败: {url}")
            print(f"   错误: {e}")
    
    return css_content

def generate_local_css():
    """生成本地字体 CSS 文件"""
    print("\n" + "="*50)
    print("开始下载 Google Fonts")
    print("="*50 + "\n")
    
    try:
        css_content = get_google_fonts_css()
        print(f"✅ 成功获取 CSS (长度: {len(css_content)} 字节)\n")
        
        local_css = download_fonts(css_content)
        
        header = """/* 
 * Google Fonts - Inter
 * 本地托管版本，自动生成于 download-fonts.py
 */

"""
        local_css = header + local_css
        
        with open(OUTPUT_CSS, 'w', encoding='utf-8') as f:
            f.write(local_css)
        
        print(f"\n✅ CSS 文件已保存到: {OUTPUT_CSS}")
        print(f"✅ 字体文件已保存到: {FONTS_DIR}/")
        print("\n重新构建网站: hugo --gc --minify\n")
        
    except Exception as e:
        print(f"\n❌ 错误: {e}")
        import traceback
        traceback.print_exc()

if __name__ == "__main__":
    try:
        import requests
    except ImportError:
        print("❌ 请先安装 requests 库: pip install requests")
        exit(1)
    
    generate_local_css()
```

使用以下命令，运行该代码：

```powershell
# 1. 安装依赖
pip install requests

# 2. 运行脚本
python3 download-fonts.py
```

脚本会自动：

- 下载字体文件到 `static/fonts/` 目录
- 生成本地 CSS 到 `assets/css/extended/fonts.css`
- Hugo 会自动加载 `extended` 目录中的 CSS

然后在文件夹./layouts/partials下创建extend_head.html，添加以下内容即可：

```html
<!-- 预加载关键字体文件，提升首屏渲染速度 -->
<link rel="preload" href="/fonts/inter-7.woff2" as="font" type="font/woff2" crossorigin>
```

### 2. 优化文章排版、代码块、表格等元素的显示效果

创建 assets/css/extended/reading.css 文件，定义详细的样式规则来优化文章排版、代码块、表格等元素的显示效果：

```css
/* === 1. CSS 变量定义 === */
:root {
    /* 颜色 */
    --primary: #1a1b1c;
    --content: #333435;
    --secondary: #666;
    --sec-color: #f2f3f4;
    --link-color: #2d8cdc;
    --code-bg: #f5f5f5;
    --sec-note-color: #6e6e6e;
    
    /* 字体 */
    --font-fallback: -apple-system, BlinkMacSystemFont, system-ui, sans-serif, 'Color Emoji';
    --font-family: 'Inter', var(--font-fallback);
    --code-font-family: 'Fira Code', Menlo, 'Lucida Console', 'DejaVu Sans Mono', var(--font-fallback);
}

/* 暗色模式 */
.dark {
    --primary: #f2f2f2;
    --content: #e3e3e3;
    --sec-color: #2A2C2B;
    --sec-note-color: #808080;
}

/* === 2. 全局字体设置 === */
body {
    font-family: var(--font-family);
    font-size: 18px;
    margin: 0;
}

/* 标题字重 */
h1, h2, h3, h4, h5, h6 {
    font-weight: 700;
}

/* 代码字体 */
.post-content code, 
.post-content code span {
    font-family: var(--code-font-family);
}

/* === 3. 文章标题样式 === */
.post-title {
    font-size: 34px;
    margin: 8px 0;
}

.post-content h1,
.post-content h2,
.post-content h3,
.post-content h4,
.post-content h5,
.post-content h6 {
    margin-bottom: 18px;
    font-weight: 600;
}

.post-content h1 {
    margin-top: 48px;
    padding-bottom: 13px;
    border-bottom: 1px solid var(--sec-color);
}

.post-content h2 {
    font-size: 24px;
    margin-top: 48px;
    padding-bottom: 13px;
    border-bottom: 1px solid var(--sec-color);
}

.post-content h3 {
    font-size: 22px;
    margin-top: 32px;
}

.post-content h4 {
    font-size: 20px;
    margin-top: 23px;
}

.post-content h5 {
    font-size: 16px;
    margin-top: 18px;
}

.post-content h6 {
    font-size: 14px;
    margin-top: 16px;
}

/* === 4. 正文样式 === */
.post-content {
    line-height: 1.86;
}

.post-content p,
.post-content blockquote,
.post-content figure,
.post-content table {
    margin: 18px 0;
}

.post-content blockquote {
    color: var(--sec-note-color);
}

.post-content hr {
    margin: 64px 128px;
}

.post-content ul,
.post-content ol,
.post-content dl,
.post-content li {
    margin: 8px 0;
}

/* === 5. 链接样式 === */
.post-content a {
    color: var(--link-color);
    box-shadow: none;
    text-decoration: none;
}

.post-content a:hover {
    text-decoration: underline;
}

/* === 6. 行内代码样式 === */
.post-content code {
    margin: unset;
    padding: 5px 7px;
    border-radius: 8px;
}

/* === 6.5. 折叠块样式 === */
.post-content details summary {
    cursor: zoom-in;
    user-select: none;
}

.post-content details[open] summary {
    cursor: zoom-out;
}

/* === 7. 图片样式 === */
.post-content img {
    margin: auto;
    max-width: 100%;
    height: auto;
    transition: opacity 0.3s ease;
}

.post-content figure {
    margin: 27px 0;
    text-align: center;
}

.post-content figure img {
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.post-content figure img:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}

.post-content figcaption {
    margin-top: 9px;
    font-size: 16px;
    color: var(--secondary);
    font-style: italic;
}

/* === 8. 移动端响应式优化 === */
@media (max-width: 768px) {
    .post-content img {
        border-radius: 4px;
    }
    
    .post-content figure img:hover {
        transform: none;
    }
}

/* === 9. 防止滚动条导致页面抖动 === */
html {
    overflow-y: scroll;
}

:root {
    overflow-y: auto;
    overflow-x: hidden;
}

:root body {
    position: absolute;
    width: 100vw;
    overflow: hidden;
}
```

### 3. 图片显示问题

在后续写文章的过程中，发现会存在需要使用图片的情况，因此这里再补充一个在线博客渲染图片的方法：

```powershell
# 创建图片目录，存放图片到该目录中，在写文章时候需要使用图片：![图片描述](/images/photo.jpg)
mkdir static\images
```



## 换电脑后，重新写博客的启动命令

因为考虑到会存在换电脑的情况，因此这里我直接把换电脑继续写博客的命令补充在这里：

```powershell
# 安装git
# 安装hugo
git clone https://github.com/你的用户名/仓库.git
cd 仓库名
git submodule update --init --recursive
hugo server -D
```

