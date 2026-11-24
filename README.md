# AI 学习与职业工作台 —— 网页版提交包

## 这是什么

把原来桌面上的单文件作品 `AI工作台.html` 转成**可直接用网址浏览的网页版**。
现在它是一个纯静态站点，入口统一为 `index.html`，不需要安装任何环境、不需要后端服务。

- 入口文件：`index.html`（单文件，CSS / JS 全部内联，约 190 KB）
- 外部依赖：**无**（没有 CDN、没有字体外链、没有图片外链）
- 数据存储：浏览器 `localStorage`，全部在本机，不上传任何服务器
- 兼容性：Chrome / Edge / Firefox / Safari 现代版本；支持深色模式、响应式

## 关键修复：首屏白屏 Bug

**原作品存在一个会导致整页空白的缺陷，已修复。**

原因：
`boot()` 在首屏渲染之前要绑定导入用的文件选择框 `#importFile`，
但这个元素原本只写在「数据与设置」页面模板里（该模板要等到渲染时才注入 DOM）。
于是 `boot()` 执行到 `document.getElementById("importFile").addEventListener(...)`
拿到 `null` 并抛出异常，后面的 `render()` 再没被执行 ——
结果就是：侧边栏和顶栏都在（静态 HTML），但主内容区永远空白。

修复内容（`index.html`）：
1. 把 `#importFile` 从页面模板移到静态 DOM，保证脚本启动时一定存在；
2. `boot()` 调整为**先 `render()` 再绑定**，并给导入改用文档级事件委托，
   这样即使某个可选控件缺失，也只是该功能不可用，不会再整页白屏。

修复后已验证 14 个页面全部正常渲染：总览、课程与绩点、学习路线、能力雷达、项目作品集、
竞赛与科研、论文阅读、每周计划、时间与习惯、里程碑、职业方向、英语与标化、知识笔记、数据与设置。

## 如何发布成可浏览的网址

### 方案 A：Netlify Drop（最快，约 2 分钟，免费）

1. 打开 https://app.netlify.com/drop
2. 用邮箱/GitHub 免费注册登录
3. 把**整个 `ai-workbench-site` 文件夹**拖进页面
4. 拿到形如 `https://xxxx-xxxx.netlify.app` 的网址，发给老师即可

优点：永久有效、可自定义域名、支持 HTTPS。缺点：国内访问速度一般。

### 方案 B：GitHub Pages（免费、稳定、评审常用）

1. 注册/登录 GitHub，新建仓库如 `ai-workbench`
2. 网页上点 **Add file → Upload files**，上传 `index.html`
3. 仓库 **Settings → Pages**，Source 选 `main` 分支 `/ (root)`，保存
4. 约 1 分钟后访问 `https://<用户名>.github.io/ai-workbench/`

优点：永久、免费、可放 README 说明作品。缺点：需 GitHub 账号。

### 方案 C：国内平台（评审访问最快）

任选其一，上传 `index.html` 即可：

- **Gitee Pages**：需实名认证，`https://<用户名>.gitee.io/<仓库名>/`
- **腾讯云 对象存储 / 静态网站托管**：需实名，有免费额度
- **阿里云 OSS 静态网站托管**：需实名，可配自定义域名

优点：国内访问快、稳定，适合学院内网/校外专家评审。缺点：需实名认证。

### 临时预览（本机，供自己检查）

```powershell
cd ai-workbench-site
python -m http.server 8899
# 浏览器打开 http://127.0.0.1:8899
```

## 提交建议

发给老师时建议这样写：

> 作品已改为网页版，无需下载解压，直接点击链接即可浏览：
> <你的网址>
> 说明：数据保存在浏览器本地，不含任何服务端或密钥；建议用 Chrome / Edge 打开。

## 文件清单

| 文件 | 说明 |
| --- | --- |
| `index.html` | 网页版作品本体（已修复白屏 Bug） |
| `README.md` | 本说明文档 |
