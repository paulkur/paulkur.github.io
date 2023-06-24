---
title: Zorro Dev and learn
date: 2023-06-02 07:59:00 +0100
categories: [servers,Zorro,learn]
tags: [code,docs,zorro,learn]     # TAG names should always be lowercase
pin: true
---
👇

## Links

- [Links](#links)
- [C++ code Snippets](#c-code-snippets)

Useful links👇

- [Financial Hacker-main](https://financial-hacker.com/)
- [F-Hacker-Build-Better-Strategies!](https://financial-hacker.com/build-better-strategies/)
- [F-Hacker-Trend_Experiment](https://financial-hacker.com/trend-delusion-or-reality/)
- [Zorro-Manual](https://zorro-project.com/manual/)
- [Zorro-Workshops](https://zorro-project.com/manual/en/tutorial_var.htm)
- [Zorro-and some...](https://zorro-project.com/manual/en/tutorial_var.htm)

1. Copy Zorro.ini file to `c:\Users\<username>\Zorro\History\`

2. [Download Data files Here](https://drive.google.com/drive/folders/18pgkha-lJdYKEz5vwGyM9AoOzfD6pXoG?usp=share_link) and unzip it in: `c:\Users\<username>\Zorro\History\`

3. When you want to run any script (example CSVtoHistory) you need to move file from folder to `Strategy` folder, then will see it in Zorro's dropdown list.

## C++ code Snippets

Plot trade distribution

```cpp
#include <profile.c>

function run()
{
  ...

    plotTradeProfile(-50);
}
```

[Back to Top](#links)
