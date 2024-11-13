---
title: 關閉RVO
date: 2024-11-13
keywords: c++, rvo
---

## 什麼是RVO

「返回值優化」（Return Value Optimization, RVO）或「拷貝省略」技術。

```
Student student = Student();
```

這行程式碼的作用是使用「臨時物件」（temporary object）初始化 student。但有以下幾個重點：

- 臨時物件的優化（Return Value Optimization, RVO）：在這段程式碼中，編譯器通常會應用「返回值優化」（RVO）或「拷貝省略」技術，這樣一來，它會直接在 student 的記憶體位置構建物件，而不是先創建一個臨時的 Student 物件然後再拷貝給 student。因此在實際運行中，不會產生臨時物件，而是直接將 Student() 產生的結果構建到 student 中。

- 不是指派記憶體位址：這段程式碼不是把臨時物件的記憶體位址「指派」給 student。它其實是構建了一個新的 Student 物件並用來初始化 student。而且，RVO 技術使得這個過程中不會額外創建臨時物件。

- 簡化的初始化語法：Student student = Student(); 的效果其實等同於 Student student; 或 Student student{};，因為它們都是呼叫 Student 類別的預設建構子來初始化 student。

- Student student = Student(); 這段程式碼在大多數情況下不會創建額外的臨時物件。編譯器會透過優化直接將 Student() 建構的物件初始化到 student，而不是先生成臨時物件再指派給 student。

- 在沒有優化的情況下，Student() 會生成一個臨時 Student 對象，並在返回時拷貝或移動到主程式中的變數 student 中。

- 但在現代編譯器中，大多數情況下會應用「返回值優化」（Return Value Optimization, RVO）或「拷貝省略」技術。這樣一來，編譯器會直接在 student 變數所在的記憶體位置構建返回的 Student 物件，避免產生額外的臨時物件。這樣的優化讓返回的過程更加高效。

## xcode關閉RVO

### 調降成c++11

![img]({{site.imgurl}}/xcode_c++/rvo1.png)

### 關閉RVO

![img]({{site.imgurl}}/xcode_c++/rvo2.png)

## 關閉RVO指令

```
g++ main.cpp -o main -fno-elide-constructors
```