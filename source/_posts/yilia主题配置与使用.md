---
title: yilia主题配置与使用
top: 0
categories:
- hexo
- 工具
essayending: true
declare: true
valineenbale: true
toc: true
reward: true
abbrlink: c8eb2a06
date: 2019-12-30 22:44:00
tags: [hexo, 教程]
---

这是一份基于本博客（yilia 主题，经过多轮自改）的**完整使用手册**。我会把每一项配置、每一个功能「在哪改、怎么改」都写清楚，避免你照着过时教程踩坑。<!-- more -->

> 重要提示：本博客在搭建过程中清理掉了一批停止维护的第三方服务（Gitment / 畅言 / Disqus 评论、Google Analytics UA、badjs 上报等），并改用了纯静态的数据驱动方案（说说 / 相册 / 视频 / 日志）。下面所有内容均为**当前实际生效**的状态。

---

### **一、环境准备与主题安装✔️**

1.Hexo 基于 Node.js，先装好 [Node.js](https://nodejs.org/) 与 Git，再全局安装 Hexo：

```bash
npm install -g hexo-cli
```

2.新建站点（已有站点可跳过）：

```bash
hexo init blog
cd blog
npm install
```

3.使用 yilia 主题：把主题放到 `themes/yilia/`，然后在根目录 `_config.yml` 指定：

```yaml
theme: yilia
```

4.本地预览与发布命令（常用）：

```bash
hexo clean && hexo g   # 清空并重新生成
hexo s -p 4000         # 本地预览，浏览器开 http://localhost:4000
hexo d                 # 部署（需先配置 deploy）
```

> 注意：yilia 主题的样式源码在 `themes/yilia/source-src/css/`（SCSS），改样式后要到 `themes/yilia` 目录执行 `npm run dist` 编译成 `themes/yilia/source/main.*.css`。浏览器缓存用 `layout/_partial/css.ejs` 里的 `?ver=N` 版本号绕过（改完样式把 N 加 1）。

### **二、目录结构总览✔️**

1.常用目录：

```text
blog/
├─ _config.yml              # 站点配置（菜单、部署、URL 等）
├─ source/
│  ├─ _posts/               # 你的文章（.md）
│  ├─ images/               # 文章配图放这里
│  ├─ assets/img/           # 头像、打赏码、favicon 等
│  ├─ tools/index.md        # 导航页（/tools/）
│  └─ dynamic/              # 动态中心：说说/相册/视频/日志
│     ├─ index.ejs          # 动态聚合页（tab 切换）
│     ├─ shuoshuo/data.json # 说说数据
│     ├─ photos/data.json   # 相册数据
│     ├─ videos/data.json   # 视频数据
│     └─ log/data.json      # 日志数据
└─ themes/yilia/
   ├─ _config.yml           # 主题配置（核心）
   ├─ source-src/css/       # 样式源码（SCSS）
   └─ layout/               # 页面模板
```

### **三、配置文件总览✔️**

1.本博客有两层配置，别改混：

- **根 `_config.yml`**：站点级，管 URL、部署、文章永久链接、菜单入口等。
- **`themes/yilia/_config.yml`**：主题级，管外观与功能开关（头像、打赏、目录、智能菜单、友链等）。

2.本博客的关键设定（根 `_config.yml`）：

```yaml
url: https://liuliange.github.io
root: /
permalink: archives/:abbrlink.html   # 用 abbrlink 生成固定短链接
post_asset_folder: false             # 不启用按文章名建资源目录
theme: yilia
deploy:
  type: git
  repo:
    github: git@github.com:liuliange/liuliange.github.io.git
  branch: master
```

> 因为 `post_asset_folder: false`，文章配图不要依赖「同名资源文件夹」，统一放 `source/images/` 用绝对路径引用（见第五节）。

### **四、写文章：新建与 front-matter 字段✔️**

1.新建文章：

```bash
hexo new "文章标题"
# 生成 source/_posts/文章标题.md
```

2.文章头部（front-matter）可用字段，本博客实际支持的：

```yaml
---
title: 文章标题
date: 2026-08-03 16:13:50     # 发布时间
categories:                    # 分类，可多级（每行一个）
- hexo
- 工具
tags: [hexo, 教程]             # 标签，数组
top: 0                         # 是否置顶，0 不置顶；非 0 为置顶权重
essayending: true              # 文章结尾「本文结束」装饰，true 开启
declare: true                  # 版权声明，true 开启
reward: true                   # 打赏按钮，true 开启
toc: true                      # 文章目录（详见第十四节，主题已全局开启）
abstract:                      # 摘要（卡片上显示；留空自动截取）
password:                      # 文章加密密码（需 hexo-blog-encrypt）
---
```

3.正文里用 `<!-- more -->` 分隔摘要与全文；目录会自动根据正文里的 `##` / `###` 标题生成。

### **五、图片怎么上传✔️**

1.**文章配图**：因为 `post_asset_folder: false`，把图片放进 `source/images/`，正文用绝对路径引用（根目录 `root: /`）：

```markdown
![示意图](/images/demo.jpg)
```

2.**头像 / 打赏码 / favicon**：放在 `source/assets/img/` 下，并在主题 `_config.yml` 里指向对应路径：

```yaml
avatar: /assets/img/logo.jpg        # 头像
alipay: /assets/img/alipay.jpg      # 支付宝收款码
weixin: /assets/img/WeChatPay.jpg   # 微信收款码
favicon: /assets/img/favicon.ico    # 浏览器图标
```

3.**相册图片**：放 `source/dynamic/photos/` 下（或任意可访问的图床 / CDN），在相册的 `data.json` 里写路径（见第十一节）。

### **六、分类与标签怎么增加✔️**

1.**分类**：在文章 front-matter 里写 `categories:`，每行一个，可多级嵌套：

```yaml
categories:
- hexo      # 一级分类
- 工具      # 二级分类（可选）
```

新分类第一次出现会自动创建，无需手动建目录。分类页在 `https://你的域名/categories`。

2.**标签**：用数组形式：

```yaml
tags: [hexo, 教程, 前端]
```

标签页在 `https://你的域名/tags`。分类与标签都会在右侧边栏和文章底部展示。

### **七、菜单导航怎么配置✔️**

1.顶部菜单在 `themes/yilia/_config.yml` 的 `menu:` 里配置，键值对是「显示名: 链接」：

```yaml
menu:
  主页: /
  归档: /archives/index.html
  分类: /categories
  动态: /dynamic
  导航: /tools/
```

2.社交图标（侧边栏 / 移动端）在 `subnav:` 里，取消注释即可启用，支持 github / weibo / qq / weixin / mail / bilibili 等：

```yaml
subnav:
  github: "https://github.com/liuliange"
  weibo: "https://weibo.com/u/xxx"
  qq: "https://qm.qq.com/xxx"
  # weixin: "#"      # 微信一般留 #，配合悬浮菜单的二维码
```

### **八、导航页（/tools/）怎么添加链接✔️**

1.「导航」是一个独立自定义页面，正文在 `source/tools/index.md`，用 `layout: tools` 渲染。它按「分组」组织链接，目前有 5 个分组：我也在这里、网盘资源、实用工具、跳转链接、开发资源。

2.加一个链接，就在对应分组的 `.nav-links` 里加一个 `<a>`：

```html
<div class="nav-links">
  <a class="nav-link" href="https://github.com/durian" target="_blank">
    <i class="icon icon-link"></i>GitHub</a>
  <!-- 复制上面这一行，改 href 和文字即可 -->
</div>
```

3.加一个全新分组，复制一整个 `<div class="nav-group">` 块，改 `nav-group-title`（标题）、`nav-group-desc`（描述）和里面的链接即可。图标统一用 `icon-link`（主题图标字体里存在的字形）。

4.要让它出现在顶部菜单，记得第七节的 `menu:` 里加 `导航: /tools/`（本博客已加）。

### **九、智能菜单 / 友链 / 关于我✔️**

1.**智能菜单**（鼠标移到头像弹出的小菜单）在 `themes/yilia/_config.yml`：

```yaml
smart_menu:
  innerArchive: '所有文章'
  friends: '友链'
  aboutme: '关于我'
```

不需要某项就删掉对应行。

2.**友链**：在同文件 `friends:` 下加键值对：

```yaml
friends:
  榴莲: https://www.durian.cn/
  淘宝优惠券: https://www.jianpianyi.cn/
```

3.**关于我**：同文件 `aboutme:` 字段，支持简单 HTML（`<br/>` 换行）：

```yaml
aboutme: 榴莲哥<br/><br/>来自广西崇左<br/>热爱生活喜欢折腾
```

### **十、说说（shuoshuo）怎么发布✔️**

1.说说是一个竖向时间线，数据驱动，**不用写文章**，改 JSON 即可。数据文件：`source/dynamic/shuoshuo/data.json`。

2.格式是一个 `list` 数组，每条含 `date`（YYYY-MM-DD）和 `content`（文字，可含 HTML）：

```json
{
  "list": [
    { "date": "2026-08-03", "content": "博客大改完成，重来！" },
    { "date": "2026-08-01", "content": "今天加了深色模式" }
  ]
}
```

3.发布新说说：在 `list` 里加一个对象，保存后 `hexo g` 重新生成即可（最新在上）。访问 `/dynamic` 或 `/dynamic/shuoshuo/` 查看。

> 旧方案（LeanCloud + Artitalk 评论型说说）已废弃，本博客不再使用，请勿照老教程配置。

### **十一、相册（photos）怎么添加图片/视频✔️**

1.相册按「年份 + 月份」分组，数据在 `source/dynamic/photos/data.json`。先放图片到 `source/dynamic/photos/`（或图床），再写数据。

2.格式：`list` 数组，每个对象是一个「年-月」分组，`arr` 里 `link` / `type` / `text` 三个数组**一一对应**：

```json
{
  "list": [
    {
      "year": 2026,
      "month": 8,
      "arr": {
        "link": ["/dynamic/photos/a.jpg", "/dynamic/photos/clip.mp4"],
        "type": ["image", "video"],
        "text": ["示例图片 1", "示例视频"]
      }
    }
  ]
}
```

- `link`：图片或视频地址（本地路径或外链均可）
- `type`：`image`（图片）或 `video`（视频）
- `text`：鼠标悬停时显示的说明文字

3.加一个月的分组：复制一个 `{ "year":..., "month":..., "arr":{...} }` 对象放进 `list`。年份时间轴会自动按数据生成，点击年份可筛选。

### **十二、视频（videos）怎么添加✔️**

1.视频模块数据在 `source/dynamic/videos/data.json`，用 iframe 嵌入（B 站等）。

2.格式：`list` 数组，每条含 `title`（标题）和 `src`（嵌入地址）：

```json
{
  "list": [
    {
      "title": "示例：B站视频",
      "src": "https://player.bilibili.com/player.html?bvid=BV1GJ411x7h7"
    }
  ]
}
```

3.加视频：复制一条，把 `src` 换成目标平台的嵌入地址（B 站用 `player.bilibili.com/player.html?bvid=xxx`），保存后重新生成。访问 `/dynamic/videos/` 查看。

### **十三、日志（log）在哪里写✔️**

1.日志是另一条时间线（jazz-timeline 样式），数据在 `source/dynamic/log/data.json`。和说说不同，日志每条可带 `title`（标题）。

2.格式：`list` 数组，每条含 `date`、`title`、`content`：

```json
{
  "list": [
    {
      "date": "2026-08-02",
      "title": "清理停止维护的第三方服务",
      "content": "移除 Gitment / 畅言 / Disqus 评论模板，博客不再依赖失效的外部服务。"
    }
  ]
}
```

3.写日志：在 `list` 里加对象，`date` 越新越靠上。访问 `/dynamic/log/` 查看。

### **十四、文章目录（TOC）怎么开✔️**

1.**本博客已全局开启目录**，配置在 `themes/yilia/_config.yml`：

```yaml
toc: 2   # 0-关闭；1-仅 md 里 toc:true 才显示；2-所有文章均显示
toc_hide_index: true      # 隐藏目录里重复的序号
toc_empty_wording: '目录，不存在的…'
```

设为 `2` 后，每篇文章右上角会有一个目录图标，鼠标悬停弹出本文大纲，无需在文章里额外设置。

2.**重要纠正**：旧教程里「在文章里插入 `show-toc-btn` / `toc-article` 代码块来自定义目录」的做法是**过时的、且有 BUG 的旧方案，已被整体删除**。现在目录统一由主题渲染，文章里只需写正常的 `##` / `###` 标题即可，**不要**再往文章里粘贴任何 TOC 相关代码。

### **十五、深色模式与主题切换✔️**

1.本博客支持深色模式（全量换肤方案），并带防闪烁脚本，刷新不会白屏闪一下。

2.切换方式：右下角悬浮菜单最底部的「🌓」按钮，点击在深 / 浅色间切换；选择会记在浏览器 `localStorage`，下次访问保持。首次访问会跟随系统偏好（系统深色则默认深色）。

3.想改配色：变量集中在 `themes/yilia/source-src/css/dark.scss` 顶部的 CSS 变量（`--d-bg` / `--d-card` / `--d-text` 等），改完 `npm run dist` 编译。

### **十六、右下角悬浮菜单✔️**

1.右下角有一个主按钮（「+」），点击向上展开 5 个子按钮：

- ↑ 回到顶部
- 微信（弹出二维码，配 `subnav.weixin`）
- QQ 群（跳转 `subnav.qq`）
- 网盘资源库（跳转到导航页的网盘分组，按需改链接）
- 🌓 主题切换（深 / 浅色）

2.样式在 `themes/yilia/source-src/css/floating-menu.scss`，结构在 `layout/_partial/floating-menu.ejs`。要改链接或增减按钮，编辑该 ejs 即可。

3.回到顶部已适配本主题的滚动容器（`#container`），点击可正常回到顶部。

### **十七、打赏 / 版权声明 / 文章结尾✔️**

1.**打赏**：`themes/yilia/_config.yml`：

```yaml
reward_type: 2   # 0-关；1-仅 md 里 reward:true 才有；2-所有文章都有
reward_wording: '谢谢你请我吃糖果'
alipay: /assets/img/alipay.jpg
weixin: /assets/img/WeChatPay.jpg
```

2.**版权声明**：`declare:` 段配置协议与图标；文章里 `declare: true` 开启单篇声明。

3.**文章结尾提示**：`essayending_type: 1` 表示「需文章 front-matter 里 `essayending: true` 才显示」。结尾文案在 `layout/_partial/article.ejs` 的 essayending 块，本博客为 `---本文结束🐾感谢您的阅读---`。

### **十八、评论功能（当前状态）✔️**

1.**本博客目前未启用任何评论系统**。原有的 Gitment / 畅言 / Disqus 模板与 Valine 相关代码已在新一轮清理中移除，文章 front-matter 里的 `valineenbale` 字段仅作保留、不再生效，请知悉，**不要误以为配置后就有评论**。

2.如需评论，可自行接入仍在维护的方案（如 Waline / Twikoo / Giscus），在 `themes/yilia/layout/_partial/article.ejs` 文章底部插入对应脚本即可。

### **十九、字数统计 / 访问量 / 其它小功能✔️**

1.字数统计：主题 `_config.yml` 的 `word_count: true`（依赖相关 Hexo 插件，如 `hexo-wordcount`）。

2.访问量（不蒜子）：`busuanzi.enable: true`，显示在页面底部。

3.中英文字间距美化：`pangu: true`（自动在中英文间加空格）。

4.标签页标题切换（离开/回到页面时改标题）：`tab_title_change.enable: true`，文案在同段 `left_tab_title` / `return_tab_title`。

### **二十、部署与本地预览✔️**

1.部署到 GitHub Pages：根 `_config.yml` 已配 `deploy.type: git`，执行：

```bash
hexo clean && hexo g && hexo d
```

> 部署前确保已配置好 Git 的 SSH key 与仓库权限。

2.本地预览：

```bash
hexo s -p 4000   # 打开 http://localhost:4000
```

改完内容刷新即可；改了样式则需重新 `npm run dist` 并刷新（必要时 `hexo clean && hexo g`）。

### **二十一、常见问题 / 避坑✔️**

1.**改了样式不生效**：样式在 `source-src/css/` 是 SCSS 源码，必须到 `themes/yilia` 执行 `npm run dist` 编译；再 `hexo clean && hexo g`；最后把 `css.ejs` 里的 `?ver=N` 加 1 绕过浏览器缓存。

2.**文章目录不显示**：确认主题 `toc` 不是 `0`；且**不要**在文章里粘贴旧的 `show-toc-btn` 代码（已废弃）。

3.**说说 / 相册 / 视频 / 日志不显示**：检查对应的 `data.json` 是否为合法 JSON（数组字段别写错），`hexo g` 后数据会拷贝到 `public/dynamic/`。

4.**图片 404**：文章配图用 `/images/xxx.jpg` 绝对路径；头像 / 打赏码放在 `source/assets/img/` 且路径与主题配置一致。

5.**菜单不出现**：在 `themes/yilia/_config.yml` 的 `menu:` 加对应项；自定义页（如导航）记得 `hexo new page tools` 或在 `source/tools/index.md` 提供页面。

---

以上就是本博客 yilia 主题的完整配置与使用说明。照着做基本能跑通大部分功能；遇到主题源码层面的定制，再针对性改 `source-src/css/` 与 `layout/` 即可。
