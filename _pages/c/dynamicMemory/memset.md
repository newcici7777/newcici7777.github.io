---
title: memset
date: 2024-07-15
keywords: c++, memset
---

memset主要目的是把記憶體位址的內容(值)，設成00000000。

[字串清空][1]

資料型態為char，設成00000000，就等同設成`\0`空字元(null character)，ascii code為0

[陣列清空][2]

[結構清空][3]

[結構中指標清空][4]

資料型態為指標，設成00000000，是把指標所`指向的記憶體位址的內容(值)`，設成00000000，等同於釋放記憶體空間，並非把指標的`指向的位址`改為00000000。

[1]: {% link _pages/c/array/charArray.md %}#字串清空
[2]: {% link _pages/c/array/array.md %}#memset陣列清空
[3]: {% link _pages/c/struct/struct_init.md %}#結構成員記憶體位址的值全初始化成00000000
[4]: {% link _pages/c/struct/pointerInStruct.md %}#memset與結構中的指標