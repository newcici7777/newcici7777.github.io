---
title: 檔案目錄操作
date: 2024-12-12
keywords: c++, getcwd
---

Prerequisites:

[在linux編譯c++][1]

以下的程式碼，請在linux下編譯執行。

## 取得目前目錄
### getcwd取得目前目錄

include
```
#include <unistd.h>
```

#### 語法
```
char* getcwd(char *buf, size_t size);
```

#### 參數

buf
路徑的儲存位置。

size
路徑的最大長度

###  get_current_dir_name

語法
```
char* get_current_dir_name(void)
```
沒有參數，不用帶入buf路徑儲存位置跟最大長度

注意事項:

- get_current_dir_name()是以malloc()建立，需要使用free()釋放記憶體
- mac xcode無法編譯，需在linux下編譯。

### 程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <unistd.h>
using namespace std;
int main() {
  // 方法1
  // 路徑，linux路徑最大長度為256
  char path1[256];
  // 參數1，路徑
  // 參數2，路徑最大長度
  getcwd(path1,256);
  cout << "path1 = " << path1 << endl;
  
  //方法2
  char* path2 = get_current_dir_name();
  cout << "path2 = " << path2 << endl;
  // get_current_dir_name()是以malloc()建立
  // 需要使用free()釋放記憶體
  free(path2);
  return 0;
}
{% endhighlight %}

## mkdir

include
```
<sys/stat.h>
```

語法
```
int mkdir(const char *pathname, mode_t mode);
```

參數

pathname: 目錄名

mode: 目錄的權限

函數說明： mkdir()函數以mode方式建立以參數pathname命名的目錄，mode定義新建立目錄的權限。

傳回值： 若目錄建立成功，則傳回0；否則回傳-1，並將錯誤記錄到全域變數errno。

mode方式：

要4個數字，最前面要有0，如0755，0700，0644


- [鳥哥](https://linux.vbird.org/linux_basic/centos7/0210filepermission.php)

```
改變權限, chmod
檔案權限的改變使用的是chmod這個指令，但是，權限的設定方法有兩種， 分別可以使用數字或者是符號來進行權限的變更。我們就來談一談：

數字類型改變檔案權限

Linux檔案的基本權限就有九個，分別是owner/group/others三種身份各有自己的read/write/execute權限， 先複習一下剛剛上面提到的資料：檔案的權限字元為：『-rwxrwxrwx』， 這九個權限是三個三個一組的！其中，我們可以使用數字來代表各個權限，各權限的分數對照表如下：
r:4
w:2
x:1
每種身份(owner/group/others)各自的三個權限(r/w/x)分數是需要累加的，例如當權限為： [-rwxrwx---] 分數則是：
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0

那如果要將權限變成『 -rwxr-xr-- 』呢？那麼權限的分數就成為 [4+2+1][4+0+1][4+0+0]=754 囉！所以你需要下達『 chmod 754 filename』。 另外，在實際的系統運作中最常發生的一個問題就是，常常我們以vim編輯一個shell的文字批次檔後，他的權限通常是 -rw-rw-r-- 也就是664， 如果要將該檔案變成可執行檔，並且不要讓其他人修改此一檔案的話， 那麼就需要-rwxr-xr-x這樣的權限，此時就得要下達：『 chmod 755 test.sh 』的指令囉！

另外，如果有些檔案你不希望被其他人看到，那麼應該將檔案的權限設定為例如：『-rwxr-----』，那就下達『 chmod 740 filename 』吧！
```

程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <sys/stat.h>  // mkdir的head file
using namespace std;
int main() {
  int ret = mkdir("/tmp/aaa", 0777);
  cout << "ret = " << ret << endl;
  return 0;
}
{% endhighlight %}
```
ret = 0
```

[1]: {% link _pages/c/compile/makefile.md %}
