
## 第1章 前端全貌与开发环境

### 1.1 HTML、CSS、JavaScript 各自负责什么

| 技术 | 核心职责 | 在登录页中的体现 |
| --- | --- | --- |
| HTML | 描述内容、结构和语义 | 标题、账号输入框、密码输入框、登录按钮 |
| CSS | 描述外观和布局 | 颜色、间距、居中、手机适配、按钮状态 |
| JavaScript | 处理数据、事件和交互 | 校验输入、调用登录接口、展示结果 |

例如，HTML 写出一个按钮，CSS 让它呈现蓝色，JavaScript 让点击按钮产生行为。

```html
<button id="hello-button" type="button">打个招呼</button>

<style>
  #hello-button {
    padding: 8px 16px;
    color: white;
    background: #2563eb;
    border: 0;
    border-radius: 6px;
    cursor: pointer;
  }
</style>

<script>
  const button = document.querySelector('#hello-button');
  button.addEventListener('click', () => {
    alert('你好，前端！');
  });
</script>
```

Vue 建立在这三者之上：模板涉及 HTML，样式依然主要使用 CSS，数据和逻辑仍然由 JavaScript 表达。框架可以减少手动操作 DOM 的代码，但不会替代语言基础。

### 1.2 浏览器、前端、后端之间的关系

以查询商品为例：

1. 浏览器加载 HTML、CSS、JavaScript。
2. 用户输入关键词并点击搜索。
3. JavaScript 发起 HTTP 请求，例如 `GET /api/products?keyword=键盘`。
4. Java 后端完成鉴权、业务处理和数据库查询。
5. 后端返回 JSON。
6. JavaScript 根据数据更新页面。

“页面展示”与“业务数据来源”是两件事。没有后端时，也能完成静态页面和本地交互；涉及真实账户、订单、权限等业务时，需要后端参与。

### 1.3 语言和运行环境要分清

| 名称 | 是什么 | 例子 |
| --- | --- | --- |
| ECMAScript | JavaScript 语言规范 | 变量、函数、数组、Promise、模块语法 |
| 浏览器 Web API | 浏览器提供的能力 | DOM、事件、`fetch`、`localStorage` |
| Node.js | 浏览器之外的一种 JavaScript 运行环境 | 文件操作、运行开发服务器、构建工具 |
| Vue | UI 框架 | 组件、响应式状态、模板 |
| npm | 包管理工具及相关生态 | 安装依赖、运行项目脚本 |

`document` 不是 JavaScript 语言本身的关键字，而是浏览器环境中的对象。普通 Node.js 脚本默认没有浏览器的 DOM。

不要因为 JavaScript 名字里有 Java，就认为两种语言的类型、对象和异步模型一致。

### 1.4 推荐的练习文件组织

一个最小练习通常只需要三个文件：

| 文件 | 内容 |
| --- | --- |
| `index.html` | 页面结构，引用 CSS 和 JS |
| `style.css` | 页面样式 |
| `main.js` | 页面逻辑 |

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>前端练习</title>
    <link rel="stylesheet" href="./style.css">
    <script src="./main.js" defer></script>
  </head>
  <body>
    <h1>我的第一个页面</h1>
  </body>
</html>
```

运行普通静态页面可以直接打开 HTML，但涉及模块、接口请求或存储时，应使用本地 HTTP 服务器，避免 `file://` 带来的环境差异。例如：

```bash
# 在练习文件所在目录执行；需要本机已安装 Python
python -m http.server 5500 --bind 127.0.0.1
```

然后访问 `http://127.0.0.1:5500/`。某些系统的命令是 `python3` 或 `py`，以实际安装情况为准。

### 1.5 开发者工具是基本功

一般可通过 F12 或浏览器菜单打开开发者工具。

| 面板 | 主要用途 | 常见排查场景 |
| --- | --- | --- |
| Elements / 元素 | 检查 DOM、样式、盒模型 | 样式没生效、宽度超出、元素被遮挡 |
| Console / 控制台 | 查看日志和错误，临时执行 JS | 变量未定义、类型错误、接口异常 |
| Sources / 源代码 | 打断点、单步调试 | 看某个函数何时执行、参数是什么 |
| Network / 网络 | 查看请求与响应 | 404、跨域、请求参数错误、返回结构不符 |
| Application / 应用 | 查看本地存储、Cookie 等 | 数据为何没保存、登录态问题 |
| 响应式设备工具 | 模拟不同视口 | 手机布局、横向滚动条 |

遇到错误先读 Console 的第一条相关报错，再看代码位置，不要只反复刷新。

---

## 第2章 HTML：结构、语义与表单

### 2.1 HTML 是标记语言

HTML 用元素标记内容的意义和结构。

```html
<p class="intro">我正在学习前端。</p>
```

- `<p>`：开始标签。
- `class="intro"`：属性。
- 中间文本：内容。
- `</p>`：结束标签。

整个组合称为一个元素。`img`、`input`、`br`、`meta`、`link` 等是空元素，不包含子内容，也不写结束标签。

HTML 通常不区分标签名大小写，但应统一使用小写。属性值尽量用引号包起来。

### 2.2 基本骨架逐项解释

| 写法 | 作用 |
| --- | --- |
| `<!doctype html>` | 声明现代 HTML 文档类型，让浏览器使用标准模式 |
| `<html lang="zh-CN">` | 文档根元素，声明主要语言 |
| `<head>` | 元数据、标题、样式和资源引用 |
| `<meta charset="UTF-8">` | 声明字符编码，避免中文乱码 |
| `viewport` 元信息 | 配合 CSS 让移动设备按合适的布局视口展示页面 |
| `<title>` | 浏览器标签页标题，区别于正文标题 `h1` |
| `<body>` | 用户主要看到和操作的内容 |

只加 `viewport` 不会自动完成移动适配，仍需要弹性布局、媒体查询等 CSS。

### 2.3 常用文本与容器元素

| 元素 | 用途 | 注意 |
| --- | --- | --- |
| `h1`～`h6` | 各级标题 | 按内容层次选择，不要为了字号随意跳级 |
| `p` | 段落 | 不要在里面嵌套 `div` 等不允许的结构 |
| `strong` | 强调重要性 | 通常显示为粗体，但语义不等于“加粗工具” |
| `em` | 语气强调 | 通常显示为斜体 |
| `span` | 通用行内容器 | 给一小段内容加样式或标记 |
| `div` | 通用容器 | 没有更合适的语义元素时使用 |
| `br` | 必要的换行 | 不要用多个 `br` 代替段落和间距 |
| `hr` | 主题分隔 | 样式可以通过 CSS 调整 |
| `pre`、`code` | 预格式文本、代码 | `pre` 保留空格和换行 |

HTML 源码里的多个普通空格、换行，通常会在默认空白处理规则下折叠。排版空间主要交给 CSS。

### 2.4 语义化布局

```html
<header>
  <h1>学习记录</h1>
  <nav aria-label="主导航">
    <a href="./index.html">首页</a>
    <a href="./notes.html">笔记</a>
  </nav>
</header>

<main>
  <article>
    <h2>今天学了什么</h2>
    <p>HTML 表单和 CSS 布局。</p>
  </article>
  <aside aria-label="相关资料">参考链接</aside>
</main>

<footer>我的前端学习站</footer>
```

| 元素 | 常见含义 |
| --- | --- |
| `header` | 页面或某个内容区域的介绍部分 |
| `nav` | 主要导航 |
| `main` | 页面主体内容 |
| `article` | 相对独立、可单独理解的内容 |
| `section` | 有主题的内容分区，通常搭配标题 |
| `aside` | 与主要内容相关的补充 |
| `footer` | 页面或某个内容区域的页脚 |

语义化能改善结构可读性、辅助技术理解和搜索引擎理解，但不会自动产生你想要的布局。

### 2.5 链接与路径

```html
<a href="https://developer.mozilla.org/" target="_blank" rel="noopener noreferrer">
  打开 MDN
</a>

<a href="#contact">跳到联系方式</a>
<h2 id="contact">联系方式</h2>
```

`noopener` 用于隔离新窗口对原窗口的 opener 访问；`noreferrer` 还会不发送来源信息。是否保留来源信息应按业务决定。

| 路径 | 含义 |
| --- | --- |
| `./images/logo.png` | 当前文档目录下的资源 |
| `../images/logo.png` | 上一级目录下的资源 |
| `/images/logo.png` | 当前站点根路径下的资源 |
| `https://example.com/logo.png` | 完整绝对 URL |

补充：HTML 中的相对资源路径通常相对文档 URL 解析；CSS 文件里的 `url(...)` 通常相对该 CSS 文件 URL 解析；模块中的 `import './utils.js'` 相对导入它的模块 URL 解析。`<base>` 等机制会影响部分解析规则，初学时不必主动使用。

“跳转”优先用 `a`，“执行操作”优先用 `button`。不要用无意义的 `href="#"` 充当所有按钮。

### 2.6 图片、音视频与替代文本

```html
<img
  src="./images/avatar.jpg"
  alt="用户头像"
  width="96"
  height="96"
>

<!-- 纯装饰图片的替代文本可以为空 -->
<img src="./images/decoration.svg" alt="" loading="lazy">

<video controls width="480">
  <source src="./videos/demo.mp4" type="video/mp4">
  你的浏览器不支持该视频。
</video>
```

- `alt` 用于传达图片承载的信息，图片加载失败、屏幕阅读器读取时都很有用。
- 为图片设置合适的宽高可预留空间，减少加载时的布局跳动。
- 对非首屏重要图片可考虑懒加载；首屏关键图片不应机械地全部懒加载。
- 不要假定浏览器允许有声视频自动播放。
- 响应式图片可进一步学习 `srcset`、`sizes` 和 `picture`。

### 2.7 列表与表格

```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>

<ol>
  <li>输入账号</li>
  <li>输入密码</li>
  <li>点击登录</li>
</ol>

<table>
  <caption>学习进度</caption>
  <thead>
    <tr><th scope="col">课程</th><th scope="col">状态</th></tr>
  </thead>
  <tbody>
    <tr><td>HTML</td><td>已完成</td></tr>
    <tr><td>CSS</td><td>学习中</td></tr>
  </tbody>
</table>
```

`ul` 是无序列表，`ol` 是有序列表，`dl`、`dt`、`dd` 可表达术语与说明。表格用于具有行列关系的数据，不要用表格搭建整个页面布局。

### 2.8 全局属性与布尔属性

| 属性 | 用途 |
| --- | --- |
| `id` | 文档内的唯一标识；用于锚点、标签关联、选择元素 |
| `class` | 可复用的分类；一个元素可以有多个类名 |
| `title` | 补充说明；不适合作为唯一的必要提示 |
| `data-*` | 自定义数据，JS 可通过 `dataset` 读取 |
| `hidden` | 表达内容当前不应展示 |
| `tabindex` | 调整键盘焦点行为，避免随意使用正整数 |

```html
<button class="button primary" data-user-id="42" type="button">查看用户</button>
```

布尔属性的关键在于是否存在：

```html
<button disabled>不可点击</button>
<button disabled="false">仍然不可点击</button>
```

要启用第二个按钮，应移除 `disabled` 属性，或在 JS 中执行 `button.disabled = false`。

`aria-*` 属性帮助描述辅助技术需要的信息，但不是所有 `aria-*` 都是布尔属性，不能套用上面的存在性规则。

### 2.9 表单完整示例

```html
<form action="/register" method="post">
  <div>
    <label for="username">用户名</label>
    <input id="username" name="username" autocomplete="username"
           required minlength="2" maxlength="20">
  </div>

  <div>
    <label for="email">邮箱</label>
    <input id="email" name="email" type="email" autocomplete="email" required>
  </div>

  <div>
    <label for="password">密码</label>
    <input id="password" name="password" type="password"
           autocomplete="new-password" required minlength="8">
  </div>

  <fieldset>
    <legend>学习方向</legend>
    <label><input type="radio" name="direction" value="frontend" checked>前端</label>
    <label><input type="radio" name="direction" value="backend">后端</label>
  </fieldset>

  <label>
    <input type="checkbox" name="agreement" value="yes" required>
    同意使用协议
  </label>

  <label for="city">城市</label>
  <select id="city" name="city">
    <option value="nanjing">南京</option>
    <option value="suzhou">苏州</option>
  </select>

  <label for="intro">自我介绍</label>
  <textarea id="intro" name="intro" rows="4"></textarea>

  <button type="submit">注册</button>
  <button type="button">查看说明</button>
</form>
```

此例描述传统表单提交，`/register` 需要真实后端。SPA 中常监听 `submit`，阻止默认提交，再使用 JS 请求接口。

### 2.10 表单必须分清的属性

| 属性 | 含义 | 典型误区 |
| --- | --- | --- |
| `id` | 标识元素，供 `label for` 等关联 | 以为后端根据 id 自动收到字段 |
| `name` | 原生表单序列化时的字段名 | 有 id 没有 name，提交时缺少字段 |
| `value` | 控件值或选项提交值 | 以为显示文字就一定是提交值 |
| `placeholder` | 输入提示 | 用它代替 label，输入后就没有标签 |
| `required` | 必填约束 | 以为前端必填能保证后端安全 |
| `readonly` | 不允许用户编辑，适用于支持它的控件 | 与 disabled 混淆 |
| `disabled` | 控件禁用，通常不可聚焦，原生提交时不包含该控件 | 禁用后还期待后端收到它 |
| `checked` | HTML 中设置默认选中状态 | 与 JS 当前选中状态不区分 |

同组单选框使用相同 `name`。未选中的普通复选框通常不出现在原生提交的表单数据中；需要显式的真假值时，可由 JS 自己组织 JSON。

`input.value` 得到的通常是字符串，即使 `type="number"` 也是如此；可用 `Number(...)` 或 `valueAsNumber` 转换，并处理空值和 `NaN`。

`button` 在常见表单使用场景下会触发提交，因此非提交按钮应明确写 `type="button"`。

### 2.11 GET、POST 与文件上传

- `GET` 常将表单字段编码到 URL 查询串，适合查询、搜索等可分享链接的场景。
- `POST` 常将数据放进请求体，适合提交表单等操作。
- POST 不等于加密；传输机密性依赖 HTTPS 等机制。
- 传统表单上传文件需要 `method="post"` 和 `enctype="multipart/form-data"`。

```html
<form action="/api/upload" method="post" enctype="multipart/form-data">
  <label for="avatar">选择头像</label>
  <input id="avatar" type="file" name="avatar" accept="image/*">
  <button type="submit">上传</button>
</form>
```

`accept` 主要用于选择提示，文件格式、大小和内容仍应由后端验证。

### 2.12 HTML 校验与无障碍基础

前端约束能改善体验，后端必须再次校验。攻击者可以绕过页面，直接构造请求。

写页面时形成以下习惯：

1. 表单控件有可识别的标签。
2. 操作用原生按钮，导航用链接。
3. 图片有合理替代文本。
4. 标题结构表达层次。
5. 键盘能访问主要操作，焦点可见。
6. 错误信息用文字说明，不只依赖红色。
7. 不随意移除 `outline`；可以换成更清晰的焦点样式。

HTML 常见错误包括重复 id、不合法嵌套、缺少表单 name、所有按钮都不写 type、把所有容器都写成 div。

---

## 第3章 CSS：样式、盒模型与现代布局

### 3.1 CSS 规则与引入方式

```css
.card {
  color: #1f2937;
  padding: 16px;
}
```

`.card` 是选择器；大括号里是声明；一条声明由属性和值组成。

| 引入方式 | 示例 | 使用场景 |
| --- | --- | --- |
| 外部样式 | `<link rel="stylesheet" href="./style.css">` | 普通项目主方式，便于维护和复用 |
| 内部样式 | `<style>...</style>` | 独立练习、小型示例 |
| 行内样式 | `<div style="color: red">` | 个别场景；不适合大量重复写 |

外部样式并非天然低于内部样式。真正生效的结果由层叠规则决定。

### 3.2 常用选择器

| 选择器 | 例子 | 匹配含义 |
| --- | --- | --- |
| 类型选择器 | `p` | 所有 p 元素 |
| 类选择器 | `.card` | class 包含 card 的元素 |
| ID 选择器 | `#app` | id 为 app 的元素 |
| 通配选择器 | `*` | 所有元素 |
| 属性选择器 | `input[type="email"]` | 具有指定属性值的元素 |
| 后代组合 | `.card p` | card 内任意层级的 p |
| 子组合 | `.card > p` | card 的直接子元素 p |
| 相邻兄弟 | `h2 + p` | 紧随 h2 之后的同级 p |
| 后续兄弟 | `h2 ~ p` | h2 后面的同级 p |
| 选择器列表 | `h1, h2, h3` | 分别匹配这些选择器 |

```css
/* 同一个元素同时有两个类 */
.button.primary { background: blue; }

/* primary 是 button 的后代 */
.button .primary { color: blue; }
```

这两个写法含义不同，空格不是可有可无。

### 3.3 伪类与伪元素

```css
a:hover { color: #2563eb; }
button:disabled { opacity: 0.5; cursor: not-allowed; }
input:focus-visible { outline: 2px solid #2563eb; }
li:nth-child(2n) { background: #f3f4f6; }
.item:not(.active) { color: #6b7280; }

.required::after {
  content: ' *';
  color: #dc2626;
}
```

- 伪类描述元素状态或结构条件，例如悬停、聚焦、某个序号。
- 伪元素选择元素的特定部分或生成的内容，例如 `::before`、`::after`、`::first-letter`。
- `::before`、`::after` 常需要设置 `content` 才生成内容。
- 必要业务信息不要只放在 CSS 生成内容里。
- `:nth-child()` 根据同级元素的位置计数；`p:nth-of-type()` 则按同类型元素计数。

### 3.4 层叠、优先级与继承

不要把 CSS 规则简化成一句“谁权重大谁生效”。判断时要先看规则是否适用，再看来源、重要性、层等因素，然后才比较具体性；后续还可能比较作用域距离与顺序。

对于初学最常见的“同一来源、同一层、同为普通声明”的规则，具体性可以记为三元组：

| 成分 | 示例 | 具体性贡献 |
| --- | --- | --- |
| ID | `#app` | `(1, 0, 0)` |
| 类、属性、伪类 | `.card`、`[disabled]`、`:hover` | `(0, 1, 0)` |
| 类型、伪元素 | `div`、`::before` | `(0, 0, 1)` |
| 通配、组合符 | `*`、`>`、空格 | 不增加 |

按列从左往右比较，不是把数字当十进制相加。再多的类选择器也不能在这个比较阶段跨过一个 ID 的第一列优势。

```css
/* 两条规则具体性相同；如果其他层叠条件相同，后写的生效 */
.title { color: red; }
.title { color: blue; }
```

补充边界：

- 普通行内样式通常胜过普通样式表声明，但并不胜过所有 `!important` 情况。
- `!important` 影响重要性，不是给选择器具体性“加分”。不要用它作为默认解决方案。
- `:where(...)` 的具体性为零；`:is(...)`、`:not(...)`、`:has(...)` 会根据参数中的选择器计算具体性。
- `@layer` 可以管理样式层；需要时再深入，不必第一天掌握复杂规则。

继承是另一件事。`color`、`font-family` 等常可从父元素继承；`margin`、`padding`、`border` 等通常不继承。

```css
body { font-family: system-ui, sans-serif; color: #222; }
button, input, select, textarea { font: inherit; }
```

子元素自己的适用声明一般先于“继承来的父元素值”，不要拿父选择器具体性和子选择器直接竞赛。

层叠细节查阅：[MDN：Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity)。

### 3.5 盒模型

页面中一个普通元素的盒子包括：内容区 content、内边距 padding、边框 border、外边距 margin。

```css
.box {
  width: 200px;
  padding: 20px;
  border: 2px solid #333;
  margin: 16px;
}
```

在默认 `box-sizing: content-box` 下：

- 内容宽度：200px。
- 边框盒宽度：`200 + 20 × 2 + 2 × 2 = 244px`。
- 外边距位于边框盒之外；相邻外边距是否叠加还与布局和外边距折叠有关。

使用 `border-box` 时，声明的 width 包含内容、padding 和 border，不包含 margin。

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

这样布局时更容易估算实际宽度。

### 3.6 margin、padding 与外边距折叠

- `padding` 是边框以内的空间，背景通常会绘制到这里。
- `margin` 是边框以外的空间，用于分隔元素。
- 普通块布局中，某些相邻垂直 margin 会折叠；水平 margin 不这样折叠。
- 父子之间也可能发生垂直 margin 折叠。
- Flex、Grid 容器内的项目不会按普通块布局的方式发生这种外边距折叠。

列表、卡片等布局优先用容器的 `gap` 统一管理间距，可以减少边界处理。

```css
.list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
```

### 3.7 尺寸与单位

| 单位/写法 | 含义 | 适合场景 |
| --- | --- | --- |
| `px` | CSS 像素，并不固定等于一个物理像素 | 边框、精细尺寸 |
| `%` | 相对值，参照对象依具体属性而定 | 容器尺寸、布局 |
| `em` | 通常与字号有关；font-size 使用时参照父字号 | 跟随文字的组件间距 |
| `rem` | 相对根元素字号 | 全站字号与间距尺度 |
| `vw`、`vh` | 视口宽高的百分之一 | 视口相关尺寸 |
| `dvh` | 动态视口高度单位 | 适配移动浏览器可变工具栏 |
| `fr` | Grid 中分配可用空间的比例单位 | 网格列宽 |
| `auto` | 由属性和布局规则计算 | 自适应、居中等 |

```css
.container {
  width: min(100% - 32px, 1100px);
  margin-inline: auto;
}

.page-title {
  font-size: clamp(1.5rem, 3vw, 2.5rem);
}
```

不要认为“百分比高度一定有效”。`height: 100%` 是否能得到预期结果，取决于包含块高度是否有可用的确定参照及具体布局规则。

`min-width`、`max-width`、`min-height`、`max-height` 用于约束尺寸。内容高度不固定时，通常优先 `min-height`，避免写死高度截断内容。

### 3.8 颜色、文字、背景与边框

```css
.article {
  color: #1f2937;
  background-color: rgb(248 250 252);
  font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
  font-size: 16px;
  font-weight: 400;
  line-height: 1.7;
  text-align: left;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  box-shadow: 0 6px 20px rgb(0 0 0 / 0.08);
}

.banner {
  background: linear-gradient(120deg, #2563eb, #7c3aed);
}
```

实用补充：

- 正文 `line-height` 可用无单位值，让后代按自己的字号计算。
- `text-align: center` 主要影响容器内的行内内容，不会自动把任意块盒子居中。
- `opacity` 会影响整个元素及其子内容；半透明背景色可以只影响背景。
- `background-size: cover` 填满时可能裁切；`contain` 完整显示时可能留白。
- `img { object-fit: cover; }` 用于替换元素内容的适配，通常需配合约束尺寸。

### 3.9 display 与普通文档流

| display | 常见表现 |
| --- | --- |
| `block` | 在普通块布局中独占一行，宽度 auto 通常填充可用宽度 |
| `inline` | 跟随文字排列；普通非替换行内元素不按块盒方式接受 width、height |
| `inline-block` | 行内排列，同时可设置宽高 |
| `none` | 不生成用于布局的盒子 |
| `flex` | 创建 Flex 布局 |
| `grid` | 创建 Grid 布局 |

HTML 标签语义与 CSS display 不应混为一谈。`div` 可以变成行内显示，`span` 可以变成块显示，但这不会改变其 HTML 语义或内容嵌套规则。

### 3.10 Flex：一维布局重点

```css
.toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}
```

Flex 容器的直接子元素成为 Flex 项目。布局围绕主轴和交叉轴展开：

- `flex-direction: row`：常见横向书写模式下，主轴沿行方向。
- `flex-direction: column`：主轴沿列方向。
- `justify-content`：主轴上的空间分配。
- `align-items`：交叉轴上的项目对齐。

不要死记“justify 水平、align 垂直”；方向改变后，它们对应的物理方向也会改变。

| 容器属性 | 常用值 |
| --- | --- |
| `flex-direction` | `row`、`column` |
| `justify-content` | `flex-start`、`center`、`space-between` |
| `align-items` | `stretch`、`center`、`flex-start` |
| `flex-wrap` | `nowrap`、`wrap` |
| `gap` | 项目间距 |
| `align-content` | 多行情况下交叉轴上的行分配，需要有可分配空间 |

| 项目属性 | 含义 |
| --- | --- |
| `flex-grow` | 正自由空间的增长比例 |
| `flex-shrink` | 空间不足时的缩小因素，计算还涉及基础尺寸 |
| `flex-basis` | 主轴上的初始尺寸依据 |
| `flex` | 上述三个属性的简写 |
| `align-self` | 单个项目覆盖交叉轴对齐方式 |
| `order` | 改变视觉排列顺序，不改变 DOM 顺序 |

常见布局：

```css
.layout { display: flex; gap: 20px; }
.sidebar { flex: 0 0 220px; }
.content { flex: 1; min-width: 0; }
```

这里 `flex: 1` 在浏览器中通常展开为 `1 1 0%`。`min-width: 0` 允许内容区域缩到其内容默认最小宽度以下，常用于解决长文本撑开布局的问题。

注意：`flex: 1` 不意味着所有情况下绝对等宽，项目的最小尺寸、padding、border 等仍会影响结果。

参阅：[MDN：Flex 基本概念](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts)。

### 3.11 Grid：二维布局重点

```css
.product-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 16px;
}
```

这个例子创建三列，每列分配相同份额。`minmax(0, 1fr)` 可以避免某些长内容按默认最小尺寸撑开列。

自适应卡片网格：

```css
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 240px), 1fr));
  gap: 16px;
}
```

| 属性 | 用途 |
| --- | --- |
| `grid-template-columns` | 定义列 |
| `grid-template-rows` | 定义行 |
| `grid-column`、`grid-row` | 控制项目占据的位置或跨度 |
| `grid-template-areas` | 用区域名称描述布局 |
| `gap` | 行列间距 |
| `place-items` | `align-items` 和 `justify-items` 的简写 |

Flex 适合工具栏、导航、单行对齐；Grid 适合卡片网格、具有明确行列关系的页面。两者经常组合使用。

### 3.12 定位 position

| 值 | 说明 |
| --- | --- |
| `static` | 默认定位 |
| `relative` | 保留原位置占位，偏移通常相对自身原位置 |
| `absolute` | 脱离普通流，相对其定位包含块定位 |
| `fixed` | 脱离普通流，通常相对视口；特定祖先属性可改变包含块 |
| `sticky` | 保留布局位置，达到阈值后相对滚动区域表现为粘性定位 |

```css
.card { position: relative; }
.badge { position: absolute; top: 8px; right: 8px; }

.site-header { position: sticky; top: 0; z-index: 10; }
```

“子绝父相”是常见做法，不是完整标准定义。absolute 的包含块还可能由 `transform` 等祖先属性建立；没有相关祖先时会使用初始包含块等规则。

sticky 不生效时检查：有没有 `top` 等阈值、最近滚动祖先是谁、父容器是否有足够滚动空间、相关 overflow 是否改变滚动环境。

### 3.13 z-index 与层叠上下文

`z-index` 不是全页面统一的排名。它的比较受到层叠上下文边界限制。

一个元素即使写 `z-index: 999999`，也可能被另一个层叠上下文中的元素挡住。

常见建立层叠上下文的情况包括：某些定位元素设置非 auto 的 z-index、fixed/sticky、opacity 小于 1、transform 非 none，以及设置非 auto z-index 的 Flex/Grid 项目等。

遇到遮挡问题应检查祖先上下文，而不是只给当前元素不断加数字。

### 3.14 常用居中方法

```css
/* 块盒在常见水平书写场景下水平居中 */
.container { width: 80%; max-width: 1000px; margin-inline: auto; }

/* 子内容水平、垂直居中 */
.center-flex { display: flex; justify-content: center; align-items: center; }

/* 简洁的二维居中 */
.center-grid { display: grid; place-items: center; }

/* 已定位元素居中 */
.parent { position: relative; }
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

垂直居中需要有可用的容器高度或空间；容器高度恰好等于内容高度时，看不出垂直位移。

### 3.15 溢出、隐藏和文本省略

```css
.scroll-panel { max-height: 300px; overflow: auto; }

.single-line {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.long-text { overflow-wrap: anywhere; }
```

单行省略需要存在宽度约束。Flex/Grid 子项还可能需要 `min-width: 0`。

| 方式 | 是否保留布局空间 | 补充 |
| --- | --- | --- |
| `display: none` | 否 | 通常也从辅助技术的可访问树中移除 |
| `visibility: hidden` | 是 | 不可见，通常不可交互 |
| `opacity: 0` | 是 | 透明但仍可能接收点击和键盘焦点 |

`pointer-events: none` 主要改变指针命中行为，不等于完整禁止键盘访问。

### 3.16 响应式与媒体查询

```css
.page {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

@media (min-width: 768px) {
  .page { grid-template-columns: 220px minmax(0, 1fr); }
}
```

这是一种从小屏开始增强的写法。断点应由内容何时放不下决定，不必把某几个设备宽度当成唯一标准。

图片常用处理：

```css
img { max-width: 100%; height: auto; }
```

响应式检查清单：长标题能否换行、按钮是否挤出屏幕、表格是否需要横向滚动、输入框是否超宽、页面缩放后是否仍可操作。

### 3.17 变量、计算与逻辑属性

```css
:root {
  --brand-color: #2563eb;
  --space: 8px;
}

.card {
  padding: calc(var(--space) * 2);
  border-inline-start: 4px solid var(--brand-color);
  margin-block-end: 16px;
}
```

- CSS 自定义属性可参与层叠和继承，便于统一主题。
- `calc()` 可混合兼容的尺寸表达式，`+`、`-` 运算符两侧应留空格。
- `inline`、`block` 逻辑方向有助于适配不同书写方向。
- `var(--name, fallback)` 可指定变量缺失等情况下的回退。

### 3.18 过渡与动画

```css
.button {
  transition: background-color 0.2s, transform 0.2s;
}
.button:hover {
  background-color: #1d4ed8;
  transform: translateY(-2px);
}

@keyframes fade-in {
  from { opacity: 0; transform: translateY(6px); }
  to { opacity: 1; transform: translateY(0); }
}
.panel { animation: fade-in 0.2s ease-out; }

@media (prefers-reduced-motion: reduce) {
  .button { transition: none; }
  .panel { animation: none; }
}
```

过渡用于属性值变化，动画可以定义多个关键帧。优先明确写需要过渡的属性，避免机械使用 `transition: all`。

### 3.19 BFC、浮动与基础渲染认识

BFC 是块级格式化上下文。初学时关注它的效果：形成相对独立的块布局环境、影响与浮动的关系、影响特定 margin 折叠情况。

```css
.article-body { display: flow-root; }
```

这是建立 BFC 的明确方式之一，可用于包含内部浮动。旧项目可能使用 float 做布局、clearfix 清除浮动；新页面主布局先掌握 Flex 和 Grid。float 仍适合图片周围文字环绕等场景。

浏览器通常要经过样式计算、布局、绘制、合成等阶段。改变宽高可能影响布局；transform、opacity 在合适情况下可减少布局或绘制成本，但不能保证任何元素都“只走 GPU”或“零开销”。

### 3.20 CSS 排错顺序

1. 选择器是否选中了目标元素？
2. 样式文件是否成功加载，路径是否正确？
3. 声明是否拼错、值是否合法？
4. 在开发者工具里是否被其他规则划掉？
5. 盒模型的实际宽高是多少？
6. 父容器使用了什么布局？是否受 min-width、overflow、定位约束影响？
7. 问题是否来自层叠上下文或滚动容器？

初学阶段最值得熟练的 CSS 主线是：选择器 → 层叠与继承 → 盒模型 → Flex → Grid → 定位 → 响应式。

---

## 第4章 JavaScript：变量、类型、流程与函数

### 4.1 JavaScript 与 Java 的关键差别

| 维度 | Java | JavaScript |
| --- | --- | --- |
| 类型 | 静态类型，变量声明类型 | 动态类型，值有类型，变量可以绑定不同类型的值 |
| 对象 | 通常基于类创建 | 可直接使用对象字面量，底层继承主要基于原型 |
| 函数 | 通常作为类的方法出现 | 函数是值，可赋给变量、传参、返回 |
| 数组 | 常见数组长度固定且有元素类型 | 普通 Array 可扩展，也可放不同类型，实际应保持结构清晰 |
| 相等 | 对象常用 equals 比内容 | 对象的 `===` 比较身份，不自动深比较 |
| 异步 | 常涉及线程、线程池、Future | 浏览器主线程常用事件循环、Promise、async/await |
| 空值 | 主要是 null | 常区分 undefined 和 null |

JavaScript 的动态类型不等于没有类型；TypeScript 可以进一步增加静态类型检查，但本笔记先讲 JS 本身。

### 4.2 注释、输出与基本规范

```js
// 单行注释
/* 多行注释 */

console.log('普通信息');
console.warn('警告信息');
console.error('错误信息');
console.table([{ name: 'HTML', done: true }]);
```

变量、函数常用 `camelCase`；类名常用 `PascalCase`。保持引号、缩进、分号风格一致。JS 有自动分号插入机制，但不要依赖含糊写法。

```js
function getUser() {
  return {
    name: '小轩'
  };
}
```

不要在 `return` 后直接换行再写返回表达式，否则可能被当作 `return;`。

### 4.3 let、const、var

```js
let score = 80;
score = 90;

const school = '南京';
const user = { name: '小轩', age: 20 };
user.age = 21; // 合法：没有重新绑定 user
// user = {}; // 错误：不能重新赋值给 const 绑定
```

| 特性 | let | const | var |
| --- | --- | --- | --- |
| 常见作用域 | 块级 | 块级 | 函数级或全局 |
| 可重新赋值 | 可以 | 不可以 | 可以 |
| 同一作用域重复声明 | 不可以 | 不可以 | 很多情况下可以 |
| 声明前访问 | 暂时性死区，报错 | 暂时性死区，报错 | 常得到 undefined |
| 新代码选择 | 需要重新赋值时 | 默认优先 | 主要用于理解旧代码 |

`const` 保证绑定不能改变，不保证对象内部不可变。

```js
if (true) {
  let a = 1;
  var b = 2;
}
// console.log(a); // ReferenceError
console.log(b); // 2
```

### 4.4 数据类型

JavaScript 有七种原始类型，以及 Object 类型。

| 类型 | 示例 | 说明 |
| --- | --- | --- |
| string | `'hello'` | 字符串，字符串值不可直接原地修改 |
| number | `3.14`、`42` | 常见数值；包含 NaN、Infinity |
| bigint | `123n` | 任意精度整数，不是小数类型 |
| boolean | `true`、`false` | 布尔值 |
| undefined | `undefined` | 未赋值、属性不存在、无显式返回值等 |
| null | `null` | 常用于主动表达没有值 |
| symbol | `Symbol('id')` | 唯一标识等进阶用途 |
| object | `{}`、`[]`、函数、Date | 对象类值；函数可调用 |

```js
let x;
console.log(x);                  // undefined
console.log({}.name);            // undefined
console.log(typeof 1);           // 'number'
console.log(typeof null);        // 'object'，历史原因
console.log(typeof []);          // 'object'
console.log(typeof (() => {}));  // 'function'
console.log(Array.isArray([]));  // true
```

判断 null 用 `value === null`，判断数组用 `Array.isArray(value)`。不要用 `typeof value === 'object'` 就断言它是普通对象，因为 null、数组等也涉及这个结果。

### 4.5 数值、精度与转换

```js
console.log(0.1 + 0.2 === 0.3); // false
console.log(Number('42'));      // 42
console.log(Number(''));        // 0，空字符串要特别处理
console.log(Number('abc'));     // NaN
console.log(parseInt('12px', 10)); // 12
console.log(Number.isNaN(Number('abc'))); // true
```

- number 的常见小数运算受二进制浮点表示影响。
- 金额等业务不要随意依赖浮点运算。可以在明确范围和舍入规则下用最小货币单位整数，或使用经过选择的十进制方案。
- 安全整数范围为 `-(2 ** 53 - 1)` 到 `2 ** 53 - 1`。
- Java 的 long 类型 ID 可能超过 JS 安全整数范围；前后端可约定将此类 ID 作为字符串传输和处理。
- BigInt 不能直接与 number 混合做算术，默认也不能直接被 `JSON.stringify` 序列化。
- `toFixed(2)` 返回字符串，用于展示格式化时要分清类型。

处理数字输入时，先确认是否为空，再转换和检查：

```js
function parseAge(raw) {
  if (raw.trim() === '') throw new Error('年龄不能为空');
  const age = Number(raw);
  if (!Number.isInteger(age) || age < 0 || age > 150) {
    throw new Error('年龄不合法');
  }
  return age;
}
```

### 4.6 隐式转换与相等比较

```js
console.log('5' + 1);    // '51'
console.log('5' - 1);    // 4
console.log(1 == '1');   // true
console.log(1 === '1');  // false
console.log(null == undefined);  // true
console.log(null === undefined); // false
```

默认优先使用 `===`、`!==`，避免意外类型转换。

对象即使内容一样，也可能不相等：

```js
console.log({ id: 1 } === { id: 1 }); // false
const first = { id: 1 };
const second = first;
console.log(first === second);       // true
```

### 4.7 真值、假值与逻辑运算符

初学需记住的假值：`false`、`0`、`-0`、`0n`、`''`、`null`、`undefined`、`NaN`。历史浏览器对象 `document.all` 有特殊行为，日常业务不应依赖它。

`[]`、`{}`、`'0'`、`'false'` 都是真值。

```js
const username = '';
const displayName = username || '匿名用户';
const count = 0;
console.log(count || 10); // 10
console.log(count ?? 10); // 0
```

| 运算符 | 规则 |
| --- | --- |
| `a && b` | a 为假值时返回 a，否则返回 b |
| `a || b` | a 为真值时返回 a，否则返回 b |
| `a ?? b` | a 为 null 或 undefined 时返回 b，否则返回 a |
| `!a` | 取布尔反值 |
| `!!a` | 将 a 转为布尔值 |

`&&`、`||` 不一定返回 boolean。表示“缺失时使用默认值”时，先考虑 `??` 是否更符合业务。`??` 与 `&&`、`||` 直接混写需要括号明确分组。

### 4.8 流程控制与循环

```js
const score = 85;
if (score >= 90) {
  console.log('优秀');
} else if (score >= 60) {
  console.log('及格');
} else {
  console.log('待提高');
}

const label = score >= 60 ? '通过' : '未通过';

for (let i = 0; i < 3; i++) {
  console.log(i);
}

const names = ['HTML', 'CSS', 'JS'];
for (const name of names) {
  console.log(name);
}
```

还应认识 `switch`、`while`、`do...while`、`break`、`continue`。`switch` 的 case 匹配不做宽松相等转换，未写 break 可能继续执行后续分支。

| 遍历方式 | 更适合什么 |
| --- | --- |
| 普通 for | 需要索引、步进或灵活控制 |
| for...of | 读取数组、字符串等可迭代对象的值 |
| for...in | 遍历可枚举字符串属性名，可能包含继承属性 |
| Object.keys / entries | 遍历对象自身的可枚举字符串属性 |

不要默认用 `for...in` 遍历数组的元素值。普通对象默认也不能直接 `for...of`，可以遍历 `Object.entries(obj)`。

### 4.9 字符串与模板字符串

```js
const language = 'Vue';
const hours = 2;
const message = `今天学习 ${language}，共 ${hours} 小时`;

const raw = '  JavaScript  ';
console.log(raw.trim());                 // 'JavaScript'
console.log(raw.includes('Script'));     // true
console.log('a,b,c'.split(','));         // ['a', 'b', 'c']
console.log('hello'.slice(0, 2));         // 'he'
console.log('vue'.toUpperCase());        // 'VUE'
console.log('a-b-a'.replaceAll('a', 'x')); // 'x-b-x'
```

字符串不可变，这些方法返回新值。JS 字符串按 UTF-16 码元存储，`'😀'.length` 是 2；涉及用户感知字符数量时，不要简单认为 length 永远等于屏幕上的字符数。

### 4.10 函数的三种常见形式

```js
// 函数声明
function add(a, b) {
  return a + b;
}

// 函数表达式
const subtract = function (a, b) {
  return a - b;
};

// 箭头函数
const multiply = (a, b) => a * b;
```

函数没有显式 return 时，返回 undefined。函数声明与函数表达式在声明前是否可调用等方面有差异，不要混用规则。

默认参数、剩余参数和箭头函数返回对象：

```js
function greet(name = '同学') {
  return `你好，${name}`;
}

function sum(...numbers) {
  return numbers.reduce((total, number) => total + number, 0);
}

const makeUser = name => ({ name, active: true });
```

- 默认参数在传入 undefined 或未传参时生效；传入 null 不会自动使用默认值。
- `...numbers` 在参数位置收集剩余参数，得到数组。
- 箭头函数直接返回对象时，用括号包住对象字面量。

### 4.11 函数是一等公民：回调与高阶函数

```js
function calculate(a, b, operation) {
  return operation(a, b);
}

console.log(calculate(3, 4, (a, b) => a + b)); // 7
```

函数可以存入变量、作为参数传入、作为结果返回。接收函数或返回函数的函数常被称为高阶函数。

```js
// 传入函数，等待未来点击时调用
button.addEventListener('click', handleClick);

// 这是现在立刻调用，再把返回值传进去，通常不是你想要的
// button.addEventListener('click', handleClick());
```

上例中的 `button`、`handleClick` 需由实际页面提供。回调不等于异步；例如 `map` 的普通回调通常在当前同步调用中执行。

### 4.12 异常与调试

```js
function divide(a, b) {
  if (b === 0) throw new Error('除数不能为 0');
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (error) {
  console.error(error instanceof Error ? error.message : String(error));
} finally {
  console.log('执行清理');
}
```

`try...catch` 能捕获当前同步执行链上的异常，以及在 try 内被 await 的 Promise 拒绝。它不能自动捕获未来另一个定时器回调里抛出的错误。

常见报错：

| 错误 | 常见原因 |
| --- | --- |
| SyntaxError | 括号缺失、语法写错 |
| ReferenceError | 变量不存在、暂时性死区访问 |
| TypeError | 对 undefined 取属性、把非函数当函数调用 |
| RangeError | 无效长度、递归过深等 |

---

## 第5章 JavaScript：对象、数组与现代语法

### 5.1 对象字面量与属性访问

```js
const user = {
  id: '1001',
  name: '小轩',
  age: 20,
  address: { city: '南京' },
  introduce() {
    return `我是${this.name}`;
  }
};

console.log(user.name);
console.log(user['name']);

const field = 'age';
console.log(user[field]); // 20，动态属性名
user.email = 'user@example.com';
delete user.email;
```

`user.field` 读取名为 field 的属性，`user[field]` 读取变量 field 的值所指定的属性。

属性简写与计算属性名：

```js
const name = '小轩';
const key = 'score';
const record = { name, [key]: 95 };
// 等价于 { name: '小轩', score: 95 }

for (const [key, value] of Object.entries(record)) {
  console.log(key, value);
}
```

常用 API：`Object.keys(obj)`、`Object.values(obj)`、`Object.entries(obj)`、`Object.hasOwn(obj, key)`。`key in obj` 还会检查原型链上的属性，不仅是自身属性。

### 5.2 数组的基础操作

```js
const items = ['HTML', 'CSS'];
items.push('JS');     // 尾部添加，返回新长度
items.pop();         // 尾部移除，返回被移除的值
items.unshift('Web'); // 头部添加
items.shift();       // 头部移除

console.log(items[0]);
console.log(items.length);
console.log(items.includes('CSS'));
console.log(items.indexOf('CSS'));
console.log(items.join(' / '));
```

尽量避免稀疏数组，例如给长度很小的数组直接写 `arr[100] = value`。不同遍历方法对空槽的处理不完全相同。

### 5.3 slice 与 splice

```js
const a = [10, 20, 30, 40];
console.log(a.slice(1, 3)); // [20, 30]，结束下标不包含
console.log(a);            // 原数组未变

const removed = a.splice(1, 2, 99);
console.log(removed);      // [20, 30]
console.log(a);            // [10, 99, 40]
```

- `slice(start, end)`：截取，返回新数组，不改变原数组结构。
- `splice(start, deleteCount, ...items)`：删除、插入或替换，改变原数组。
- 现代环境还提供不原地修改数组的 `toSpliced`、`toSorted`、`toReversed` 等，使用时需按项目目标环境确认支持情况。

### 5.4 前端最常用的数组方法

统一使用这组数据：

```js
const products = [
  { id: 1, name: '键盘', price: 199, stock: 5 },
  { id: 2, name: '鼠标', price: 99, stock: 0 },
  { id: 3, name: '显示器', price: 899, stock: 2 }
];
```

| 方法 | 作用 | 返回值 | 是否自动改变原数组结构 |
| --- | --- | --- | --- |
| `forEach` | 对每项执行操作 | undefined | 否 |
| `map` | 把每项转换为新值 | 新数组 | 否 |
| `filter` | 保留满足条件的项 | 新数组 | 否 |
| `find` | 找第一项满足条件的值 | 该值或 undefined | 否 |
| `findIndex` | 找第一项满足条件的下标 | 下标或 -1 | 否 |
| `some` | 是否至少一项满足 | boolean | 否 |
| `every` | 是否所有项满足 | boolean | 否 |
| `reduce` | 累计处理 | 累加结果 | 否 |
| `sort` | 排序 | 原数组引用 | 是 |

```js
const names = products.map(product => product.name);
const inStock = products.filter(product => product.stock > 0);
const target = products.find(product => product.id === 2);
const hasSoldOut = products.some(product => product.stock === 0);
const allAvailable = products.every(product => product.stock > 0);
const total = products.reduce((sum, product) => sum + product.price, 0);
const sorted = [...products].sort((a, b) => a.price - b.price);
```

注意三个边界：

1. “不自动改变原数组”不代表回调里不能修改对象。`map(p => { p.price = 0; return p; })` 仍修改了共享对象。
2. `sort()` 不传比较函数时按字符串相关顺序排序，数字排序应写比较函数。
3. 空数组 `every(...)` 返回 true，`some(...)` 返回 false；`reduce` 对空数组且没有初始值时会报错。

```js
const numbers = [2, 10, 1];
console.log([...numbers].sort());              // [1, 10, 2]
console.log([...numbers].sort((a, b) => a - b)); // [1, 2, 10]
```

列表删除常见写法：

```js
let tasks = [{ id: 1, title: 'HTML' }, { id: 2, title: 'CSS' }];
tasks = tasks.filter(task => task.id !== 1);
```

这类写法以后会频繁出现在 Vue 的列表操作中。

### 5.5 解构赋值

```js
const user = { id: '1001', name: '小轩', age: 20 };
const { name, age } = user;
const { name: username } = user; // 重命名本地变量
const { city = '南京' } = user;  // 缺失或 undefined 时使用默认值

const [first, second] = ['HTML', 'CSS'];

function printUser({ name, age = 18 }) {
  console.log(name, age);
}
printUser(user);
```

解构不会自动深拷贝。嵌套对象被解构出来后，仍可能指向原来的对象。

```js
const source = { address: { city: '南京' } };
const { address } = source;
address.city = '苏州';
console.log(source.address.city); // '苏州'
```

解构 null 或 undefined 会出错。先处理缺失对象：

```js
const payload = null;
const { name = '匿名' } = payload ?? {};
```

这只处理 null/undefined，并不替代对任意外部数据的类型验证。

### 5.6 展开语法与剩余语法

```js
const a = [1, 2];
const b = [...a, 3];

const user = { name: '小轩', age: 20 };
const updatedUser = { ...user, age: 21 };

const { age, ...rest } = user;
```

| 位置 | 意义 |
| --- | --- |
| `[...arr]` | 展开可迭代值到数组 |
| `fn(...arr)` | 将数组等展开为实参 |
| `{ ...obj }` | 复制对象自身的可枚举属性 |
| `function fn(...args)` | 收集剩余参数 |
| `const { a, ...rest } = obj` | 收集剩余属性 |

顺序会影响对象合并结果：

```js
const a = { age: 18, ...{ age: 20 } }; // age 为 20
const b = { ...{ age: 20 }, age: 18 }; // age 为 18
```

### 5.7 赋值、浅拷贝、深拷贝

```js
const source = { name: '小轩', address: { city: '南京' } };
const same = source;
const shallow = { ...source };

console.log(source === same);            // true
console.log(source === shallow);         // false
console.log(source.address === shallow.address); // true
```

| 操作 | 外层是否新对象 | 嵌套对象是否独立 |
| --- | --- | --- |
| `const b = a` | 否 | 否 |
| `{ ...a }`、`Object.assign({}, a)` | 是 | 通常仍共享 |
| `structuredClone(a)` | 对支持的数据创建深层副本 | 支持类型范围内通常独立 |

```js
const deep = structuredClone(source);
deep.address.city = '杭州';
console.log(source.address.city); // '南京'
```

`structuredClone` 能处理许多常见数据类型和循环引用，但不能克隆任意值；函数、DOM 节点、Proxy 等不能当作普通业务数据随意克隆，也不要期待保留任意自定义类的全部语义。Vue 响应式代理对象不能直接套用普通对象克隆思路。

`JSON.parse(JSON.stringify(obj))` 有明显局限：会丢失或改变某些值，Date 会变字符串，循环引用和 BigInt 等会报错。它不是通用深拷贝工具。

参阅：[MDN：structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone)。

### 5.8 可选链与空值合并

```js
const user = { profile: null, points: 0 };
const city = user.profile?.address?.city ?? '未填写';
const points = user.points ?? 100; // 保留 0
console.log(city, points);
```

- `?.` 在左侧为 null 或 undefined 时短路并返回 undefined。
- 它不代表“捕获任何异常”。方法存在却不是函数，使用 `obj.method?.()` 仍可能报错。
- 根变量本身必须已声明；`notDeclared?.name` 仍会报 ReferenceError。
- 可选链结果不能作为普通赋值目标，例如不能写 `obj?.name = 'a'`。

### 5.9 JSON 与 JavaScript 对象

```js
const user = { name: '小轩', age: 20 };
const json = JSON.stringify(user);
const parsed = JSON.parse(json);
```

| JavaScript 对象 | JSON 文本 |
| --- | --- |
| 内存中的数据结构 | 一种文本数据交换格式 |
| 属性名可在语法允许时不加引号 | 属性名必须使用双引号 |
| 可以包含函数、undefined 等 | 标准 JSON 不包含函数、undefined、注释等 |
| 可使用单引号字符串等 JS 语法 | 字符串使用双引号，不能有尾逗号 |

后端返回 JSON，经 `response.json()` 解析后，前端得到的是 JS 值。不要对已经解析好的对象再次 `JSON.parse`。

### 5.10 Set、Map、日期与正则

```js
const unique = [...new Set([1, 1, 2, 3])];
console.log(unique); // [1, 2, 3]

const scores = new Map();
scores.set('HTML', 90);
scores.set('CSS', 85);
console.log(scores.get('HTML'));
console.log(scores.has('CSS'));

const now = new Date();
console.log(now.toISOString()); // UTC 时间表示

const simpleCode = /^[A-Z]{2}\d{4}$/;
console.log(simpleCode.test('AB1234')); // true
```

- Set 适合去重、成员判断。不同对象即使内容相同，也不会因此自动合并。
- Map 的键可以是对象等多种值，API 与普通对象不同。
- 日期字符串应明确格式和时区；日期展示优先考虑 `Intl.DateTimeFormat` 等方案。
- `new Date(year, monthIndex, day)` 的月份从 0 开始。
- 正则 `^`、`$` 常用于限定整个字符串；`\d` 匹配数字；`{4}` 表示重复四次。
- 不要指望一条简易正则完成所有真实邮箱或电话号码的业务校验。

### 5.11 this：取决于调用方式

```js
const user = {
  name: '小轩',
  sayName() {
    return this.name;
  }
};
console.log(user.sayName()); // '小轩'

const say = user.sayName;
// say(); // 丢失 user 这个调用接收者；严格模式中 this 为 undefined
const boundSay = user.sayName.bind(user);
console.log(boundSay()); // '小轩'
```

| 调用方式 | 普通函数中的 this 常见结果 |
| --- | --- |
| `obj.fn()` | obj |
| 严格模式下 `fn()` | undefined |
| `fn.call(obj, a)` | 显式指定为 obj |
| `fn.apply(obj, [a])` | 显式指定为 obj，参数数组形式 |
| `fn.bind(obj)` | 返回绑定 this 的新函数 |
| `new Fn()` | 新建的实例对象，受构造函数返回规则影响 |

箭头函数没有自己的 this，捕获外围的 this，也不能作为构造函数。`call`、`apply`、`bind` 不能把箭头函数的 this 重新绑定成另一个接收者。

因此“箭头函数更简洁”不等于“任何方法都应写箭头函数”。在 Vue 组合式 API 中通常不依赖组件 this；以后学习选项式 API 时，要特别注意 methods 等位置的 this 规则。

### 5.12 作用域、闭包与声明提升

作用域决定变量在哪些位置可访问。常见有全局作用域、模块作用域、函数作用域和块级作用域。内部作用域可以沿词法作用域链查找外层变量，外层通常不能反过来直接访问内部局部变量。

闭包是函数及其可访问的词法环境的组合。实际效果是：外层函数返回后，内部函数仍能访问相关变量。

```js
function createCounter() {
  let count = 0;
  return function () {
    count += 1;
    return count;
  };
}

const counterA = createCounter();
const counterB = createCounter();
console.log(counterA()); // 1
console.log(counterA()); // 2
console.log(counterB()); // 1，独立环境
```

闭包常用于状态封装、回调、事件处理和组合函数。长期保留的闭包也可能延长被引用对象的生命周期，应注意清理不再需要的监听和定时器。

声明提升的实用理解：

```js
hello(); // 函数声明通常可在前面调用
function hello() { console.log('hello'); }

console.log(value); // undefined
var value = 10;

// console.log(score); // ReferenceError
let score = 90;
```

不要把提升理解成“源代码真的被搬到了顶部”。它是对绑定在执行前如何建立的概括。

### 5.13 原型与 class：先掌握够用的范围

对象可以通过原型链访问自身没有的属性。`class` 提供更清晰的类语法，但 JavaScript 的对象模型并没有变成 Java 的类模型。

```js
class Task {
  constructor(title) {
    this.title = title;
    this.done = false;
  }

  complete() {
    this.done = true;
  }
}

const task = new Task('学习 Vue');
task.complete();
console.log(task.done); // true
console.log(Object.getPrototypeOf(task) === Task.prototype); // true
```

先认识 `constructor`、实例方法、`extends`、`super`、`static` 即可。学习 Vue 前不必先穷尽原型链面试题，但应该理解对象方法不一定都存放在对象自身上。

---

## 第6章 浏览器编程：DOM、事件与本地存储

### 6.1 DOM 是什么

浏览器解析 HTML 后，会建立表示文档的对象树，即 DOM。JavaScript 可以通过 DOM API 读取和修改页面。

例如 `<ul><li>HTML</li><li>CSS</li></ul>` 中，ul 是 li 的父元素，两个 li 是兄弟元素。

常见对象包括 `document`、元素节点、文本节点。`children` 只包含元素子节点，`childNodes` 还包含文本等其他节点；源码中的空白也可能产生文本节点。

### 6.2 选择元素

```js
const title = document.querySelector('.title');
const buttons = document.querySelectorAll('button');
const app = document.getElementById('app');

if (title) {
  title.textContent = '开始学习 Vue';
}

buttons.forEach(button => {
  console.log(button.textContent);
});
```

| 方法 | 返回结果 |
| --- | --- |
| `querySelector(selector)` | 第一个匹配元素或 null |
| `querySelectorAll(selector)` | 静态 NodeList，没有匹配时为空 |
| `getElementById(id)` | 对应元素或 null |
| `element.closest(selector)` | 从自身向祖先寻找第一个匹配元素或 null |
| `element.matches(selector)` | 当前元素是否匹配 |

先确认是否选到元素，再使用属性。`Cannot read properties of null` 常见原因是选择器写错，或代码执行时对应元素尚未解析出来。

### 6.3 文本、HTML、属性与样式

```js
const box = document.querySelector('#box');
if (box) {
  box.textContent = '这是一段纯文本';
  box.classList.add('active');
  box.classList.toggle('loading', false);
  box.style.backgroundColor = '#f3f4f6';
  box.setAttribute('aria-label', '内容区域');
}
```

| API | 用途 | 注意 |
| --- | --- | --- |
| `textContent` | 读取/设置文本内容 | 设置时不会把字符串解析为 HTML |
| `innerHTML` | 读取/设置 HTML 内容 | 不可信字符串可能形成 XSS |
| `innerText` | 更接近页面渲染文本的语义 | 受布局与样式影响，读取可能触发布局工作 |
| `classList` | 管理 class | 优先于到处拼接 className 字符串 |
| `style` | 管理行内样式 | 大量静态样式仍应放到 CSS |
| `getComputedStyle` | 读取解析后的样式值 | 区别于只查看行内 style |
| `dataset` | 读取 data-* | 读取结果通常是字符串 |

```html
<button id="view" type="button" data-user-id="42">查看</button>
```

```js
const view = document.querySelector('#view');
console.log(view.dataset.userId); // '42'
```

`data-user-id` 在 JS 中对应 `dataset.userId`。

HTML 属性 attribute 与 DOM 属性 property 不完全相同。例如 input 的 `value` attribute 常描述初始/默认值，而 `input.value` 表示当前值；`getAttribute('checked')` 与 `input.checked` 也不能混同。

### 6.4 创建、插入和删除元素

```js
const list = document.querySelector('#list');
if (list) {
  const item = document.createElement('li');
  item.textContent = '学习 JavaScript';
  list.append(item);
  // item.remove(); // 从 DOM 中移除
}
```

批量构造可先使用 `DocumentFragment`：

```js
const list = document.querySelector('#list');
if (list) {
  const fragment = document.createDocumentFragment();
  for (const name of ['HTML', 'CSS', 'JavaScript']) {
    const item = document.createElement('li');
    item.textContent = name;
    fragment.append(item);
  }
  list.replaceChildren(fragment);
}
```

这能让构造过程清晰，不能据此宣称任何情况下都必然性能最好。真实项目先保证正确，再按测量结果优化。

### 6.5 事件监听与移除

```js
const button = document.querySelector('#save');

function handleSave(event) {
  console.log('点击了保存', event.type);
}

if (button) {
  button.addEventListener('click', handleSave);
  // 不再需要时：
  // button.removeEventListener('click', handleSave);
}
```

移除监听时，需要使用同一个函数引用，以及匹配的捕获设置。两次书写 `() => {}` 得到的是不同函数，不能互相移除。

常见事件：

| 事件 | 场景 |
| --- | --- |
| `click` | 点击、键盘触发原生按钮等激活行为 |
| `input` | 输入值随用户编辑变化 |
| `change` | 控件值提交/确认变化，具体时机依控件而定 |
| `submit` | 表单提交 |
| `keydown`、`keyup` | 键盘按键 |
| `focus`、`blur` | 聚焦、失焦，通常不冒泡 |
| `focusin`、`focusout` | 可冒泡的焦点事件 |
| `pointerdown` 等 | 鼠标、触控、笔等指针输入 |
| `scroll`、`resize` | 滚动与窗口尺寸变化 |

通过 JS 设置 `input.value`，通常不会自动触发一次用户 input 事件。

### 6.6 捕获、目标与冒泡

当点击嵌套元素时，事件常经历：

1. 捕获阶段：沿祖先向目标传播。
2. 目标阶段：到达实际目标。
3. 冒泡阶段：对于支持冒泡的事件，从目标向祖先传播。

默认 `addEventListener` 通常监听非捕获阶段，第三个参数传 `{ capture: true }` 可监听捕获阶段。

```js
parent.addEventListener('click', event => {
  console.log(event.target);        // 事件最初的目标；复杂场景可能重定向
  console.log(event.currentTarget); // 当前正在执行此监听的 parent
});
```

上例中的 parent 需先选取为真实 DOM 元素。

| 方法 | 作用 |
| --- | --- |
| `preventDefault()` | 取消可取消事件的默认行为，例如表单导航提交 |
| `stopPropagation()` | 阻止继续向其他节点传播，不自动取消默认行为 |
| `stopImmediatePropagation()` | 进一步阻止当前节点后续监听器执行 |

两种常见误区：阻止冒泡不会自动阻止链接跳转；阻止默认行为不会自动阻止父元素监听器执行。被动监听器中不能用 preventDefault 达到取消默认行为的目的。

### 6.7 事件委托

当列表很多项，或项目动态增加时，可以给父容器绑定一次监听，根据事件目标定位操作元素。

```html
<ul id="task-list">
  <li><span>学习 HTML</span><button type="button" data-id="1">删除</button></li>
  <li><span>学习 CSS</span><button type="button" data-id="2">删除</button></li>
</ul>
```

```js
const taskList = document.querySelector('#task-list');
taskList.addEventListener('click', event => {
  if (!(event.target instanceof Element)) return;
  const button = event.target.closest('button[data-id]');
  if (!button || !taskList.contains(button)) return;
  console.log('删除任务', button.dataset.id);
  button.closest('li')?.remove();
});
```

使用 closest 可以兼容按钮内部还有图标、span 的情况。事件委托依赖对应事件的传播特性，不能假定每一种事件都适合原样套用。

### 6.8 表单用 JS 接管

```html
<form id="search-form">
  <label for="keyword">关键词</label>
  <input id="keyword" name="keyword" required>
  <button type="submit">搜索</button>
</form>
<p id="search-result" role="status"></p>
```

```js
const form = document.querySelector('#search-form');
const result = document.querySelector('#search-result');

form.addEventListener('submit', event => {
  event.preventDefault();
  const formData = new FormData(form);
  const keyword = String(formData.get('keyword') ?? '').trim();
  result.textContent = keyword ? `准备搜索：${keyword}` : '请输入关键词';
});
```

优先监听 form 的 submit，而不是只监听按钮 click，这样按回车提交也能进入同一套逻辑。原生表单约束校验不通过时，一般不会进入正常 submit 事件流程。

### 6.9 脚本加载时机

| 写法 | 常见行为 |
| --- | --- |
| 普通外部 script | 解析器插入的经典外部脚本通常暂停 HTML 解析，等待下载和执行完成 |
| 外部 script + defer | 并行下载，文档解析完成后按文档顺序执行 |
| 外部 script + async | 并行下载，准备好后尽快执行，不保证多个脚本顺序 |
| `type="module"` | 默认具有延后执行特性，依赖模块机制，自动严格模式 |

上述是初学常见场景；动态插入、模块 async、顶层 await 等还有额外规则。

```js
// 若脚本可能在不同阶段加载，可这样等待 DOM 可用
function init() {
  console.log('现在可以查找页面元素');
}

if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', init, { once: true });
} else {
  init();
}
```

`DOMContentLoaded` 不等于所有图片等资源都已加载完成；`load` 关注更完整的资源加载。一般 DOM 初始化不用等到整页图片都加载完。

### 6.10 BOM、定时器与 URL

BOM 是对浏览器窗口相关 API 的传统统称，常见对象有 window、location、history、navigator。

```js
console.log(location.href);
const url = new URL(location.href);
console.log(url.searchParams.get('keyword'));

const timeoutId = setTimeout(() => {
  console.log('稍后执行');
}, 1000);
// clearTimeout(timeoutId);

const intervalId = setInterval(() => {
  console.log('周期执行');
}, 1000);
// 不需要时 clearInterval(intervalId);
```

定时器延迟不是精确定时保证。主线程繁忙、后台标签页节流等都可能让实际执行更晚。定时轮询还应处理上次请求未结束又触发下一次的问题。

`location.assign(url)` 会导航并通常保留历史项，`location.replace(url)` 通常替换当前历史项。`history.pushState` 可更新同源 URL 和历史，但不会自动帮你渲染新页面；路由库会封装相关行为。

### 6.11 localStorage、sessionStorage 与 Cookie

| 机制 | 主要特点 | 典型用途 |
| --- | --- | --- |
| localStorage | 按源隔离，通常跨会话保留，键值为字符串，同步 API | 主题、少量本地偏好或练习数据 |
| sessionStorage | 按源及顶层页面会话隔离，刷新通常保留，标签页结束通常清除 | 单页会话内临时数据 |
| Cookie | 依规则随匹配的 HTTP 请求发送，可配置 HttpOnly、Secure、SameSite 等 | 会话与服务端相关状态 |
| IndexedDB | 异步的结构化数据存储，容量和能力更强 | 较大量离线数据等 |

```js
localStorage.setItem('theme', 'dark');
console.log(localStorage.getItem('theme')); // 不存在时返回 null
localStorage.removeItem('theme');

const preferences = { fontSize: 16, theme: 'light' };
localStorage.setItem('preferences', JSON.stringify(preferences));

function readPreferences() {
  try {
    const raw = localStorage.getItem('preferences');
    return raw === null ? null : JSON.parse(raw);
  } catch {
    return null;
  }
}
```

必须知道的边界：

- `JSON.parse` 成功只说明是有效 JSON，不说明结构符合业务，需要继续校验字段类型。
- 存储可能因配额、用户策略、隐私环境等失败，写入也要考虑异常。
- 本地存储不是数据库备份，用户清理数据后可能丢失。
- 同源其他标签页的 localStorage 修改可通过 storage 事件获知；修改所在页面自身通常不收到这次 storage 事件。
- JavaScript 可读取的存储并不是放置敏感凭据的天然安全区域；认证存储应由整体安全方案决定。
- 不要机械记死“所有浏览器都是固定 5MB”，配额与行为取决于实现和模式。

参阅：[MDN：Web Storage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)。

---

## 第7章 异步编程、HTTP 请求与跨域

### 7.1 同步、异步与事件循环

同步代码通常按当前执行顺序完成。异步操作允许发起工作后，稍后再处理结果，例如网络响应、定时器、用户交互。

浏览器页面主线程上的普通 JS 在同一时刻执行一段代码。网络、渲染、Worker 等能力由宿主环境协作提供，不能把“JS 常说是单线程”理解为“整个浏览器只有一个线程”。

```js
console.log('A');
setTimeout(() => console.log('B'), 0);
console.log('C');
// 输出 A、C、B
```

0 毫秒也不代表立即打断当前代码。长时间同步计算会阻塞交互；把耗时计算包在 Promise 的执行器里，也不会自动把计算移到另一个线程。

### 7.2 Promise 的状态与基本用法

Promise 表示未来才可能得到的结果，状态包括 pending、fulfilled、rejected。一次状态确定后不会再转换成另一种终态。

```js
function wait(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

wait(500)
  .then(() => {
    console.log('等待完成');
    return '下一步结果';
  })
  .then(value => console.log(value))
  .catch(error => console.error(error))
  .finally(() => console.log('清理结束'));
```

- `new Promise(executor)` 的执行器同步执行。
- then 中的回调不会在注册时同步执行。
- then 返回新的 Promise；返回普通值会传给下一步，返回 Promise 会等待它的结果。
- 在 then 中抛错会导致返回的 Promise 拒绝。
- catch 如果正常返回而不重新抛错，后续链通常视为已恢复。
- finally 通常用于清理；若其中抛错或返回拒绝的 Promise，也会改变最终结果。

### 7.3 微任务与任务的基本顺序

```js
console.log(1);

setTimeout(() => console.log(2), 0);

Promise.resolve().then(() => console.log(3));

console.log(4);
// 输出 1、4、3、2
```

初学可采用这个模型：当前同步代码完成后，在相应的微任务检查点清空微任务，再继续处理后续任务。Promise 回调、queueMicrotask 等属于常见微任务来源，定时器回调属于任务。

微任务执行时新加入的微任务也会继续处理，因此无限追加微任务可能饿死页面其他工作。浏览器不承诺每执行一个任务就一定绘制一次画面；完整渲染调度比这段入门模型更复杂。

参阅：[MDN：JavaScript 执行模型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model)。

### 7.4 async 与 await

```js
function wait(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function run() {
  console.log('开始');
  await wait(300);
  console.log('结束');
  return 100;
}

run().then(value => console.log(value));
```

- async 函数总是返回 Promise，即使 return 的是普通值。
- await 暂停当前 async 函数后续执行，让调用方继续运行，不阻塞整个主线程。
- await 普通值也会使后续部分延后到异步继续执行的时机。
- 普通函数体内不能随意使用 await；可以在 async 函数中使用，支持的 ES 模块也可使用顶层 await。
- await 遇到拒绝会抛出对应错误，配合 try/catch 处理。

```js
async function loadData() {
  try {
    const result = await Promise.resolve({ id: 1 });
    console.log(result);
  } catch (error) {
    console.error(error);
  } finally {
    console.log('关闭加载提示');
  }
}
loadData();
```

### 7.5 串行与并发

```js
async function serial() {
  const a = await getA();
  const b = await getB(a); // B 依赖 A，应该串行
  return b;
}

async function parallel() {
  const [users, categories] = await Promise.all([
    getUsers(),
    getCategories()
  ]); // 两个独立请求可并发发起
  return { users, categories };
}
```

上面的 getA、getB 等是业务函数占位，需要自己实现。

| API | 适合场景 | 关键行为 |
| --- | --- | --- |
| `Promise.all` | 所有结果都需要 | 任一个拒绝，整体拒绝；结果顺序按输入排列 |
| `Promise.allSettled` | 每项成功失败都要记录 | 等全部结束，得到各项状态 |
| `Promise.race` | 取最先落定的结果 | 最先成功或失败的项决定结果 |
| `Promise.any` | 任一成功即可 | 全部拒绝时整体才拒绝 |

Promise.all 整体拒绝，不等于其他请求被自动取消。是否真正取消需使用对应 API 的取消机制，例如 AbortController。

不要用 `forEach(async item => ...)` 后就以为外层会等待全部操作。需要依次执行用 for...of + await；需要并发用 map 生成 Promise 数组再 Promise.all，并考虑请求数量限制。

### 7.6 HTTP 联调基础

一次 HTTP 请求通常包括方法、URL、请求头和可选请求体；响应包括状态码、响应头和响应体。

| 请求方式 | 常见用途 |
| --- | --- |
| GET | 查询资源 |
| POST | 创建资源或提交操作 |
| PUT | 常表达对资源的整体替换 |
| PATCH | 常表达部分更新 |
| DELETE | 删除资源 |

具体接口语义以 API 契约为准。状态码常见含义：

| 状态码 | 一般含义 |
| --- | --- |
| 200 | 请求成功 |
| 201 | 成功创建资源 |
| 204 | 成功但没有响应内容 |
| 400 | 请求不符合要求 |
| 401 | 缺少有效认证信息等认证问题 |
| 403 | 访问被拒绝 |
| 404 | 未找到资源或路由 |
| 409 | 资源状态冲突 |
| 500 | 服务端错误 |

HTTP 成功不保证业务成功。有些项目统一返回 HTTP 200，再通过 JSON 内的 code 判断业务结果；这必须看后端约定，不能认定所有项目都用同一个成功码。

### 7.7 fetch 查询接口

需要后端提供 `GET /api/products`，并返回 JSON：

```js
async function getProducts(keyword) {
  const params = new URLSearchParams({ keyword, page: '1' });
  const response = await fetch(`/api/products?${params}`);

  if (!response.ok) {
    throw new Error(`查询失败，HTTP ${response.status}`);
  }

  return await response.json();
}

getProducts('键盘')
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

要点：

- fetch 通常不会因为 HTTP 404、500 自动拒绝，需要检查 response.ok 或 status。
- 网络失败、部分跨域问题、请求取消等可能导致拒绝。
- `response.json()` 是异步解析，不是直接拿到属性。
- 响应体通常只能读取一次；确需多份读取可在消费前考虑 clone。
- 收到 204、空响应体或非 JSON 内容时，不要一律调用 json。
- 不要用字符串随意拼接含特殊字符的查询参数，URLSearchParams 可以处理编码。

详细行为：[MDN：使用 Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)。

### 7.8 POST JSON 与上传文件

需要后端提供对应接口：

```js
async function createTask(title) {
  const response = await fetch('/api/tasks', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ title })
  });
  if (!response.ok) throw new Error(`创建失败：${response.status}`);
  return response.status === 204 ? null : await response.json();
}
```

Java 后端如果使用 Spring MVC，接收这种 JSON 时常使用 `@RequestBody`，还应与字段名、类型和校验规则保持一致。

上传文件时使用 FormData：

```js
async function uploadFile(file) {
  const data = new FormData();
  data.append('file', file);

  const response = await fetch('/api/upload', {
    method: 'POST',
    body: data
  });
  if (!response.ok) throw new Error(`上传失败：${response.status}`);
  return response.status === 204 ? null : await response.json();
}
```

不要在这个 FormData 例子中手动设置 `Content-Type: multipart/form-data`，浏览器需要自动加入正确的 boundary。

### 7.9 加载、错误、空数据与重复提交

发起请求时，页面至少应考虑以下状态：

| 状态 | 页面处理 |
| --- | --- |
| 初始 | 显示提示或默认数据 |
| 加载中 | 展示加载反馈，必要时禁用提交按钮 |
| 成功且有数据 | 展示内容 |
| 成功但无数据 | 展示空状态 |
| 失败 | 展示可理解的错误和重试入口 |

通用逻辑框架：

```js
async function submit() {
  if (loading) return;
  loading = true;
  errorMessage = '';
  try {
    result = await saveData();
  } catch (error) {
    errorMessage = error instanceof Error ? error.message : '保存失败';
  } finally {
    loading = false;
  }
}
```

这是状态逻辑示意，loading、errorMessage、result、saveData 需要由实际代码声明和实现。原生 JS 还需要主动渲染状态，Vue 会帮助建立状态与界面之间的联系。

前端禁用按钮主要改善体验。支付、下单等关键业务仍需后端实现合理的幂等和重复提交控制。

### 7.10 取消请求与响应乱序

```js
async function loadWithTimeout() {
  const controller = new AbortController();
  const timer = setTimeout(() => controller.abort(), 5000);

  try {
    const response = await fetch('/api/products', {
      signal: controller.signal
    });
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    return await response.json();
  } finally {
    clearTimeout(timer);
  }
}
```

调用方需要处理取消或其他失败。前端取消等待不等于后端一定撤销已执行的业务操作。

搜索框常有乱序问题：用户先搜 A 再搜 B，A 的响应却更晚到达，最后错误显示了 A。可以取消旧请求，或用递增请求编号，只接受当前最新请求的结果。

```js
let latestRequest = 0;

async function search(keyword) {
  const requestId = ++latestRequest;
  const response = await fetch(`/api/search?${new URLSearchParams({ keyword })}`);
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  const data = await response.json();
  if (requestId !== latestRequest) return;
  // 此处才把 data 渲染到页面；调用方仍需处理失败
  console.log(data);
}
```

### 7.11 同源策略与 CORS

源 origin 主要由协议、主机、有效端口共同决定。

以页面地址 `http://localhost:5173` 为基准：

| 请求目标 | 是否同源 | 原因 |
| --- | --- | --- |
| `http://localhost:5173/api/users` | 是 | 协议、主机、端口一致 |
| `http://localhost:8080/api/users` | 否 | 端口不同 |
| `http://127.0.0.1:5173/api/users` | 否 | 主机不同 |
| `https://localhost:5173/api/users` | 否 | 协议不同 |

这解释了为什么“localhost 能访问，改成 127.0.0.1 却不行”：它们可能指向同一台机器，但浏览器不会因此把两者视作同一个源。后端 CORS 白名单、Cookie、存储、监听绑定地址等也可能不同。具体原因要结合 Console 和 Network 判断，不能只凭地址变化就断言一定是 CORS。

浏览器会对跨源脚本读取等行为实施限制。不是“所有跨源资源都完全不能加载”，普通图片嵌入、导航等有各自规则。

常见解决方式：

1. 后端允许明确的前端来源，并正确处理需要的请求头、方法和预检。
2. 开发环境配置代理：浏览器请求前端开发服务器，再由开发服务器转发。
3. 生产环境通过反向代理，让前端与 API 对浏览器表现为同源。

把 `Access-Control-Allow-Origin` 加到前端请求头里，不能让后端自动授权跨域。fetch 的 `mode: 'no-cors'` 也不能让 JS 正常读取原本不被允许的 JSON 响应，通常得到不可读的 opaque 响应。

某些跨域请求会先发 OPTIONS 预检，例如带 application/json 的 POST、使用特定自定义头等；并不是所有请求都有预检。简单请求可能已经被发到服务器，只是响应不允许被前端读取。

参阅：[MDN：CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)。

### 7.12 跨域 Cookie 与认证

```js
// 需要后端和 Cookie 策略一起配合
fetch('https://api.example.com/profile', {
  credentials: 'include'
});
```

fetch 的 credentials 默认是 same-origin。跨源携带 Cookie 时常需 include，同时服务器正确返回允许来源与凭据的 CORS 响应头。

注意：

- 携带凭据的 CORS 场景不能简单使用 `Access-Control-Allow-Origin: *`。
- Cookie 是否发送还受 Domain、Path、Secure、SameSite 及浏览器第三方 Cookie 策略等影响。
- 同源 origin 与同站 site 不是同一概念。
- Cookie 会自动发送，认证方案需考虑 CSRF；将令牌放在 JS 可读存储中又需考虑 XSS 等风险。
- CORS 是浏览器的访问控制机制，不是后端用户认证或权限校验。

---

## 第8章 模块化、工程基础、安全与性能

### 8.1 ES 模块：export 与 import

当代码增多，不应该把所有函数塞进一个文件。ES 模块用导出与导入明确依赖。

文件 `utils.js`：

```js
export function formatPrice(price) {
  return `¥${price.toFixed(2)}`;
}

export const appName = '学习商城';

export default function greet(name) {
  return `你好，${name}`;
}
```

文件 `main.js`：

```js
import greet, { formatPrice, appName } from './utils.js';
console.log(appName, greet('同学'), formatPrice(19.9));
```

HTML 中：

```html
<script type="module" src="./main.js"></script>
```

| 导出方式 | 导入方式 | 规则 |
| --- | --- | --- |
| 命名导出 `export const a = 1` | `import { a } from './x.js'` | 名称需对应，可用 as 重命名 |
| 默认导出 `export default value` | `import anyName from './x.js'` | 本地名字可自行命名 |
| 命名重命名导入 | `import { a as value } from './x.js'` | 修改当前模块中的名称 |
| 命名空间导入 | `import * as utils from './x.js'` | 通过 utils.a 等访问 |

一个模块可以有多个命名导出，至多一个默认导出。

注意：

- 原生浏览器模块通常要写相对路径和扩展名，如 `./utils.js`。
- `import { ref } from 'vue'` 这样的包名导入，需要构建工具、import map 等机制帮助解析，不能假定浏览器天然知道依赖位置。
- `@/utils` 常是项目别名，需要配置，不是 JS 原生固定语法。
- 模块有自己的作用域，自动使用严格模式。
- 导入绑定不是随意可重赋值的局部变量；被导出对象的可变性是另一件事。
- 同一模块在同一模块环境中通常只求值一次，不会因为多处导入就把初始化逻辑重复执行。
- 使用本地 HTTP 服务器测试模块，避免直接依赖 file://。

动态加载：

```js
async function loadUtils() {
  const utils = await import('./utils.js');
  console.log(utils.formatPrice(10));
}
```

实际 Vue 项目常使用动态 import 实现路由页面按需加载，稍后再学具体框架配置。

参阅：[MDN：JavaScript 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)。

### 8.2 ES6、ES2015 与“现代 JS”

ES6 对应 ECMAScript 2015，加入了 let/const、箭头函数、类、模块、Promise 等重要能力。后续标准继续按年份演进。

可选链、空值合并等并不都属于 ES2015。不要把所有后来出现的语法都严格称为 ES6；日常“补 ES6”通常只是泛指补现代 JS。

新语法能否在项目中使用，要结合目标浏览器、编译转换与运行时 API 支持情况。语法转换和补齐运行时 API 是不同问题。

### 8.3 package.json、依赖与构建

| 文件或目录 | 作用 |
| --- | --- |
| `package.json` | 项目元信息、依赖声明、脚本 |
| 锁文件 | 记录更精确的依赖解析结果，帮助复现安装 |
| `node_modules` | 本地安装的依赖文件 |
| `src` | 源代码目录，具体名称由项目约定 |
| `public` | 某些工具中的静态资源目录，规则依工具而定 |
| `dist` | 常见构建产物目录名，不是语言强制规定 |

```json
{
  "name": "frontend-learning",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

这里只展示脚本概念；要运行这些脚本，项目必须实际安装并配置对应工具。不要只复制这一段 JSON 就认为依赖已经存在。

- `npm install`：按项目描述安装依赖，具体行为还会受锁文件影响。
- `npm run dev`：运行项目自己定义的 dev 脚本。
- `npm run build`：生成构建产物。
- dev、build 不是固定内置业务命令，而是 scripts 中的名称。
- 开发服务器、构建产物、本地预览与生产部署是不同阶段。

Node.js 在常见 Vue 工程中主要运行开发和构建工具，不意味着上线后必须由 Node.js 负责业务后端；Java 后端完全可以继续承担业务。

### 8.4 前端环境变量与配置

前端工程可以使用环境变量区分 API 地址等配置，但最终发给浏览器的代码和数据可被用户查看。

不要把数据库密码、服务端私钥、需要保密的第三方密钥放进前端代码或可打包的环境变量中。环境变量名和读取方式依构建工具决定，不能把后端的保密方式直接套到浏览器端。

### 8.5 XSS 与安全输出

```js
// 用户输入或接口数据可能不可信
const content = '<img src=x onerror=alert(1)>';

// 危险：如果进入可执行 HTML 上下文，可能造成 XSS
// element.innerHTML = content;

// 需要展示文字时，使用文本 API
const element = document.createElement('p');
element.textContent = content;
```

XSS 的核心是把不可信数据当作可执行的代码或 HTML 解释。普通文本用 textContent；确需富文本时，使用适合该场景、维护可靠的净化方案和明确的允许规则，不要自制几次字符串替换就认为处理完整。

Vue 普通文本插值会按文本处理；使用 `v-html` 等原始 HTML 能力时仍需要理解内容可信边界。前端框架不会自动替你解决所有安全问题。

### 8.6 权限、认证与 CSRF

- 登录认证回答“你是谁”，权限校验回答“你能做什么”。
- 前端隐藏删除按钮只改善展示，后端接口必须检查真实权限。
- CSRF 常利用浏览器自动携带认证信息的行为发起非用户本意的请求；应由完整的前后端方案处理，例如合理的 SameSite、CSRF token、来源校验等。
- 不要使用 GET 接口执行删除、转账等状态变更操作。
- 不要将 HTTPS、POST、CORS、登录四者当成同一层防护。

### 8.7 防抖与节流

| 技术 | 行为 | 例子 |
| --- | --- | --- |
| 防抖 debounce | 一段时间内持续触发时推迟，停止后再执行 | 搜索输入停止后再发请求 |
| 节流 throttle | 限制单位时间内执行频率，具体首尾行为依实现 | 高频滚动统计、拖动反馈 |

一个仅在最后一次触发后执行的防抖实现：

```js
function debounce(fn, delay) {
  let timer;
  function debounced(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  }
  debounced.cancel = () => clearTimeout(timer);
  return debounced;
}

const logKeyword = debounce(keyword => {
  console.log('执行搜索', keyword);
}, 300);

logKeyword('V');
logKeyword('Vu');
logKeyword('Vue'); // 前两次尚未执行时被合并，最后输出 Vue
```

这个实现用于理解原理，不包含成熟库可能提供的 leading、maxWait、flush 等功能。移除监听或销毁组件时，也要清理相关定时器。

防抖减少请求次数，不自动解决已发出请求的响应乱序；两者需要分别处理。

### 8.8 实用性能习惯

1. 避免一次渲染特别庞大的列表，需要时分页或虚拟列表。
2. 避免在循环中反复交替读取布局尺寸、写入样式，减少强制布局的机会。
3. 图片使用合适尺寸、格式和加载策略。
4. 不要让长期监听器、定时器保留大量无用状态。
5. 独立请求可并发，但应限制无界并发。
6. 使用 requestAnimationFrame 协调需要跟随渲染的动画更新，不能把它当作耗时计算加速器。
7. 优先用开发者工具定位真实瓶颈，不凭感觉把每行代码改得难懂。

### 8.9 前后端联调排错表

| 现象 | 优先检查 |
| --- | --- |
| 页面不显示内容 | Console 首个错误、HTML 结构、初始化时机 |
| CSS 没生效 | Network 是否加载成功、选择器、层叠、缓存 |
| JS 文件 404 | 引用路径、服务器静态资源位置 |
| 点击按钮页面刷新 | button type、form submit、preventDefault |
| 请求根本没发出 | 事件是否触发、校验是否拦截、代码是否提前报错 |
| 请求返回 400 | 参数名、类型、请求体格式、必填字段 |
| 返回 401/403 | 会话或令牌、后端认证及权限、预检是否被错误拦截 |
| 后端收到空参数 | Content-Type、JSON 与表单方式、后端接收注解 |
| 接口成功但页面空白 | 数据层级、空值、类型、渲染逻辑 |
| JSON 解析失败 | 真实响应是否为空、HTML 错误页或其他文本 |
| 跨域报错 | Origin、OPTIONS、响应头、凭据、代理配置 |
| 刷新前端子路由 404 | 生产服务器是否配置 SPA 回退；排除真实 API/静态文件路由 |
| 列表重复或数据回退 | 重复绑定事件、请求乱序、状态覆盖 |

Network 里至少会看：Request URL、Method、Status、Query String、Request Payload、Response、Headers。

---

## 第9章 综合实战：学习任务清单

### 9.1 练习目标与运行方式

本例不需要后端，也不依赖第三方库。它将练习：

- HTML 语义、表单、label 和原生控件。
- CSS 盒模型、Flex、响应式与焦点样式。
- JS 数组操作、事件委托、状态与渲染。
- localStorage 序列化、数据校验和异常处理。

创建一个目录，把下面三个代码块分别保存为 `index.html`、`style.css`、`main.js`，然后用第1章的本地 HTTP 服务器运行。不要把三个代码块全部粘到同一个 JS 文件。

### 9.2 index.html

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>学习任务清单</title>
    <link rel="stylesheet" href="./style.css">
    <script src="./main.js" defer></script>
  </head>
  <body>
    <main class="app">
      <h1>学习任务清单</h1>
      <p class="intro">把今天要学的内容记下来，一项项完成。</p>

      <form id="task-form" class="add-form">
        <label for="title-input">新任务</label>
        <div class="add-row">
          <input id="title-input" name="title" required maxlength="100"
                 autocomplete="off" placeholder="例如：掌握 Flex 布局">
          <button type="submit">添加</button>
        </div>
      </form>

      <div class="filters">
        <div class="field">
          <label for="keyword-filter">搜索任务</label>
          <input id="keyword-filter" type="search" placeholder="输入关键词">
        </div>
        <div class="field">
          <label for="status-filter">任务状态</label>
          <select id="status-filter">
            <option value="all">全部</option>
            <option value="todo">未完成</option>
            <option value="done">已完成</option>
          </select>
        </div>
      </div>

      <p id="stats" role="status"></p>
      <p id="feedback" role="status"></p>
      <ul id="task-list" class="task-list" aria-label="任务列表"></ul>
    </main>
  </body>
</html>
```

### 9.3 style.css

```css
:root {
  font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
  color: #1f2937;
  background: #f3f6fb;
  line-height: 1.6;
}

*, *::before, *::after { box-sizing: border-box; }
body { margin: 0; }

.app {
  width: min(100% - 32px, 760px);
  margin: 40px auto;
  padding: 28px;
  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 16px;
}

h1 { margin: 0 0 8px; font-size: 1.8rem; }
.intro { margin: 0 0 24px; color: #64748b; }
label { display: block; margin-bottom: 6px; }
input, select, button { font: inherit; }
input:not([type="checkbox"]), select {
  width: 100%;
  min-width: 0;
  min-height: 44px;
  padding: 8px 12px;
  border: 1px solid #94a3b8;
  border-radius: 8px;
  background: white;
  color: inherit;
}
button {
  min-height: 44px;
  padding: 8px 16px;
  border: 0;
  border-radius: 8px;
  color: white;
  background: #2563eb;
  cursor: pointer;
}
button:hover { background: #1d4ed8; }
:focus-visible { outline: 3px solid #f59e0b; outline-offset: 3px; }

.add-row { display: flex; gap: 10px; }
.add-row input { flex: 1; }
.add-row button { flex-shrink: 0; }
.filters { display: flex; gap: 16px; margin-top: 24px; }
.field { flex: 1; min-width: 0; }
#stats { margin-bottom: 4px; }
#feedback { min-height: 1.6em; margin-top: 0; color: #475569; }

.task-list { list-style: none; padding: 0; margin: 16px 0 0; }
.task-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 0;
  border-bottom: 1px solid #e5e7eb;
}
.task-label {
  display: flex;
  flex: 1;
  align-items: center;
  gap: 10px;
  min-width: 0;
  margin: 0;
  cursor: pointer;
}
.task-label input { width: 18px; height: 18px; flex-shrink: 0; }
.task-title { overflow-wrap: anywhere; }
.task-item.is-done .task-title { text-decoration: line-through; color: #64748b; }
.delete-button { flex-shrink: 0; background: #b91c1c; }
.delete-button:hover { background: #991b1b; }
.empty { padding: 24px 0; text-align: center; color: #64748b; }

@media (max-width: 520px) {
  .app { margin: 16px auto; padding: 18px; }
  .filters { flex-direction: column; }
}
```

### 9.4 main.js

```js
'use strict';

const STORAGE_KEY = 'frontend-learning-tasks-v1';
const form = document.querySelector('#task-form');
const titleInput = document.querySelector('#title-input');
const keywordInput = document.querySelector('#keyword-filter');
const statusSelect = document.querySelector('#status-filter');
const list = document.querySelector('#task-list');
const stats = document.querySelector('#stats');
const feedback = document.querySelector('#feedback');

let tasks = readTasks();

function readTasks() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (raw === null) return [];
    const value = JSON.parse(raw);
    if (!Array.isArray(value)) throw new Error('数据不是数组');

    const valid = value.every(task =>
      task !== null && typeof task === 'object' &&
      typeof task.id === 'string' && task.id.length > 0 &&
      typeof task.title === 'string' &&
      task.title.trim().length > 0 && task.title.length <= 100 &&
      typeof task.done === 'boolean'
    );
    const uniqueIds = new Set(value.map(task => task?.id));
    if (!valid || uniqueIds.size !== value.length) {
      throw new Error('数据结构不合法');
    }
    // 只保留此应用需要的字段
    return value.map(({ id, title, done }) => ({ id, title, done }));
  } catch {
    feedback.textContent = '未能恢复本地数据，本次从空列表开始。';
    return [];
  }
}

function saveTasks() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
    feedback.textContent = '已保存到本地。';
  } catch {
    feedback.textContent = '当前修改仍在页面中，但本地保存失败，刷新后可能丢失。';
  }
}

function visibleTasks() {
  const keyword = keywordInput.value.trim().toLowerCase();
  const status = statusSelect.value;
  return tasks.filter(task => {
    const matchesKeyword = task.title.toLowerCase().includes(keyword);
    const matchesStatus = status === 'all' ||
      (status === 'done' ? task.done : !task.done);
    return matchesKeyword && matchesStatus;
  });
}

function render() {
  // 重建列表后，尽量保留原有任务控件的焦点
  const active = document.activeElement;
  const focusId = active?.dataset?.id;
  const focusAction = active?.dataset?.action;
  const visible = visibleTasks();
  const completed = tasks.filter(task => task.done).length;
  stats.textContent = `共 ${tasks.length} 项，完成 ${completed} 项，当前显示 ${visible.length} 项`;

  const fragment = document.createDocumentFragment();
  for (const task of visible) {
    const item = document.createElement('li');
    item.className = 'task-item';
    item.classList.toggle('is-done', task.done);

    const label = document.createElement('label');
    label.className = 'task-label';

    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.checked = task.done;
    checkbox.dataset.action = 'toggle';
    checkbox.dataset.id = task.id;

    const title = document.createElement('span');
    title.className = 'task-title';
    title.textContent = task.title;
    label.append(checkbox, title);

    const button = document.createElement('button');
    button.type = 'button';
    button.className = 'delete-button';
    button.dataset.action = 'delete';
    button.dataset.id = task.id;
    button.textContent = '删除';
    button.setAttribute('aria-label', `删除任务：${task.title}`);

    item.append(label, button);
    fragment.append(item);
  }

  if (visible.length === 0) {
    const empty = document.createElement('li');
    empty.className = 'empty';
    empty.textContent = tasks.length === 0 ? '还没有任务，先添加一项吧。' : '没有符合条件的任务。';
    fragment.append(empty);
  }

  list.replaceChildren(fragment);
  if (focusId && focusAction) {
    const controls = [...list.querySelectorAll('[data-action]')];
    const replacement = controls.find(control =>
      control.dataset.id === focusId && control.dataset.action === focusAction
    );
    if (replacement) replacement.focus();
    else titleInput.focus();
  }
}

form.addEventListener('submit', event => {
  event.preventDefault();
  const title = titleInput.value.trim();
  if (!title || title.length > 100) {
    feedback.textContent = '请输入 1～100 个字符的任务名称。';
    return;
  }

  tasks = [...tasks, { id: crypto.randomUUID(), title, done: false }];
  saveTasks();
  form.reset();
  render();
  titleInput.focus();
});

list.addEventListener('change', event => {
  const target = event.target;
  if (!(target instanceof HTMLInputElement)) return;
  if (target.dataset.action !== 'toggle') return;

  tasks = tasks.map(task => task.id === target.dataset.id
    ? { ...task, done: target.checked }
    : task
  );
  saveTasks();
  render();
});

list.addEventListener('click', event => {
  if (!(event.target instanceof Element)) return;
  const button = event.target.closest('button[data-action="delete"]');
  if (!button || !list.contains(button)) return;

  tasks = tasks.filter(task => task.id !== button.dataset.id);
  saveTasks();
  render();
});

keywordInput.addEventListener('input', render);
statusSelect.addEventListener('change', render);
render();
```

本例使用 `crypto.randomUUID()` 生成 ID，需要支持该 API 的安全上下文；现代浏览器中的本地 localhost/回环地址开发环境通常可用。请按前面的 HTTP 方式运行。

### 9.5 按职责理解代码

| 代码 | 职责 | 对应基础 |
| --- | --- | --- |
| `tasks` | 应用真实状态 | 数组、对象 |
| `readTasks` / `saveTasks` | 本地读写与验证 | JSON、Storage、try/catch |
| `visibleTasks` | 从真实状态计算可见列表 | filter、字符串方法 |
| `render` | 根据状态生成页面 | DOM、classList、文本输出 |
| submit 监听 | 组织新增流程 | 表单、trim、展开语法 |
| change 监听 | 切换完成状态 | 事件委托、map、对象展开 |
| click 监听 | 删除任务 | closest、filter |

关键思想：真实数据存放在 tasks，页面是这份状态的展示。不要一会儿只改 DOM，一会儿只改数组，导致两者对不上。

本例为学习而重建整个列表，适合小规模任务。大型应用还会考虑增量更新、复杂焦点管理、多标签同步、数据迁移等问题。

### 9.6 验收与扩展

先逐项检查：

- [ ] 空列表出现提示。
- [ ] 正常任务可添加，纯空格不会加入列表。
- [ ] 勾选后样式和统计一致。
- [ ] 删除后统计和存储一致。
- [ ] 搜索与状态筛选可以组合使用。
- [ ] 刷新后恢复任务与完成状态。
- [ ] 长标题在窄屏下不会撑破容器。
- [ ] 输入 `<b>学习</b>` 时按文字显示，不被当作标签。
- [ ] 本地数据被破坏时，页面有提示而不是直接崩溃。

再尝试扩展：编辑名称、清除已完成、截止日期排序、按标签分类。之后可将存储改成 Java 后端接口，并补齐加载、失败、空状态与鉴权。

---

## 第10章 衔接 Vue：学习重点、复习题与查阅入口

### 10.1 哪些知识要先熟练，哪些可以边做边补

| 优先级 | 知识 | 达到什么程度 |
| --- | --- | --- |
| 学 Vue 前应熟练 | HTML 表单、label、常见标签 | 能写登录页、搜索表单和列表结构 |
| 学 Vue 前应熟练 | CSS 盒模型、Flex、基础响应式 | 能独立搭出常见管理页面布局 |
| 学 Vue 前应熟练 | let/const、函数、对象、数组 | 能独立组织数据和处理列表 |
| 学 Vue 前应熟练 | map/filter/find、解构、展开 | 能读懂接口数据处理和状态更新代码 |
| 学 Vue 前应熟练 | Promise、async/await、try/catch | 能请求接口并处理失败 |
| 学 Vue 前应熟练 | import/export | 能理解多个文件之间的依赖 |
| 先理解再逐步熟练 | DOM、事件传播、事件委托 | 知道框架在帮助处理什么 |
| 可随项目深入 | Grid、复杂定位、层叠上下文 | 遇到布局问题会查找、能定位原因 |
| 可随项目深入 | 闭包、this、原型、事件循环细节 | 理解基本行为，能判断常见结果 |
| 后续专题学习 | Canvas、WebSocket、Worker、IndexedDB 等 | 根据项目实际需要学习 |

对以 Java 后端为主、希望先完成一个完整项目的你，前端的阶段目标可以是：独立完成表单、列表、分页、校验、接口联调和常见布局。基础掌握到能够解释并修改代码后，就可以进入 Vue，不需要等到所有浏览器 API 都背下来。

### 10.2 原生写法与 Vue 的联系

| 原生前端概念 | Vue 中对应的常见能力 | 需要保留的基础理解 |
| --- | --- | --- |
| textContent 更新文本 | `{{ message }}` 文本插值 | 值与展示格式 |
| setAttribute / DOM 属性 | `v-bind`、`:属性` | 属性类型、布尔属性 |
| addEventListener | `v-on`、`@click` 等 | 事件对象、回调、默认行为 |
| 根据条件插入/隐藏节点 | `v-if`、`v-show` | 条件表达式、布局隐藏差异 |
| 遍历数组创建节点 | `v-for` | 数组结构、稳定身份标识 |
| 读取 input.value 和监听输入 | `v-model` | 表单控件、类型转换 |
| 根据数据计算筛选结果 | `computed` 等 | 纯计算、filter/map |
| 手工管理数据变化后的渲染 | 响应式系统与组件渲染 | 状态是数据来源，界面由状态产生 |
| 多个 JS 文件 | 模块和单文件组件 | import/export 与依赖关系 |

这是帮助理解的对应关系，不是严格一一替换。Vue 的指令、生命周期、组件通信与渲染都有自己的规则。

### 10.3 提前看一个 Vue 3 例子

下面是单文件组件 `Counter.vue`，需要放进已配置的 Vue 工程中，不能直接作为普通 HTML 在浏览器打开。

```vue
<script setup>
import { ref, computed } from 'vue';

const count = ref(0);
const doubled = computed(() => count.value * 2);

function increment() {
  count.value += 1;
}
</script>

<template>
  <section class="counter">
    <p>当前计数：{{ count }}</p>
    <p>两倍计数：{{ doubled }}</p>
    <button type="button" @click="increment">加一</button>
  </section>
</template>

<style scoped>
.counter {
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
</style>
```

现在先看懂以下几点即可：

1. import 是 JS 模块语法。
2. ref、computed 是从 Vue 导入的函数。
3. count 是一个 ref 对象，所以 JS 中通过 count.value 读写；const 不阻止修改其 value。
4. 此处模板会自动解包顶层 ref，不用写 count.value；不要把这个行为推广到所有嵌套和集合情况。
5. 点击按钮会执行函数并改变状态，Vue 据此安排 DOM 更新。
6. DOM 更新通常会批量调度，并非每次赋值都立即同步完成，后续会学习 nextTick。
7. style 中仍是 CSS；scoped 的具体实现和边界留到组件章节。

可继续阅读 [Vue：模板语法](https://cn.vuejs.org/guide/essentials/template-syntax) 和 [Vue：响应式基础](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals)。

### 10.4 建议的实际学习顺序

1. 用 HTML 写一个登录页与一个数据表格，先确保结构和表单正确。
2. 用 CSS 完成布局，桌面和手机宽度都检查一次。
3. 用 JS 完成输入校验、数组筛选、列表新增删除。
4. 运行第9章任务清单，尝试关掉笔记重写核心逻辑。
5. 接一个 Java 后端接口，练习查询、添加、错误处理。
6. 学习 Vue 模板、响应式、计算属性、事件和表单。
7. 将任务清单改写为 Vue，比较哪些 DOM 操作被框架接管。
8. 再进入组件、路由、状态管理与完整项目。

练习时尽量做到：先自己写，卡住时查文档，最后对照示例。只阅读代码，往往很难发现自己对类型、事件和状态的理解缺口。

### 10.5 复习题与参考要点

**题1：为什么 input 有 id，原生提交时后端却可能收不到字段？**

参考要点：id 主要标识与关联元素，原生表单提交的字段名通常取自 name；还要检查 disabled、未选中的复选框等情况。

**题2：设置 width 为 200px、左右 padding 各 20px、左右 border 各 2px，实际边框盒多宽？**

参考要点：content-box 时是 244px，border-box 时声明宽度包含 padding 和 border，边框盒宽度为 200px；margin 不算在这个边框盒宽度里。

**题3：为什么 text-align:center 无法让整个卡片在页面居中？**

参考要点：它主要对齐容器里的行内内容；块盒居中可考虑合适宽度加 margin-inline:auto，或由父容器使用 Flex/Grid。

**题4：flex-direction:column 后，justify-content 控制哪个方向？**

参考要点：主轴方向；在常见水平书写环境下，此时主轴竖直，不能死记 justify 等于水平。

**题5：const user = {} 后，能不能 user.name = 'a'？**

参考要点：可以。const 限制绑定重新赋值，不保证对象属性不可变。

**题6：为什么两个 { id:1 } 用 === 比较为 false？**

参考要点：这是两个分别创建的对象，身份不同，=== 不会自动比较嵌套内容。

**题7：map 与 forEach 有什么区别？**

参考要点：map 生成新数组，适合转换；forEach 执行副作用并返回 undefined。两者回调都可能主动修改共享对象。

**题8：浅拷贝后修改嵌套属性，为什么原对象也变了？**

参考要点：外层对象虽然新建，嵌套对象仍可能共享引用。需要按数据类型和业务目的选择复制或局部更新方式。

**题9：0 || 10 和 0 ?? 10 分别是什么？**

参考要点：前者 10，后者 0。|| 按真值，?? 只按 null/undefined 判断缺失。

**题10：preventDefault 和 stopPropagation 能互相替代吗？**

参考要点：不能。一个取消默认行为，一个阻止继续传播。

**题11：为什么 fetch 收到 500 却没直接进入 catch？**

参考要点：HTTP 错误状态通常仍产生成功取得的 Response，需要检查 ok/status 并主动抛错；网络失败等才会以不同方式拒绝。

**题12：为什么 await 不会把整个页面都停住？**

参考要点：它暂停当前 async 函数的后续部分，把后续执行交还异步调度；不是让主线程同步等待。

**题13：Promise.all 失败后，其他请求会自动取消吗？**

参考要点：不会。整体 Promise 拒绝与底层请求取消不是同一件事。

**题14：localhost 和 127.0.0.1 都是本机，为什么跨域配置可能不同？**

参考要点：源比较包含主机，两者不同；还可能涉及 Cookie、存储、监听地址和代理，需要看实际错误。

**题15：前端隐藏删除按钮，是否等于用户不能调用删除接口？**

参考要点：不等于。后端必须独立校验权限，客户端展示不能代替服务器授权。

### 10.6 常见错误观念速查

| 容易形成的想法 | 更准确的理解 |
| --- | --- |
| HTML 管颜色、CSS 管结构 | HTML 表达结构与语义，CSS 控制样式和布局 |
| disabled="false" 就启用了 | 布尔属性存在就生效，需移除或修改 DOM property |
| 所有百分比都相对父宽度 | 参照依属性与格式化规则而异 |
| CSS 只比较选择器权重 | 先考虑完整层叠条件，再比较具体性等 |
| z-index 越大一定越靠前 | 受到层叠上下文边界等限制 |
| opacity:0 就完全不能交互 | 透明元素仍可能被点击或聚焦 |
| JS 数组和 Java 数组完全一样 | 普通 JS Array 的长度和元素约束不同 |
| const 代表深度不可变 | 它约束变量绑定 |
| 展开语法就是深拷贝 | 通常只是浅层复制 |
| 所有回调都是异步 | map 等很多回调同步执行 |
| async 自动让计算进入后台线程 | 普通同步计算仍在当前线程执行 |
| setTimeout(fn,0) 马上执行 | 需要等待当前工作及相应调度 |
| POST 天然比 GET 加密 | 机密传输依赖 HTTPS 等机制 |
| no-cors 是跨域万能解法 | 通常让响应不可读，不能满足普通 JSON 联调 |
| localStorage 永不丢失 | 用户清理和浏览器策略等都可能影响它 |
| 学 Vue 以后就不需要 JS | Vue 逻辑大量依赖对象、函数、数组和异步 |

### 10.7 术语对照

| 术语 | 简要含义 |
| --- | --- |
| DOM | 文档对象模型，页面结构的程序表示 |
| API | 可供代码调用的接口；可指浏览器 API，也可指后端 HTTP API |
| origin | 源，主要由协议、主机、端口确定 |
| CORS | 跨源资源共享机制 |
| callback | 回调，作为值传入并由其他代码调用的函数 |
| closure | 闭包，函数与相关词法环境的组合 |
| Promise | 表达未来结果的对象 |
| state | 状态，驱动应用行为和展示的数据 |
| render | 渲染，把数据组织成界面；具体层次依上下文而异 |
| component | 组件，组织可复用界面与逻辑的单位 |
| SPA | 单页应用，常在同一文档中处理界面与路由变化 |
| SSR | 服务端渲染，由服务端先生成页面 HTML 等内容 |
| XSS | 跨站脚本攻击，涉及不可信内容被当作可执行内容解释 |
| CSRF | 跨站请求伪造，涉及借助已有认证状态发起非预期请求 |

### 10.8 文档与进一步查阅

本笔记的讲解与业务示例为面向学习的重新组织。语言、浏览器 API 和框架的精确规则，可通过以下文档继续核对；涉及新特性时查看目标环境兼容性。

| 主题 | 文档 |
| --- | --- |
| HTML 学习主线 | [MDN：Structuring content with HTML](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content) |
| CSS 学习主线 | [MDN：CSS styling basics](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics) |
| JS 语言主线 | [MDN：JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) |
| CSS 层叠与具体性 | [MDN：Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Specificity) |
| Flex | [MDN：Basic concepts of flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Flexible_box_layout/Basic_concepts) |
| JS 运行模型 | [MDN：Execution model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model) |
| JS 模块 | [MDN：Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) |
| Fetch 请求 | [MDN：Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) |
| 跨域 | [MDN：CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) |
| Web Storage | [MDN：Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) |
| 深层克隆 | [MDN：structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone) |
| script 加载 | [MDN：script 元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script) |
| 事件监听 | [MDN：addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) |
| Vue 模板 | [Vue：模板语法](https://cn.vuejs.org/guide/essentials/template-syntax) |
| Vue 响应式 | [Vue：响应式基础](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals) |

后续还可以按需要补充：HTML 元信息与 SEO、复杂无障碍交互、CSS 容器查询与特性查询、网络缓存、WebSocket、文件与二进制处理、Worker、前端测试和 TypeScript。这些内容不影响先按本文主线开始学习 Vue。
