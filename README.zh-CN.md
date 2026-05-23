# iGUITest 团队主页维护说明

本仓库是一个用于 GitHub Pages 的静态团队主页。页面使用纯 `HTML`、`CSS` 和少量前端脚本实现，没有构建流程，也没有框架依赖。

## 文件结构

- `index.html`：页面结构与前端渲染逻辑
- `styles.css`：样式与布局
- `paper.csv`：论文数据
- `members.json`：成员数据
- `support.json`：奖励 / 资助数据
- `teaching.json`：教学数据
- `YSC.jpg`、`award.gif`、`ETH.svg`、`TUM.png`：页面使用的图片资源

## 页面工作方式

页面会在浏览器中动态加载结构化数据：

- 论文来自 `paper.csv`
- 成员来自 `members.json`
- 奖励 / 资助来自 `support.json`
- 教学内容来自 `teaching.json`

因此，大部分内容维护都应该通过修改数据文件完成，而不是直接改 `index.html`。

## 内容维护方法

### 1. 修改成员信息

编辑 `members.json`。

每个成员对象的结构如下：

```json
{
  "name": "成员姓名",
  "role": "角色/身份",
  "type": "featured",
  "image": "photo.jpg",
  "description": "简介文本。",
  "highlights": [
    "亮点 1",
    "亮点 2"
  ]
}
```

说明：

- `type` 可选，设为 `"featured"` 时会显示为顶部大卡片
- `image` 只在 featured 成员中需要
- `highlights` 可选

普通成员可以写成：

```json
{
  "name": "成员姓名",
  "role": "Student Researcher",
  "description": "简介文本。"
}
```

### 2. 修改奖励 / 资助

编辑 `support.json`。

每个卡片对象结构如下：

```json
{
  "title": "标题",
  "image": "award.gif",
  "items": [
    "条目 1",
    "条目 2"
  ]
}
```

说明：

- `image` 可选
- `items` 建议保持为简洁的要点列表

### 3. 修改教学内容

编辑 `teaching.json`。

每个对象结构如下：

```json
{
  "title": "课程或教学活动名称",
  "description": "简短说明。",
  "tag": "标签"
}
```

说明：

- `tag` 可选，但建议保留，便于页面展示分类感

### 4. 修改论文列表

编辑 `paper.csv`。

当前列结构如下：

```text
year,id,title,link,status,author,cofauthor,corauthor,level,venue,note
```

当前页面实际使用这些字段：

- `year`
- `id`
- `title`
- `link`
- `author`
- `level`
- `venue`
- `note`

说明：

- 页面会按照 `year` 分组
- 同一年内按照 `id` 排序
- `status` 当前不会显示在页面上
- 期刊/会议缩写由 `index.html` 中的 `getVenueLabel()` 补充

如果新增 venue 后页面没有显示缩写，需要去 `index.html` 里更新 `getVenueLabel()` 映射。

## 视觉与页面维护

### 修改页面文案

按内容类型修改：

- `index.html`：静态区块，例如 hero、about、research areas
- 数据文件：members、support、teaching、publications

### 修改颜色、间距、卡片样式

编辑 `styles.css`。

页面主要设计变量在文件顶部，例如：

- `--bg`
- `--surface`
- `--surface-muted`
- `--text`
- `--brand`
- `--accent`
- `--shadow`

### 替换图片

步骤：

1. 将新图片放到仓库根目录
2. 在 `index.html` 或对应 `json` 数据中修改文件名
3. 文件名尽量简单，避免空格和特殊字符

## 本地预览

由于页面通过 `fetch()` 加载 `CSV` 和 `JSON`，如果直接用 `file://` 打开 `index.html`，有些浏览器会拦截数据请求，导致内容加载失败。

推荐预览方式：

1. 推送到 GitHub Pages 后在线查看
2. 本地启动一个简单静态服务器

示例：

```bash
python3 -m http.server 8000
```

然后访问：

```text
http://localhost:8000/
```

## GitHub Pages 部署

该仓库已经适合直接作为 GitHub Pages 静态站点部署。

常规流程：

1. 修改数据文件或页面文件
2. 提交并推送到 GitHub Pages 对应分支
3. 等待 GitHub Pages 自动发布

## 建议的维护规则

- 页面主文案默认保持英文，除非明确要做中英双语页面
- 能改数据文件，就不要优先改渲染逻辑
- 团队主页适合简洁、学术风格的表达，不要把成员简介和项目说明写得过长
- 新增论文时，注意 `year`、`id`、`venue` 填写一致
- 如果后续内容继续增长，建议把 `about` 和 `research areas` 也拆成独立 JSON

## 常见问题

### 页面显示 “Unable to load ... data.”

常见原因：

- 使用了 `file://` 直接打开页面
- 数据文件名写错
- JSON 格式不合法
- CSV 格式损坏

### 修改 JSON 后某一栏为空

请检查：

- JSON 中逗号、引号是否正确
- 文件是否为合法 JSON
- 是否缺少必要字段，比如 `title` 或 `name`

### 某篇论文没有显示会议/期刊缩写

需要到 `index.html` 中更新 `getVenueLabel()` 的映射表。

## 后续建议

- 将 `about` 拆到 `about.json`
- 将 `research areas` 拆到 `research.json`
- 在 `paper.csv` 中增加 `abbr` 列，替代当前前端硬编码的 venue 缩写映射
