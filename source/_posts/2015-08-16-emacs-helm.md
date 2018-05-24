---
layout: post
title:  "emacs helm"
tags: emacs helm
---

helm은 이맥스상에서 무엇인가를 선택할때(find-files, switch-buffer, recentf) 사용하는 프레임웍이다. 기존에는 ido-mode를 사용하고 있었는데 이번에 helm으로 바꾸면서 정리해 본다. 대부분의 내용은 레퍼런스에 있는 것을 번역한 수준임.

## 설치
`M-x list-packages` 에서 `helm' 패키지를 찾아서 설치한다.

## 설정
```lisp
(require 'helm-config)
(helm-mode 1)
```

## 사용법
기존의 tab-completion류의 것이랑은 다른 컨셉이라서 기본 설명이 필요할듯 하다.

1. helm 모드에 들어가면 pattern을 입력한다.
2. pattern은 N개 입력이 가능하고, 각각은 공백으로 구분한다. pattern은 regex일 수도 있다
3. helm은 후보들 중에서 pattern과 매칭하는 후보들을 리스트 업 하고 RET로 선택
4. `C-n`/`C-p` 로 아래/위로, `C-v` `M-v`로 아래페이지/위 페이지로 이동
5. `C-spc`로 후보를 마크할 수 있다. `M-a`로 모두 다 선택이다.
6. mark한 파일 list를 `C-c C-i`로 현재 버퍼에 카피할 수 있다.
7. `C-t`로 helm buffer를 vertical 또는 horizontal로 스위칭

## Reference
- [helm intro](http://tuhdo.github.io/helm-intro.html)


## revision history
* 2015/8/16 initial draft

