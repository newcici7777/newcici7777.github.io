---
title: Data class
date: 2025-05-30
keywords: kotlin, Data class
---
Prerequisites:

- [Any][1]
- [Java ==與equals][2]

一般類別繼承Any，所以==與===都是比較記憶體位址是否相同。

Data class資料類別會自動覆寫equals()與toString()，
所以==是比較內容是否相同，===是比較記憶體位址是否相同。





[1]: {% link _pages/kotlin/any.md %}
[2]: {% link _pages/java/equals_compare.md %}