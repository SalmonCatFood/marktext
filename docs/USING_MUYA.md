# Using Muya

Muya 是 MarkText 的核心 Markdown 编辑模块。根据开发文档中的描述：

> MarkText can be split in three parts: the core called Muya, the main- and renderer process.
> Muya provides realtime preview and markdown editing via multiple modules based on a block structure. You can imagine it as the editor backend with modules for markdown parsing, data store as block structure, markdown document transformations according CommonMark and GitHub Flavored Markdown specification with some extra specifications, event listeners and an exporter to generate standalone HTML and markdown files but also to generate the WYSIWYG editor. Muya is single threaded as well as MarkText but use asynchronous functions to boost performance.

【F:docs/dev/ARCHITECTURE.md†L22-L30】

编辑器界面部分说明：

> The editor is the core element that hosts the realtime preview editor called Muya and consists of three parts. Tabs are located at the top and at the bottom the per-tab notification bar is located for events like file changed or deleted. The main part is the editor that is either provided by Muya or CodeMirror for the source-code editor. There are multiple overlays available like inline toolbar, emoji picker, quick insert or image tools.

【F:docs/dev/INTERFACE.md†L21-L21】

## 调用方式

1. 在页面中准备一个容器元素，例如 `<div id="editor"></div>`。
2. 从 `muya/lib` 中导入 `Muya`，并创建实例：

```javascript
import Muya from 'muya/lib'

const container = document.getElementById('editor')
const muya = new Muya(container, {
  markdown: '# Hello Muya'
})

muya.on('change', ({ markdown }) => {
  console.log(markdown)
})
```

实例方法如 `setMarkdown`、`getMarkdown`、`undo`、`redo` 等，可用于控制编辑器行为。

## 依赖

Muya 在构建时依赖多种第三方库，包括但不限于：

- `katex`、`mermaid`、`prismjs` 等渲染库
- `popper.js`、`fuzzaldrin`、`unsplash-js`
- `html-tags`、`github-markdown-css`
- `turndown`、`underscore`、`webfontloader`

这些依赖在项目的 `package.json` 中列出，如下所示：

```json
"fuzzaldrin": "^2.1.0",
"github-markdown-css": "^3.0.1",
"html-tags": "^3.2.0",
"katex": "^0.15.3",
"mermaid": "^10.0.0",
"popper.js": "^1.16.1",
"prismjs": "^1.27.0",
"turndown": "^7.1.1",
"underscore": "^1.13.2",
"unsplash-js": "^7.0.15",
"vega-lite": "^5.2.0",
"webfontloader": "^1.6.28"
```

【F:package.json†L60-L89】

## 示例

完整示例可在 MarkText 的 `src/renderer/components/editorWithTabs/editor.vue` 文件中看到，其中通过 `new Muya(ele, options)` 初始化编辑器并监听 `change` 等事件。

