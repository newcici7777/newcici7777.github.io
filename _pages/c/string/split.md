---
title: split 切割字串
date: 2024-09-06
keywords: c++, split 
---

切割字串 = aa,bb,cc,dd,ee,ff

切割符號 = ,

儲存二維陣列 values[10][50]


第一次循環

|字串|aa|,|bb|,|cc|,|dd|,|ee|,|ff|
|p|^|||||||||||
|find||^||||||||||


第二次循環

|字串|aa|,|bb|,|cc|,|dd|,|ee|,|ff|
|p|||^|||||||||
|find||||^||||||||


最後一次循環

|字串|aa|,|bb|,|cc|,|dd|,|ee|,|ff|
|p|||||||||||^|
|find||||||||||||


完整程式碼

{% highlight c++ linenos %}
/**
 str 要切割的字串
 splitStr 切割符號
 values儲存的結果陣列
**/
size_t splitstr(const char* str,const char* splitStr, char values[][50]) {
    //若位址為空，回傳0
    if(str == 0 || splitStr == 0) return 0;
    size_t slen = strlen(splitStr);//split符號長度
    if(slen == 0) return 0;//若符號長度為0，就不用切割
    //強制轉型const char* str轉成char*
    char *p = (char*)str;
    //aa,bb,cc,dd,ee,ff
    //切割符號為逗號，切割的個數為6個，分別為aa|bb|cc|dd|ee|ff
    size_t size = 0;//記錄切割的個數
    while(true) {
        //找尋切割符號第一個位置
        char* find = strstr(p, splitStr);
        //如果找到
        if(find != 0) {
            //截取p的位置到切割符號之前的字元個數
            //參數1要儲存的位址
            //參數2要拷貝的開始位址
            //參數3要拷貝的個數
            strncpy(values[size++], p, find - p);
            //將p指標移到切割符號右邊的字元
            p = find + slen;
        } else {
            //如果找不到切割符號
            //代表是最後一個字串
            strcpy(values[size++], p);
            break;//跳出無限迴圈
        }
    }
    return size;
}
int main() {
    char values[10][50];
    memset(values, 0, sizeof(values));
    size_t count = splitstr("aa,bb,cc,dd,ee,ff", ",", values);
    cout << "count = " << count << endl;
    for(int i = 0; i < count; i++) {
        cout << "values[" << i << "] = " << values[i] << endl;
    }
    return 0;
}
{% endhighlight %}