# DriveMind 项目主页

NeurIPS 2026 论文 *DriveMind: Mind-Evolving Belief Tracking for Closed-Loop Autonomous Driving* 的项目网站。

纯静态网站：一个 `index.html` 加一个 `assets/` 文件夹。不需要 Node、不需要构建工具，样式和脚本都写在 `index.html` 里。

## 文件结构

```
index.html                         整个页面（HTML + CSS + JS）
assets/fig/                        论文 Figure 1、Figure 2 的高清裁剪图
assets/cases/<案例>/01.jpg ...      五个闭环案例的相机帧（论文 Fig. 4、7、8、9、10）
assets/og.jpg                      分享到社交平台时的预览图（1200×630）
assets/favicon.svg                 浏览器标签页图标
static/DriveMind_NeurIPS2026.pdf   论文 PDF
.nojekyll                          让 GitHub Pages 原样发布，不经过 Jekyll
```

## 本地预览

在这个文件夹里运行：

```bash
python3 -m http.server 8000
```

然后打开 http://localhost:8000 。直接双击 index.html 也能看，但图片路径在本地服务器下最可靠。

## 发布到 GitHub Pages

1. 在 GitHub 新建一个公开仓库，例如 `drivemind`。
2. 把本文件夹里的**所有文件**（包括隐藏文件 `.nojekyll`）放到仓库根目录，推送到 `main` 分支：
   ```bash
   git init
   git add .
   git commit -m "DriveMind project page"
   git branch -M main
   git remote add origin https://github.com/<用户名>/drivemind.git
   git push -u origin main
   ```
3. 打开仓库 **Settings → Pages**，在 *Build and deployment* 里选 **Deploy from a branch**，Branch 选 `main`，目录选 `/ (root)`，保存。
4. 一两分钟后访问 `https://<用户名>.github.io/drivemind/`。

## 上线前要改的地方（在 index.html 里搜索关键词）

| 要改什么 | 搜索 | 怎么改 |
|---|---|---|
| arXiv 和代码链接 | `btn--soon` | 把 `<span class="btn btn--soon" ...>arXiv <small>soon</small></span>` 换成 `<a class="btn" href="https://arxiv.org/abs/XXXX.XXXXX">arXiv</a>`，Code 同理 |
| 页面底部的链接列表 | `Coming soon` | 把 `<span>arXiv</span><span class="soon">Coming soon</span>` 换成真实链接 |
| 论文 PDF | `DriveMind_NeurIPS2026.pdf` | 替换 `static/` 里的文件，或把链接改成 arXiv / OpenReview 地址 |
| 作者主页 | `class="authors"` | 把 `<span class="a">Yan Shi<sup>1</sup></span>` 改成 `<a class="a" href="主页网址">Yan Shi</a><sup>1</sup>` |
| BibTeX | `id="bibtex"` | 直接改里面的文字，复制按钮会自动复制新内容 |
| 分享预览图 | `og:image` | 部署后改成完整网址，例如 `https://<用户名>.github.io/drivemind/assets/og.jpg`（`twitter:image` 同理） |

## 数据在哪里

所有数字和案例文字都在 `index.html` 底部 `<script>` 的 **DATA** 部分，逐项对应论文：

- `CASES`：五个闭环案例（论文 Fig. 4、7、8、9、10）的帧编号、四条假设文本和权重
- `DYN`：论文 Fig. 3 的三条曲线（熵、主导权重、ESS），数值读自原图矢量路径
- `SCEN`：场景类别（Table 2）和长尾场景（Table 8）
- `BOARD`：Bench2Drive 闭环对比（Table 1a）
- `LATENCY`：系统级延迟（Table 5）
- `RESAMPLE`：重采样示意（Fig. 6）

改了论文数字，只需要改这里，图表会自动跟着变。

## 加一个新案例

1. 把相机帧放进 `assets/cases/<新名字>/`，命名为 `01.jpg`、`02.jpg` ……
2. 在 `CASES` 数组里照着已有的格式加一项：`id` 填文件夹名，`stages` 里写每个推理步的帧编号 `f`、四个权重 `w` 和四条假设 `h`。
3. 页面上会自动多出一个标签页。
