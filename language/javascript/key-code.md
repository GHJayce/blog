---
title: "JS 键盘按键keyCode对应表"
date: 2017-11-08T09:05:13+08:00
updateDate: 2017-11-08T09:05:13+08:00
slug: language/javascript/key-code
image: https://ghjayce.github.io/asset/blog/hhu3ZhdElGKvdDDmPImUMjAxNzExMDhfMDkwNTEz.jpeg
categories:
  - 程序设计语言
tags:
  - JavaScript
  - KeyCode
  - KeyboardEvent
draft: false
---

## keydown event对象

![js keydown event](https://ghjayce.github.io/asset/blog/XQ9pwkmDr2oRDf8QMnTYMjAxODAzMzFfMTEyMTAw.jpeg "js keydown event")

| 按键    | 对象       | 数据类型    |
| :---- | :------- | :------ |
| ctrl  | ctrlKey  | boolean |
| alt   | altKey   | boolean |
| shirt | shirtKey | boolean |

## keyCode

### 速记
| 按键       | 对象        | 描述     |
| :------- | :-------- | :----- |
| A - Z    | 65 ~ 90   | 字母     |
| 0 - 9    | 48 ~ 57   | 字母上数字键 |
| 0 - 9    | 96 ~ 105  | 小键盘数字键 |
| F1 - F12 | 112 ~ 123 | 功能键    |

### 字母按键

| 按键  | 键码  | 按键  | 键码  | 按键  | 键码  |
| :-- | :-- | :-- | :-- | :-- | :-- |
| A   | 65  | B   | 66  | C   | 67  |
| D   | 68  | E   | 69  | F   | 70  |
| G   | 71  | H   | 72  | I   | 73  |
| J   | 74  | K   | 75  | L   | 76  |
| M   | 77  | N   | 78  | O   | 79  |
| P   | 80  | Q   | 81  | R   | 82  |
| S   | 83  | T   | 84  | U   | 65  |
| V   | 86  | W   | 87  | X   | 88  |
| Y   | 89  | Z   | 90  |     |     |


### 字母上面一排数字键

| 按键  | 键码  | 按键  | 键码  | 按键  | 键码  |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0   | 48  | 1   | 49  | 2   | 50  |
| 3   | 51  | 4   | 52  | 5   | 53  |
| 6   | 54  | 7   | 55  | 8   | 56  |
| 9   | 57  |     |     |     |     |

### 控制键

| 按键          | 键码  | 按键       | 键码  | 按键          | 键码  |
| :---------- | :-- | :------- | :-- | :---------- | :-- |
| tab         | 9   | enter    | 13  | shift       | 16  |
| control     | 17  | alt      | 18  | pause break | 19  |
| caps lock   | 20  | spacebar | 32  | page up     | 33  |
| page down   | 34  | end      | 35  | home        | 36  |
| left arrow  | 37  | up arrow | 38  | right arrow | 39  |
| down arrow  | 40  | insert   | 45  | delete      | 46  |
| windows     | 91  | menu     | 93  | num lock    | 144 |
| scroll lock | 145 | esc      | 27  |             |     |

### 小键盘按键

| 按键  | 键码  | 按键     | 键码  | 按键  | 键码  |
| :-- | :-- | :----- | :-- | :-- | :-- |
| /   | 111 | *      | 106 | -   | 109 |
| \+  | 107 | delete | 110 | 0   | 96  |
| 1   | 97  | 2      | 98  | 3   | 99  |
| 4   | 100 | 5      | 101 | 6   | 102 |
| 7   | 103 | 8      | 104 | 9   | 105 |

### 符号

| 按键       | 键码  | 按键       | 键码  | 按键       | 键码  |
| :------- | :-- | :------- | :-- | :------- | :-- |
| `;`（分号）  | 186 | `=`      | 187 | `,`      | 188 |
| `-`（减号）  | 189 | `.`      | 190 | `/`（斜杠）  | 191 |
| \`（反单引号） | 192 | `[`      | 219 | `\`（反斜杠） | 220 |
| `]`      | 221 | `'`（单引号） | 222 |          |     |

### 功能键

| 按键 | 键码 | 按键 | 键码 | 按键 | 键码 |
|:---- |:---- |:---- |:---- |:---- |:---- |
| F1   | 112  | F2   | 113  | F3   | 114  |
| F4   | 115  | F5   | 116  | F6   | 117  |
| F7   | 118  | F8   | 119  | F9   | 120  |
| F10  | 121  | F11  | 122  | F12  | 123  |