---
title: File
date: 2025-05-07
keywords: Java, File
---
## 語法
```
File(String 檔案路徑與檔名);
File(String 目錄名, String 檔名);
File(File 目錄, String 檔名);
```

## 取得檔案資訊
{% highlight java linenos %}
public class FileTest {
  public static void main(String[] args) {
    File file = new File("/Users/cici/testc", "file_test");
    System.out.println(file.getName());
    // 文件長度
    System.out.println(file.length());
    // 文件路徑
    System.out.println(file.getAbsolutePath());
  }
}
{% endhighlight %}
```
file_test
6
/Users/cici/testc/file_test
```

## 建立檔案
Java輸出入操作一定要用try catch，所以建立檔案時，要try catch錯誤，並且要判斷是否重覆建立。
{% highlight java linenos %}
  try {
    // 判斷是否重覆建立
    if (!file2.exists()) {
      file2.createNewFile();
    }
  } catch (IOException e) {
    throw new RuntimeException(e);
  }
{% endhighlight %}

## 取得目錄下的檔案
file.list()方法傳回值為String陣列
{% highlight java linenos %}
String[] list()
{% endhighlight %}

取得目錄下的檔案
{% highlight java linenos %}
  // 目錄
  File file4 = new File("/Users/cici/testc");
  String[] filenames2 = file4.list();
  for (String name: filenames2) {
    System.out.println(name);
  }
{% endhighlight %}
```
testc.c
libTest.a
test
file_test2
test.o
file_test
testc.cpp
testc.o
libTest.so
testc
````
## 取得目錄下的檔案(限定附檔名)
先建立FileAccept，要去實作FilenameFilter，覆寫accept()方法。
{% highlight java linenos %}
import java.io.File;
import java.io.FilenameFilter;

public class FileAccept implements FilenameFilter {
  @Override
  public boolean accept(File dir, String name) {
    return name.endsWith(".c");
  }
}
{% endhighlight %}

使用FileAccept
{% highlight java linenos %}
  File file3 = new File("/Users/cici/testc");
  FileAccept fileAccept = new FileAccept();
  // 使用fileAccept
  String[] filenames = file3.list(fileAccept);
  for (String name: filenames) {
    System.out.println(name);
  }
{% endhighlight %}
```
testc.c
````




