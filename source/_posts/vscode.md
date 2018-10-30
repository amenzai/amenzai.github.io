---
title: VScode个人常用扩展及设置
comment: true
categories: 前端工具
tags: ide
date: 2018-10-26 01:14:43
---

这一篇记录一下，`VScode` 中 个人常用的扩展及设置。


<!-- more -->

## 扩展

- ESLint
- One Dark Theme
- Seti-theme
- Sublime Text Keymap
- vetur
- Prettier
- Bracket Pair Colorizer
- [px2rem](https://marketplace.visualstudio.com/items?itemName=arturiapendragon.px2rem#overview)
- [auto close tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
- auto rename tag
- beautify
- git history
- JavaScript (ES6) snippets
- Markdown Preview Enhanced
- markdown preview github
- markdown all in one
- Terminal
- Path Intellisense
- Open in Browser
- react/redux/react-router
- editorconfig for vscode

## 个人设置

```json
// 将设置放入此文件中以覆盖默认设置
{
    "window.zoomLevel": -1,
    "editor.tabSize": 2,
    "workbench.colorTheme": "One Dark Theme",
    "editor.snippetSuggestions": "top",
    "explorer.confirmDragAndDrop": false,
    // "editor.formatOnSave": true,
    // 控制字体系列。
    "editor.fontFamily": "Consolas, 'Courier New', monospace, Dengxian",
    "editor.multiCursorModifier": "ctrlCmd",
    "files.associations": {
        "*.inc": "html",
        "*.ejs": "html",
        "*.wxss": "css",
        "*.wpy": "vue"
    },
    "explorer.confirmDelete": false,
    "vsicons.dontShowNewVersionMessage": true,
    "prettier.singleQuote": true,
    "prettier.semi": false,
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    "vetur.format.defaultFormatterOptions": {
        "wrap_attributes": "force-aligned"
    },
    "emmet.syntaxProfiles": {
        "vue-html": "html",
        "vue": "html",
        "wpy": "html"
    },
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "html",
        {
            "language": "vue",
            "autoFix": true
        }
    ],
    "eslint.autoFixOnSave": true,
    "beautify.language": {
        "js": {
            "type": [
                "javascript",
                "json"
            ],
            "filename": [
                ".jshintrc",
                ".jsbeautify"
            ]
        },
        "css": [
            "css",
            "less",
            "scss"
        ],
        "html": [
            "htm",
            "html"
        ]
    },
    "git.autofetch": true,
    "git.enableSmartCommit": true,
    "javascript.implicitProjectConfig.experimentalDecorators": true,
    "emmet.includeLanguages": {
        "wxml": "html"
    },
    "minapp-vscode.disableAutoConfig": true
}
```