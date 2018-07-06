---
layout: post
title:  "vscode tips and tricks"
tags: vscode
---

vscode를 사용하면서 항상 사용해오던 emacs 사용을 중단했다. 그만큼 맘에 드는 에디터 이기 때문이다. 
visual studio code 팁들에 대해서 정리해 보자.

## shortcut

keybindings.json
```
{ "key": "cmd+e",      "command": "workbench.action.quickOpen" },
{ "key": "alt+shift+-","command": "undo",         "when": "textInputFocus && !editorReadonly" },
```

- navigate back, `ctrl -`
- navigate forward, `ctrl shift -`

## settings 

내가 사용하고 있는 설정들..

```json
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
```

- `editor.formatOnSave` : save 시마다 `format document`를 실행
- `editor.formatOnPaste` : paste 시마다 `format document`를 실행
