```json
// setting.json

{

"debug.console.fontFamily": "JetBrains Mono",

// 控制在运行预启动任务后遇到错误时应该怎么做。debugAnyway: 忽略任务错误并开始调试。

"debug.onTaskErrors": "debugAnyway",

// 终端配置

"terminal.integrated.fontSize": 14,

"terminal.integrated.lineHeight": 2,

// 代码字体大小

"editor.fontSize": 15,

"editor.lineHeight": 32,

// 保存自动格式化

"editor.formatOnSave": true,

"editor.formatOnType": true,

// 控制编辑器是否应在对屏幕阅读器进行了优化的模式下运行。设置为“开”将禁用自动换行。

"editor.accessibilitySupport": "off",

// 控制是否启用平滑插入动画

"editor.cursorSmoothCaretAnimation": true,

"editor.find.addExtraSpaceOnTop": false,

// 这个控制是否启用字体连字

"editor.fontLigatures": "'ss01', 'ss02', 'ss03', 'ss06', 'zero'",

// 控制编辑器是否应呈现垂直字形边距。字形边距最常用于调试。

"editor.glyphMargin": true,

// 控制是否在编辑器中启用括号对指南

"editor.guides.bracketPairs": "active",

// 标签自动修改

"editor.linkedEditing": true,

// 控制是否在编辑器中自动显示内联建议。

"editor.inlineSuggest.enabled": true,

"editor.tabSize": 2,

"editor.fontWeight": 500,

"editor.fontFamily": "JetBrains Mono",

// 控制是否突出显示可能与基本 ASCII 字符混淆的字符，但当前用户区域设置中常见的字符除外。

"editor.unicodeHighlight.ambiguousCharacters": false,

// 控制是否突出显示仅保留空格或完全没有宽度的字符。

"editor.unicodeHighlight.invisibleCharacters": false,

// 控制每个方框有自己的独立颜色池

"editor.bracketPairColorization.independentColorPoolPerBracketType": true,

// 注释的颜色

"editor.tokenColorCustomizations": {

"comments": {

"fontStyle": "",

"foreground": "#00eeff"

},

"textMateRules": [

{

"scope": [

"function",

"constant", //String, Number, Boolean…, this, super

"storage", //static keyword

"keyword" //import, export, return…

],

"settings": {

"fontStyle": "italic"

}

},

{

"scope": ["keyword.operator"],

"settings": {

"fontStyle": ""

}

}

]

},

// 文件是否自动延迟保存

"files.autoSave": "afterDelay",

// 文件自动延迟保存时间

"files.autoSaveDelay": 2000,

// 默认行尾字符。 LF

"files.eol": "\n",

// 启用简单文件对话框。启用时，简单文件对话框将替换系统文件对话框。

"files.simpleDialog.enable": true,

// 同步 Git 存储库前请先进行确认。

"git.confirmSync": false,

"git.autofetch": true,

// 控制如何处理工作区中的松散文件

"security.workspace.trust.untrustedFiles": "open",

// 编辑器左侧目录树偏移量

"workbench.tree.indent": 16,

"workbench.iconTheme": "material-icon-theme",

"workbench.startupEditor": "newUntitledFile",

"workbench.colorTheme": "One Dark Pro",

// 控制无标题提示应该是编辑器或浮动按钮中的内联文本，还是应该隐藏。

"workbench.editor.untitled.hint": "hidden",

// 调整对话框窗口的外观。

"window.dialogStyle": "custom",

// 启用 macOS Sierra 窗口选项卡。请注意，更改在完全重新启动程序后才能生效。同时，开启原生选项卡将禁用自定义标题栏样式。

"window.nativeTabs": true, // this is great, macOS only

// 调整窗口标题栏的外观。在 Linux 和 Windows 上，此设置也会影响应用程序和上下文菜单的外观。更改需要完全重新启动才能应用。

"window.titleBarStyle": "custom",

// 控制工作台中活动栏的可见性。

"workbench.activityBar.visible": true,

// 控制在会话期间显示已打开文件的编辑器是否应在被其他进程删除或重命名时自动关闭。

// 禁用此功能将使编辑器在此类事件中保持打开状态。

// 请注意，从应用程序内删除将始终关闭编辑器，且永远不会关闭具有未保存更改的编辑器以保留数据。

"workbench.editor.closeOnFileDelete": true,

// 控制是否在具有未保存更改的编辑器的选项卡上绘制顶部边框。

"workbench.editor.highlightModifiedTabs": true,

// 控制在工作台中字体的渲染方式。antialiased: 进行像素而不是次像素级别的字体平滑。可能会导致字体整体显示得更细

"workbench.fontAliasing": "antialiased",

// 控制列表和树是否具有平滑滚动效果。

"workbench.list.smoothScrolling": true,

"extensions.autoUpdate": "onlyEnabledExtensions",

// 启用后，将不会显示扩展建议的通知。

"extensions.ignoreRecommendations": true,

// 控制终端光标是否闪烁。

"terminal.integrated.cursorBlinking": true,

// 控制终端光标的样式。

"terminal.integrated.cursorStyle": "underline",

// 要在终端中用于非粗体文本的字体粗细。接受“正常”和“加粗”这两个关键字，或接受 1-1000 之间的数字。

"terminal.integrated.fontWeight": "300",

// 当必须关闭终端进程(例如当窗口或应用程序关闭时)时，这将确定何时应还原以前的终端会话内容，以及在下次打开工作区时重新创建的进程。

"terminal.integrated.persistentSessionReviveProcess": "never",

// 控制终端选项卡是否以列表的形式显示在终端的一侧。如果禁用此功能，将改为显示下拉列表。

"terminal.integrated.tabs.enabled": true,

// 删除时，是否确认

"explorer.confirmDelete": false,

// 控制在资源管理器内拖放移动文件或文件夹时是否进行确认。

"explorer.confirmDragAndDrop": false,

// 让函数(名)和后面的括号之间加个空格

"javascript.format.insertSpaceBeforeFunctionParenthesis": true,

  

// ESLint要验证的语言

"eslint.validate": [

"javascript",

"javascriptreact",

"vue",

"typescript",

"typescriptreact",

"html",

"css",

"scss",

"less",

"mpx",

"json",

"markdown"

],

// 不显示eslint校验的警告信息

"eslint.quiet": true,

// eslint保存时只修复有问题的代码

"eslint.codeActionsOnSave.mode": "problems",

"eslint.format.enable": true,

"editor.codeActionsOnSave": {

//告诉ESLint插件在保存时运行

"source.fixAll.eslint": true

},

// prettier格式化配置

"editor.defaultFormatter": "esbenp.prettier-vscode", // 默认的代码格式化工具

"prettier.requireConfig": false,

"[vue]": {

"editor.defaultFormatter": "esbenp.prettier-vscode"

},

//针对共用的语言如JS、TS和JSX关闭文件保存自动格式化功能，通过eslint来做这件事

"[javascript]": {

"editor.formatOnSave": false

},

"[javascriptreact]": {

"editor.formatOnSave": false

},

"[typescript]": {

"editor.formatOnSave": false

}

}
```