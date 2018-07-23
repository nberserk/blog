---
layout: post
title:  "vscode tips and tricks"
tags: vscode
---

vscode를 사용하면서 항상 사용해오던 emacs 사용을 중단했다. 그만큼 맘에 드는 에디터 이기 때문이다. 
visual studio code 팁들에 대해서 정리해 보자.

## shortcut customization

keybindings.json

```
{ 
    "key": "cmd+e",
    "command": "workbench.action.quickOpen" },
{ 
    "key": "alt+shift+-",
    "command": "undo",         "when": "textInputFocus && !editorReadonly" },
{
    "key": "cmd+[",
    "command": "workbench.action.navigateBack"
},    
{
    "key": "cmd+]",
    "command": "workbench.action.navigateForward"
},
```

### multiple cursor

가장 유용한 기능중의 하나인데, 여러개의 라인에 걸쳐서 같이 편집이 가능한 기능인데 잘 쓰면 매우 유용하다. [여기에 있는 git를 보면] 이해가 갈듯하다.

맥에서의 단추키는 <kbd>alt+cmd+up|down</kbd> 이다. paste 단어 삭제등도 동시에 가능하다.


## settings 

내가 사용하고 있는 설정들..

```json
    "editor.formatOnSave": true,        // format on save
    "editor.formatOnPaste": true,       // format on paste
    "editor.minimap.enabled": false,    // disable minimap
```

