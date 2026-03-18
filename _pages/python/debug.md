---
title: debug 中斷點
date: 2026-03-17
keywords: Python, debug, break point
---
## Step over 執行一行
Step Over 的快速鍵是 F8，執行一行。<br>

以下程式碼，主程式會呼叫f1()函式，f1函式會呼叫f2()函式。
{% highlight python linenos %}
def f2():
    sum = 0
    for i in range(5):
        sum += i
        print(f"i = {i}")
    return sum


def f1():
    print("Hello")
    result = f2()
    print("result = ", result)

print("Start ...")
f1()
print("Finish ...")
{% endhighlight %}

以上的程式碼設置中斷點在`print("Start ...")`，執行debug，點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/step_over1.png)<br>

移到`f1()`，點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/step_over2.png)<br>

移到`print("Finish ...")`，不會進入f1()函式，而是執行完 f1() 函式內的程式碼，下方的圖片，點擊console，會輸出f1()與f2()函式執行結果。<br>
![img]({{site.imgurl}}/python/step_over3.png)<br>

## Step into 進入函式
Step into 的快速鍵是 F7，進入函式。

程式碼設置中斷點在`print("Start ...")`，執行debug，點擊鍵盤 F8(Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/step_over1.png)<br>

移到`f1()`，點擊鍵盤 F7(Step into)，進入函式。<br>
![img]({{site.imgurl}}/python/step_over2.png)<br>

就會進入到f1()函式，並移到f1()函式第一行`print("Hello")`，點擊鍵盤 F8 (Step Over)，執行此行<br>
![img]({{site.imgurl}}/python/step_into1.png)<br>

移到f1()函式`result = f2()`，點擊鍵盤 F7(Step into)，進入函式。<br>
![img]({{site.imgurl}}/python/step_into2.png)<br>

就會進入到f2()函式，並移到第一行`sum = 0`。<br>
![img]({{site.imgurl}}/python/step_into3.png)<br>

## Step out 離開函式
Step out 的快速鍵是 Shift + F8，離開函式。<br>

點擊鍵盤 Shift + F8(Step out)，離開函式。<br>
![img]({{site.imgurl}}/python/step_into3.png)<br>

移到f1()函式`result = f2()`。<br>
![img]({{site.imgurl}}/python/step_out1.png)<br>

點擊Console，會發現f2()的函式結果都執行在console，證明Step out離開f2()函式時，也會把f2()函式執行完畢。<br>
![img]({{site.imgurl}}/python/step_out2.png)<br>

點擊鍵盤 Shift + F8(Step out)，離開f1()函式。<br>

移回呼叫`f1()`函式的位置，點開console，也有輸出`result =  10`，證明Step out離開f1()函式時，也會把f1()函式執行完畢。<br>
![img]({{site.imgurl}}/python/step_out3.png)<br>

## Step out 執行並離開目前迴圈
在f2()函式中，在for 迴圈中的程式碼`sum += i` 設中斷點，執行Debug。<br>
點擊鍵盤 Shift + F8(Step out)，試圖離開函式。<br>
![img]({{site.imgurl}}/python/step_out_loop1.png)<br>

但發現無法離開f2()函式，i還變成1，因為Step out遇上迴圈，是執行完目前的迴圈，並離開目前的迴圈，移到下一個迴圈，所以i才會變成1。<br>
![img]({{site.imgurl}}/python/step_out_loop2.png)<br>

點擊鍵盤 Shift + F8(Step out)，執行並離開目前的迴圈，移到下一個迴圈，i變成2。<br>
![img]({{site.imgurl}}/python/step_out_loop3.png)<br>

若要離開f2()函式，將迴圈中的中斷點取消，再點擊鍵盤 Shift + F8 (Step out)，就會離開f2()函式。
![img]({{site.imgurl}}/python/step_out_loop4.png)<br>

移到一開始呼叫f2()的那行程式碼`result = f2()`。
![img]({{site.imgurl}}/python/step_out_loop5.png)<br>

點擊鍵盤 F8(Step Over)，就會移到下一行程式碼，不會再次Step into進入f2()函式。<br>
![img]({{site.imgurl}}/python/step_out_loop6.png)<br>

## index out of range debug
程式碼
{% highlight python linenos %}
list1 = ["Hello", "World"]
i = 0
while i <= len(list1):
    print(list1[i])
    i += 1
{% endhighlight %}

中斷點設在`i = 0`，執行Debug，點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/out_range1.png)<br>

移到`while i <= len(list1):`，此時 i 為 0。<br>
![img]({{site.imgurl}}/python/out_range2.png)<br>

滑鼠選取`len(list1)`，請仔細看出圖中`len(list1)`底色是偏深藍。<br>
![img]({{site.imgurl}}/python/out_range3.png)<br>

滑鼠右鍵，有`Evaluate Expression`的選項。<br>
![img]({{site.imgurl}}/python/out_range4.png)<br>

點擊`Evalute`按鈕。<br>
![img]({{site.imgurl}}/python/out_range5.png)<br>

`len(list1)`的運算結果為`result = (int)2`，2就是結果。<br>
![img]({{site.imgurl}}/python/out_range6.png)<br>

回到`while i <= len(list1):`，點擊鍵盤 F8 (Step Over)，執行此行。<br>
移到`print(list1[i])`，點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/out_range7.png)<br>

移到`i += 1`，點擊鍵盤 F8 (Step Over)，執行此行。<br>
點擊`Console`，會發現已輸出`Hello`。<br>
![img]({{site.imgurl}}/python/out_range8.png)<br>

回到`while i <= len(list1):`，此時 i 變為 1 。<br>
點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/out_range9.png)<br>

移到`print(list1[i])`，點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/out_range10.png)<br>

點擊`Console`，會發現已輸出`World`。<br>
點擊鍵盤 F8 (Step Over)，執行`i += 1`。<br>
![img]({{site.imgurl}}/python/out_range11.png)<br>

回到`while i <= len(list1):`，此時 i 變為 2 。<br>
點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/out_range12.png)<br>

移到`print(list1[i])`，點擊鍵盤 F8 (Step Over)，執行此行。<br>
![img]({{site.imgurl}}/python/out_range13.png)<br>

會發現有閃電符號在`print(list1[i])`這行，代表此行執行有錯誤。<br>
Console也會有IndexError的訊息。<br>
![img]({{site.imgurl}}/python/out_range14.png)<br>

## Resume F9 跳到中斷點執行
點擊鍵盤 F9 (Resume)，直接跳到中斷點執行。<br>