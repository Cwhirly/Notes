# Luogu Markdown

Render [Luogu](https://www.luogu.com.cn/) flavored Markdown in Obsidian.

洛谷的 Markdown 是 CommonMark + GFM 再加一组**块级**扩展。Obsidian 本身已经能正确渲染
CommonMark、GFM 和 LaTeX，但会把 `:::info` 折叠框、`::cute-table` 和表格的 `^` / `<`
合并标记当成普通文字。这个插件补上这部分。

方言依据（不是凭印象写的）：

- [洛谷 Markdown 格式手册](https://help.luogu.com.cn/rules/academic/handbook/markdown)
- [LaTeX 格式手册](https://help.luogu.com.cn/rules/academic/handbook/latex)
- [wudream813/luogu-markdown-editor](https://github.com/wudream813/luogu-markdown-editor) 的解析器与测试语料
- 用 [luogu-renderer](https://www.npmjs.com/package/luogu-renderer)（remark + remark-directive + remark-gfm
  管线）对上面每条语法做了交叉验证，`:::align` 无参数默认 `right` 就是这么发现的

关于官方的 [luogu-dev/markdown-palettes](https://github.com/luogu-dev/markdown-palettes)：
它是 2019 年的 Vue 2 + markdown-it 8 编辑器，只带了 `markdown-it-v` / KaTeX / Prism，
**没有** `:::info`、`::cute-table` 这些指令语法，落后于洛谷现在线上使用的 remark/rehype 管线，
所以无法直接复用。官方组织下也没有发布实现这些语法的库。
`luogu-renderer` 是第三方包且为 **AGPL-3.0**，因此只用于开发期交叉验证，未作为依赖引入。

## 用法

### 直接写在正文里（不需要围栏，阅读视图）

    :::info[提示]
    正文可以用 **加粗**、`行内代码`、$公式$。
    :::

- **阅读视图**：渲染成可折叠的 callout。
- **实时预览（编辑模式）**：同样渲染成可折叠的 callout（CodeMirror block widget），
  光标移进容器时自动显示原文以便编辑。
- **外观完全用 Obsidian 原生 callout 的样式**：生成的就是 `details.callout[data-callout]`
  这套 DOM，图标、配色、暗色模式、折叠行为全部交给主题，和 `> [!info]` 完全一致。
- 容器**内部不能有空行**。有空行就跨了多个段落，插件会**原样保留文字**而不是冒险处理
  （早期版本在这种情况下会丢内容）。需要多段落时用下面的围栏。

### 用 `luogu` 围栏（推荐，实时预览也能看）

    ~~~luogu
    :::info[提示]
    第一段。

    第二段也没问题。
    :::

    | a | b |
    | :-: | :-: |
    | 1 | < |
    ~~~

围栏是**功能最完整**的路径：**实时预览里就能看到渲染结果**，支持容器内空行、
Tuack 表格、单元格 `^` / `<` 合并、代码块 `line-numbers` / `lines=`。

### 对照

| | 正文直接写 | `~~~luogu` 围栏 |
| --- | --- | --- |
| 阅读视图 | ✅ 可折叠 | ✅ 可折叠 |
| 实时预览（编辑模式） | ✅ 可折叠 | ✅ 可折叠 |
| 外观 | Obsidian 原生 callout | Obsidian 原生 callout |
| 容器内空行 | ❌（保留原文） | ✅ |
| Tuack 表格 / 单元格合并 | ❌ | ✅ |
| 代码块行号与高亮 | ❌ | ✅ |

## 支持的语法

| 语法 | 说明 |
| --- | --- |
| `:::info` `:::success` `:::warning` `:::error` | 折叠框，默认标题为 提示 / 成功 / 警告 / 错误 |
| `:::info[标题]` | 自定义标题，标题里可以用 LaTeX |
| `:::info[标题]{open}` | 默认展开 |
| `::::info` … `:::` | 嵌套，外层冒号更多；闭合栏的冒号数可以大于等于开始栏 |
| `:::align{center}` / `{left}` / `{right}` | 对齐容器；**不写参数时是 `right`**（与洛谷一致） |
| `:::epigraph[作者]` | 引言 |
| `::cute-table{tuack}` | Tuack 风格表格（对齐居中、表头加重） |
| 单元格写 `^` | 向上合并（rowspan） |
| 单元格写 `<` | 向左合并（colspan） |
| ```` ```cpp line-numbers ```` | 代码块显示行号 |
| ```` ```cpp lines=1-3,5 ```` | 代码块高亮指定行 |
| `![](bilibili:BV1xx411c7mD)` | Bilibili 视频，渲染为可点击链接 |

`$...$`、`$$...$$`、`**加粗**`、表格、任务列表、脚注等交给 Obsidian 原生的 Markdown /
MathJax 管线处理，插件不会重复解析它们。

## 命令

| 命令 | 作用 |
| --- | --- |
| Wrap selection in a Luogu block | 把选中的内容用 `luogu` 围栏包起来（会自动选足够长的围栏） |
| Check selection for Luogu incompatibilities | 检查选中的内容 |
| Check note for Luogu incompatibilities | 检查整篇笔记 |

检查项基于官方手册与参考实现的结论：

- 原始 HTML：洛谷**不解析**，会原样显示
- `++文字++`：洛谷不支持下划线，会原样显示
- 容器名拼写、闭合栏冒号数量是否匹配
- 未闭合的容器

## 已知限制

2. **正文里的容器不能含空行。** 含空行时整段原样保留为文字（不渲染，也不丢内容）。
3. **Bilibili 视频不内嵌播放器**，只渲染成链接。笔记不应该自己向 bilibili.com 发请求，
   这既是 Obsidian 的预期，也是社区目录的政策要求。
4. **KaTeX 用 Obsidian 自己的版本**，不是洛谷的 0.16.7，极少数宏可能有差异。
5. **`::cute-table` 的 `{tuack=N}` / `{three=N}` 列加粗没有实现**（`{tuack}` 整体样式已支持）；
   `{marku=}` / `{markl=}` 自定义合并标记也没有实现（上游 `markl` 本身有 bug）。
6. **没有标题锚点**，因为洛谷的管线里也没有。
7. 洛谷对超过 10 层的嵌套会直接输出 `Too many levels of nesting!`；本插件限制 12 层后按纯文本处理。

## 开发

```bash
npm install
npm run dev      # esbuild watch，改源码自动重建 main.js
npm test         # 49 项行为测试（解析器 / 正文容器变换 / 校验 / 视频链接）
npm run build    # 生产构建，发布会用这个
```

本地调试：把插件目录软链到测试库的 `.obsidian/plugins/` 下即可。

```bash
ln -s ~/Projects/obsidian-plugins/luogu-markdown \
      /path/to/vault/.obsidian/plugins/luogu-markdown
```

注意：Obsidian 不会在 `main.js` 变化后自动重载插件，需要手动关开插件开关，
或安装 [Hot-Reload](https://github.com/pjeby/hot-reload)。

## 结构

```
src/
  main.ts       插件入口：注册代码块处理器、post processor、命令、设置
  luogu.ts      洛谷块级语法解析器（纯函数，无 DOM 依赖）
  render.ts     把语法树渲染成 DOM（全部用 createEl，不用 innerHTML）
  transform.ts  正文模式下把已渲染的容器重新组装
  lint.ts       洛谷兼容性检查
  video.ts      Bilibili 链接规范化
  settings.ts   设置面板
test/           行为测试
```

## 许可

MIT
