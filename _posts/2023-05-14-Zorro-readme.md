---
title: Zorro Dev
date: 2023-05-14 09:59:00 +0100
categories: [servers,Zorro,setup]
tags: [code,docs,zorro,setup]     # TAG names should always be lowercase
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

### C++ code Snippets

```cpp
function run()
{
  BarPeriod = 60;  // 15 minutes per histogram bar
  LookBack = 100;
  StartDate = 2020;
  EndDate = NOW;
  asset("BTCUSDT");
  set(PLOTNOW);
  MaxLong = MaxShort = 1; // ONLY one dirrection
  Capital = 10000;

  vars Prices = series(price());
  vars slow_EMA = series(EMA(Prices,50));
  vars fast_EMA = series(EMA(Prices,20));

  if(crossOver(fast_EMA,slow_EMA))
    enterLong();
  if(crossUnder(fast_EMA,slow_EMA))
    enterShort();

// PLOTS
  plot("slow_EMA", slow_EMA, LINE, RED);
  plot("fast_EMA", fast_EMA, LINE, BLUE);
} 
```

[Back to Top](#links)
