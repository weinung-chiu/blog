# 文章 Front Matter 模板

新增文章時，複製貼上以下區塊：

```yaml
---
title: ""                 # 文章標題；會作為頁面的 H1，本文不要再寫一層 #
date: YYYY-MM-DD           # 發布日期
slug: ascii-hyphenated     # 純小寫英文 + 連字號，不要中文、空白、底線
tags:
  - topic-tag
draft: true                # 完成後改成 false
description: ""            # 一兩句話摘要
---
```

## 單檔 vs page bundle（可混用）

同一個主題資料夾下，兩種形式可以並存：

- **單檔**：沒有圖片／附件的文章，直接放 `content/<topic>/<slug>.md`
- **page bundle**：有圖片／附件的文章，建一個資料夾 `content/<topic>/<slug>/`，本文放 `index.md`，圖片等附件放在同一資料夾，用相對路徑引用，例如 `![alt](./photo.png)`

判斷規則很單純：只要文章有要 co-locate 的檔案就用資料夾，純文字就用單檔。新增文章後記得到該主題的 `README.md` 補上一行連結。

## 內文規則

- 本文從 `##` 開始，不要放 `#`（H1 由 front matter 的 `title` 提供）
- 只用純 CommonMark 與標準 HTML，不要用任何 generator 專屬語法（Liquid、Hugo shortcode、Hexo tag plugin 等），避免之後換 generator 要重寫內文
