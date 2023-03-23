---
layout: post
title: "Defining commands and titles for a VSCode extension/plug-in"
date: 2023-03-23
author: Forrest Sheng Bao/鮑盛
---

I recently need to access the commands and titles of VSCode plug-ins/extensions. 

A command in VSCode has two parts: the command itself (dot-separated string, e.g., `python.select.interpreter`) and a title, which is in English explaining the command, e.g., "Choose a Python interpreter". You can refer to the info from the offical doc [here](https://code.visualstudio.com/api/extension-guides/command#creating-a-user-facing-command). 

However, in many cases, the titles are actually stored in another file. This is the case for the [official Python extension](https://github.com/microsoft/vscode-python) of VSCode. If we look into [`package.json`](https://github.com/microsoft/vscode-python/blob/main/package.json#L241) of this extension, it's like this

```
{
  "category": "Python",
  "command": "python.analysis.restartLanguageServer",
  "title": "%python.command.python.analysis.restartLanguageServer.title%"
}
```

So where is the variable `%python.command.python.analysis.restartLanguageServer.title%` defined at? 

It's in another file [`package.nls.json`](https://github.com/microsoft/vscode-python/blob/main/package.nls.json) like this: 
```
Restart Language Server
```
 I assume it is for the purpose of making multi-language or i18n easier. The titles can be in any language depending on the use configuration. 

The title eventually displayed to the user is `{displayName}: {title}` where `displayName` is defined in `package.json` of the extension, e.g., "Python: Restart Language Server". 

