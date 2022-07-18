---
title: "Take a look at our online compiler"
layout: post
date: 2022-06-03 12:45
image: https://upload.wikimedia.org/wikipedia/commons/thumb/1/1f/WebAssembly_Logo.svg/900px-WebAssembly_Logo.svg.png
headerImage: True
tag:
- Tech
category: blog
projects: false
author: Hao Feng
description:
---

Ningzi Zhan, Hansong Zhang, Cheng Huang and I contributed to an online compiler and REPL of Python which translates source code to WebAssembly, collaborated with 40+ developers to implement it in TypeScript. For users, it has the following features:

- **Drag bar**: It could make adjustments to the size of the editor field and interactive console. With this feature, users can focus on a specific mode they are interested in.
- **Clear button**: Clicking this button would reset all content in the editor and interactive area.
- **Refresh button**: Clicking this button would remove error hits and breakpoints in the editor.
- **End-to-end test**: We introduced Cypress as the testing automation solution used for web automation. Testcases are defined in cypress/integration/fronted.spec.js. Uses could simply run `npm run e2e` to start this test tool.

With the power of CodeMirror, we introduce other fancy features in the editor field:

- **Themes selection**: A simple select menu to change the editor style with many options.
- **Edit mode**: Users could switch between normal mode and vim mode of the editor and write code like in vim.
- **Breakpoint**: With breakpoints, users could control the scope of the code running. We are currently unable to add watchers as the stack is inaccessible. However we can still make multiple brekapoints and achieve similar functionalities.
- **Error highlight**: The code location returned by the error reporting module will be displayed in the editor and will be marked as well as underline the word casuign errors.
- **Autocompletion and hint**: When the user presses the Ctrl key, code before the cursor can be automatically completed based on the rules of the config file we add.

The overall front page looks like below:
![final_preview]({{site.url}}/assets/images/compiler/final_preview.png)

And the overall error display and breakpoing functionalities are like below:
![bp1]({{site.url}}/assets/images/compiler/bp1.png)
![bp2]({{site.url}}/assets/images/compiler/bp2.png)
