---
title: File IO
date: 2024-12-27
keywords: c++, file io
---

![img]({{site.imgurl}}/c++/file_io_tree.jpg)  

o是output簡寫，輸出

i是input簡寫，輸入

f是file文字檔

上圖是各個輸入串流與輸出串流繼承狀況，本頁著重在fstream(讀取與寫入文字檔)與ifstream(讀取文字檔)與ofstream(寫入文字檔)。

## 寫入文字檔

步驟有4步

### include
```
#include <fstream>
```

### 建立寫入文字檔的物件

寫入文字檔用ofstream(output file stream)。
{% highlight c++ linenos %}
  ofstream fout;
{% endhighlight %}

### 打開文字檔
{% highlight c++ linenos %}
fout.open(file_name, ios::trunc);
{% endhighlight %}
- 參數1是檔名
- 參數2是開啟文字檔的模式
	- 不填，使用預設ios::out
	- ios::trunc
	- ios:out
	- ios::app

ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本文字檔內容

ios::app是append附加在文字檔內容後面

### 用ofstreamt物件打開文字檔

ofstream物件本身也可以打開文字檔，使用以下語法，就不需要使用fout.open()打開，可以取代fout.open()
{% highlight c++ linenos %}
  ofstream fout(file_name, ios::app);
{% endhighlight %}
- 參數1是檔名
- 參數2是開啟文字檔的模式
	- 不填，使用預設ios::out
	- ios::trunc
	- ios:out
	- ios::app

### 判斷是否打開成功
使用is_open()函式判斷是否有下列問題，若有以下問題會傳回true。
- 目錄不存在
- 硬碟容量不足
- 權限不夠
{% highlight c++ linenos %}
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
{% endhighlight %}

### 寫入文字檔
寫入(用法與cout一樣，cout<<是輸出到螢幕，fout<<是輸出到文字檔)
{% highlight c++ linenos %}
fout << "file 1 test test \n";
{% endhighlight %}

### 關閉文字檔
{% highlight c++ linenos %}
fout.close();
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test.txt";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout;
  // 2.打開檔案，若無檔案會建立新
  // ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本檔案內容
  // ios::app是append附加在檔案內容後面
  fout.open(file_name, ios::trunc);
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  // 3.寫入(用法與cout一樣，cout<<是輸出到螢幕，fout<<是輸出到檔案)
  fout << "file 1 test test\n";
  fout << "file 2 test test\n";
  fout << "file 3 test test\n";
  // 4.關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}

## 讀取文字檔

### include
```
#include <fstream>
```

### 建立讀取文字檔的物件

讀取文字檔用ifstream(input file stream)。
{% highlight c++ linenos %}
  ifstream fin;
{% endhighlight %}

### 打開文字檔
{% highlight c++ linenos %}
fin.open(file_name, ios::in);
{% endhighlight %}
- 參數1是檔名
- 參數2是開啟文字檔的模式
	- 不填，使用預設ios::in
	- ios::in

### 用ifstream物件打開文字檔
ifstream物件本身也可以打開文字檔，使用以下語法，就不需要使用fin.open()打開，可以取代fin.open()
{% highlight c++ linenos %}
  ofstream fin(file_name, ios::in);
{% endhighlight %}
- 參數1是檔名
- 參數2是開啟文字檔的模式
	- 不填，使用預設ios::in
	- ios::in

### 判斷是否打開成功
使用is_open()函式判斷是否有下列問題，若有以下問題會傳回true。
- 文字檔不存在
- 硬碟容量不足
- 權限不夠
{% highlight c++ linenos %}
  if (fin.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
{% endhighlight %}

### 讀取一列文字檔
{% highlight c++ linenos %}
  // 用於存放讀取的內容
  string buffer;
  // 讀取一列字串，存放到buffer中
  getline(fin, buffer);
{% endhighlight %}

### while讀取整個文字檔
前一個例子只能讀取一列，使用while()讀取整個文字檔，若讀不到文字，就會傳回nullptr
{% highlight c++ linenos %}
  // 用於存放讀取的內容
  string buffer;
  // 讀取一列字串，存放到buffer中
  while (getline(fin, buffer)) {
    cout << buffer << endl;
  }
{% endhighlight %}
```
file 1 test test 
file 2 test test 
file 3 test test 
file 1 test test 
file 2 test test 
file 3 test test 
```

### 關閉文字檔
{% highlight c++ linenos %}
fin.close();
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <string>  // getline()函式需要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test.txt";
  ifstream fin(file_name, ios::in);
  if (fin.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  // 用於存放讀取的內容
  string buffer;
  // 讀取一列字串，存放到buffer中
  while (getline(fin, buffer)) {
    cout << buffer << endl;
  }
  // 關閉檔案
  fin.close();
  return 0;
}
{% endhighlight %}

## 寫入Binary file
Binary file中文翻譯可為二進位檔案，它跟文字檔不同

### 文字檔
string str = "123";

大小3byte

|字元|\'1\'|\'2\'|\'3\'|
|ascii|49|50|51|

記憶體實際存放的是二進位資料如下:

|二進位|00110001|00110010|00110011|

文字檔以字串一列一列組成，讀取也以一列一列的方式讀取。

### Binary file
int i = 12345678;

int大小4byte，記憶體實際存放的是二進位資料如下:

|二進位|00000000|10111100|01100001|01001110|

二進位檔案可以存放陣列、結構、類別...什麼類型都可以，圖片檔、mp3檔、mp4這些都是Binary file。

### 打開Binary file

{% highlight c++ linenos %}
  ofstream fout(file_name, ios::binary);
{% endhighlight %}
- 參數1是檔名
- 參數2是ios::binary打開Binary file

{% highlight c++ linenos %}
  ofstream fout(file_name, ios::app | ios::binary);
{% endhighlight %}

ios::app \| ios::binary意思是，寫入檔案從檔案最後開始寫。

{% highlight c++ linenos %}
  ofstream fout(file_name, ios::trunc | ios::binary);
{% endhighlight %}

ios::trunc \| ios::binary意思是，寫入檔案用覆蓋原本檔案的方式寫入。

### 建立內容格式
假設要寫入類別，注意，不能用string，string 類型的資料無法直接用 write 和 read 操作進行正確的二進制序列化和反序列化。
{% highlight c++ linenos %}
  class Student{
   public:
    char name[100];
    int age;
  };
  Student s1;
  strcpy(s1.name, "Mary");
  s1.age = 15;
{% endhighlight %}

### 寫入write

{% highlight c++ linenos %}
  // 使用write(位址, size)寫入
  // 參數1:物件位址，需手動轉型成const char*
  // 參數2:物件大小
  fout.write((const char*)&s1, sizeof(s1));
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <cstring>  // strcpy()要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.exe";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout(file_name, ios::trunc | ios::binary);
  if (fout.is_open() == false) {
    cout << "開啟檔案失敗" << endl;
    return 0;
  }
  class Student{
   public:
    char name[100];
    int age;
  };
  Student s1;
  strcpy(s1.name, "Mary");
  s1.age = 15;
  // 使用write(位址, size)寫入
  // 參數1:物件位址，需手動轉型成const char*
  // 參數2:物件大小
  fout.write((const char*)&s1, sizeof(s1));
  // 4.關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}

## 讀取Binary file

### 打開Binary file
{% highlight c++ linenos %}
  string file_name = "/home/cici/test/app/test1.exe";
  ifstream fin(file_name, ios::in | ios::binary);
{% endhighlight %}

使用ios::in \| ios::binary打開Binary file

### 讀取read
```
fin.read((char*)位址, 大小);
```
將物件位址手動轉型成(char\*)

{% highlight c++ linenos %}
  // 怎麼寫入怎麼讀出來
  class Student{
    public:
     char name[100];
     int age;
  };
  Student s1;
  fin.read((char*)&s1, sizeof(s1));
  cout << "name = " << s1.name << ", age = " << s1.age << endl;
{% endhighlight %}

以上範例是只有寫入一個Student物件，若寫N個Student物件，可用while讀出來。
{% highlight c++ linenos %}
while (fin.read((char*)&s1, sizeof(s1))) {
	cout << "name = " << s1.name << ", age = " << s1.age << endl;
}
{% endhighlight %}

### 完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.exe";
  ifstream fin(file_name, ios::in | ios::binary);
  if (fin.is_open() == false) {
    cout << "開啟檔案失敗" << endl;
    return 0;
  }
  // 怎麼寫入怎麼讀出來
  class Student{
    public:
     char name[100];
     int age;
  };
  Student s1;
  fin.read((char*)&s1, sizeof(s1));
  cout << "name = " << s1.name << ", age = " << s1.age << endl;
  // 關閉檔案
  fin.close();
  return 0;
}
{% endhighlight %}

## 檔案的游標位置
用記事本或sublime文件編輯軟體打開文字檔，一開始閃爍的游標會停在第0格的位置，將滑鼠游標移動到其它文字或其它列，在這邊所說的"游標"就是會回應使用者輸入的位置，而這個"位置""就是這小節的重點。

### 語法
取得游標位置函式，以下功能都相同
- ofstream.tellp()
- ifstream.tellg()
- fstream.tellp()
- fstream.tellg()

### 文字檔寫入與取得游標位置
將先前寫入文字檔的範例，增加取得游標位置函式，每換行一次就印出目前的游標位置
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test.txt";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout;
  // 2.打開檔案，若無檔案會建立新
  // ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本檔案內容
  // ios::app是append附加在檔案內容後面
  fout.open(file_name, ios::trunc);
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  // 3.寫入(用法與cout一樣，cout<<是輸出到螢幕，fout<<是輸出到檔案)
  cout << "游標位置 = " << fout.tellp() << endl;
  fout << "file 1 test test \n";
  cout << "游標位置 = " << fout.tellp() << endl;
  fout << "file 2 test test \n";
  cout << "游標位置 = " << fout.tellp() << endl;
  fout << "file 3 test test \n";
  cout << "游標位置 = " << fout.tellp() << endl;
  // 4.關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}
```
游標位置 = 0
游標位置 = 18
游標位置 = 36
游標位置 = 54
```
### 二進位檔案讀取與游標位置
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.exe";
  ifstream fin(file_name, ios::in | ios::binary);
  if (fin.is_open() == false) {
    cout << "開啟檔案失敗" << endl;
    return 0;
  }
  // 怎麼寫入怎麼讀出來
  class Student{
    public:
     char name[100];
     int age;
  };
  Student s1;
  cout << "游標位置 = " << fin.tellg() << endl;
  fin.read((char*)&s1, sizeof(s1));
  cout << "游標位置 = " << fin.tellg() << endl;
  cout << "name = " << s1.name << ", age = " << s1.age << endl;
  // 關閉檔案
  fin.close();
  return 0;
}
{% endhighlight %}
```
游標位置 = 0
游標位置 = 104
name = Mary, age = 15
```

## 隨機讀取與隨機寫入

### 移動游標位置
移動游標位置函式，以下功能都相同

std::istream & seekg(std::streampos _Pos);
- 參數_Pos為游標位置
- 傳回游標位置指標

以下都是移動游標位置的語法:
- ofstream.seekp(_Pos)
- ifstream.seekg(_Pos)
- fstream.seekp(_Pos)
- fstream.seekg(_Pos)


### 位置參數
- ios::beg，代表移動到檔案開始的位置
- ios::end，代表移動到檔案最末尾的位置
- ios::cur，代表目前游標在檔案中的位置

以上的值，都可作為上述_Pos的參數
{% highlight c++ linenos %}
// 移動到檔案最末尾的位置
fout.seekp(ios::end);
// 移動到100
fout.seekp(100);
{% endhighlight %}

### 移動游標位置2
{% highlight c++ linenos %}
std::istream & seekg(std::streamoff _Off,std::ios::seekdir _Way);
// 移動到檔案開始的位置，再往右邊移動6個字元
fin.seekg(6, ios::beg);
// 移動到游標位置，再往左移動5字元
fin.seekg(-5, ios::cur);
// 移動到游標位置，再往右移動8字元
fin.seekg( 8, ios::cur);
// 移動到檔案最末尾的位置，再往左移動10字元
fin.seekg(-10, ios::end);
{% endhighlight %}

### 移動游標程式碼
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <string>  // getline()函式需要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test.txt";
  ifstream fin(file_name, ios::in);
  if (fin.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  // 用於存放讀取的內容
  string buffer;
  // 移動到檔案開始的位置，再往右邊移動6個字元
  fin.seekg(6, ios::beg);
  // 讀取一列字串，存放到buffer中
  while (getline(fin, buffer)) {
    cout << buffer << endl;
  }
  // 關閉檔案
  fin.close();
  return 0;
}
{% endhighlight %}

原本文字檔如下:
```
file 1 test test 
file 2 test test 
file 3 test test
```
執行後結果，確實"file 1"共6個字元都沒有被顯示出來。
```
 test test 
file 2 test test 
file 3 test test 
```

## 緩衝區

暫時置放輸出或輸入資料的記憶體區域，也就是說寫入文件沒有即時寫入檔案，而是先放在記憶體區域，等待記憶體區域滿了，才會寫入檔案。

![img]({{site.imgurl}}/c++/file_buffer_w.jpg)  

以下程式碼可能可以證明緩衝區的存在。

開二個linux終端機，一個編譯並執行以下程式碼，另一個終端機輸入`tail -f test1.txt`

注意！<span class="markline">fout << 結尾不能是endl</span>因為endl本身自帶flush的功能。

{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <unistd.h>  // usleep()要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.txt";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout;
  // 2.打開檔案，若無檔案會建立新
  // ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本檔案內容
  // ios::app是append附加在檔案內容後面
  fout.open(file_name, ios::out);
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  for (int i = 0; i < 1000; i++) {
    // 寫入
    fout << "第 i = " << i << "次,abcdefghijklm.kljoppo kllkkjlkjjljjkldfjfdladfaklffadjkfladfjadklfalfakdfadlfjadkljfdakldfajfkfjkal \n";
    // usleep(微秒) 暫時使程式停止執行
    // 1秒 = 1,000,000 微秒
    // 0.1秒 = 100,000
    usleep(100000);
  }
  // 關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}

若執行到999次之間有停頓住，代表緩衝區還沒滿。

### flush
fout.flush()將緩衝區的資料立即寫入檔案，終端機使用`tail -f test1.txt`可以追蹤查看檔案是一列列顯示，而不會停頓一下又冒出一批資料出來。
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <unistd.h>  // usleep()要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.txt";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout;
  // 2.打開檔案，若無檔案會建立新
  // ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本檔案內容
  // ios::app是append附加在檔案內容後面
  fout.open(file_name, ios::out);
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  for (int i = 0; i < 1000; i++) {
    // 寫入
    fout << "第 i = " << i << "次,abcdefghijklm.kljoppo kllkkjlkjjljjkldfjfdladfaklffadjkfladfjadklfalfakdfadlfjadkljfdakldfajfkfjkal \n";
    fout.flush();
    // usleep(微秒) 暫時使程式停止執行
    // 1秒 = 1,000,000 微秒
    // 0.1秒 = 100,000
    usleep(100000);
  }
  // 關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}

### endl
除了斷行之外，還會將緩衝區的資料立即寫入檔案。
{% highlight c++ linenos %}
fout << "第 i = " << i << "次,abcdefghi" << endl;
{% endhighlight %}

{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <unistd.h>  // usleep()要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.txt";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout;
  // 2.打開檔案，若無檔案會建立新
  // ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本檔案內容
  // ios::app是append附加在檔案內容後面
  fout.open(file_name, ios::out);
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  for (int i = 0; i < 1000; i++) {
    // 寫入
    fout << "第 i = " << i << "次,abcdefghi" << endl;
    // usleep(微秒) 暫時使程式停止執行
    // 1秒 = 1,000,000 微秒
    // 0.1秒 = 100,000
    usleep(100000);
  }
  // 關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}

### unibuf
將緩衝區設置為有資料立即寫入檔案。
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <unistd.h>  // usleep()要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test1.txt";
  // 1.建立寫入檔案的物件(輸出串流)
  ofstream fout;
  // 2.打開檔案，若無檔案會建立新
  // ios::trunc與ios:out與預設不代入第2個參數，覆蓋(清除)原本檔案內容
  // ios::app是append附加在檔案內容後面
  fout.open(file_name, ios::out);
  fout << unitbuf;
  if (fout.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  for (int i = 0; i < 1000; i++) {
    // 寫入
    fout << "第 i = " << i << "次 \n";
    // usleep(微秒) 暫時使程式停止執行
    // 1秒 = 1,000,000 微秒
    // 0.1秒 = 100,000
    usleep(100000);
  }
  // 關閉檔案
  fout.close();
  return 0;
}
{% endhighlight %}

## 判斷輸入串流錯誤
### eof()
只有輸入串流有eofbit，輸出串流沒有。

當inputstream讀到檔案末尾會設定eofbit變數，eof()函式會檢查是否設定eofbit
{% highlight c++ linenos %}
#include <iostream>
#include <fstream>
#include <string>  // getline()函式需要用到
using namespace std;
int main() {
  // linux檔案位置
  string file_name = "/home/cici/test/app/test.txt";
  ifstream fin(file_name, ios::in);
  if (fin.is_open() == false) {
    cout << "開啟文字檔失敗" << endl;
    return 0;
  }
  // 用於存放讀取的內容
  string buffer;
  while (true) {
    fin >> buffer;
    cout << buffer << endl;
    if (fin.eof() == true) break;
  }
  // 關閉檔案
  fin.close();
  return 0;
}
{% endhighlight %}

以上的程式碼可被取代為
{% highlight c++ linenos %}
 while (fin >> buffer) {
    cout << buffer << endl;
 }
{% endhighlight %}

### bad()
bad()函式會檢查badbit是否設定，當硬碟空間不足，或系統有錯誤，就會被設定。

### fail()
fail()函式會檢查failbit是否設定，當檔案到了末尾，或程式有bug，就會被設定。

### good()
eofbit,badbit,failbit都是0的時候good()函式就會傳回true
