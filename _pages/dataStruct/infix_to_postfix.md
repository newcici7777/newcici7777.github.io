---
title: 中序轉後序
date: 2025-09-11
keywords: java, stack, infix to postfix
---
## 把中序存成list
```
(53+(34 * 45))
```
把中序運算式轉成list，要考慮十位、百位以上的數字，例:53,100<br>
如果字母介於0 - 9，使用while把十位與個位拚接在temp變數中。<br>
{% highlight java linenos %}
String temp = "";
while (i < str.length() && 
  str.charAt(i) >= '0' && str.charAt(i) <= '9') {
  temp += str.charAt(i);
  i++;
}
{% endhighlight %}

### 中序轉後序步驟
準備二個容器，下圖中左邊是Stack，右邊是List。<br>
Stack是放「加減乘除」與左括號`(`，List是放數字<br>
把中序運算式遍歷一遍，加減乘除放入左邊List，數字放右邊List。<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix1.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix2.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix3.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix4.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix5.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix6.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix7.png)<br>

右括號，把「加減乘除」符號放入List中，直到遇見左括號`(`停止。<br>
![img]({{site.imgurl}}/java_datastruct/in_pofix8.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix9.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix10.png)<br>

![img]({{site.imgurl}}/java_datastruct/in_pofix11.png)<br>

### 判斷數字與左右括號
準備二個容器，一個是Stack，一個是List。<br>
Stack是放「加減乘除」與左括號`(`，List是放數字<br>

### 數字
數字直接加入list，記得正規式後面要有\+加號，`\\d+`代表不只一個數字，而是由1至多個數字組成。<br>
{% highlight java linenos %}
if (str.matches("\\d+")) {
   list.add(str);
}
{% endhighlight %}

### 左括號
Stack是放「加減乘除」與左括號`(`。<br>
把左括號加入stack。<br>
{% highlight java linenos %}
if (str.equals("(")) {
   stack1.add(str);
}
{% endhighlight %}

### 右括號
peek是檢查堆疊頂端元素，並非pop刪除元素。<br>
{% highlight java linenos %}
stack1.peek()
{% endhighlight %}

把「加減乘除」符號放入List中，直到遇見左括號`(`停止。<br>
{% highlight java linenos %}
while (!stack1.peek().equals("(")) {
  list.add(stack1.pop());
}
{% endhighlight %}

最後要把左括號`(`，pop出去。
{% highlight java linenos %}
stack1.pop();
{% endhighlight %}

### 加減乘除判斷優先順序
數字愈大代表優先等級愈高，乘與除的的優先順序最高，要注意的是，左括號`(`仍在stack中，所以遇到左括號要傳回0。<br>
{% highlight java linenos %}
  public static int piorty(String str) {
    if (str.equals("+") || str.equals("-")) {
      return 1;
    } else if (str.equals("*") || str.equals("/")) {
      return 2;
    } else { // 左括號
      return 0;
    }
  }
{% endhighlight %}

#### 判斷步驟
乘除優先次序最高，加減第二，左括號優先次序最低。<br>
下表數字愈大，代表優先次序愈高。<br>

|符號|優先次序|
|\+|1|
|-|1|
|\*|2|
|`/`|2|
|(|0|

下圖中，左邊是Stack，專門放加減乘除與左括號，右邊是List，一開始是放數字。<br>

\+ 要放入「加減乘除」的Stack中，但裡面已經有\*與除`/`，\+的優先次序比乘\*除`/`低，要先把乘\*除`/`從Stack pop出來，並加入右邊的數字List，才能把\+加入Stack中。
![img]({{site.imgurl}}/java_datastruct/infix_piorty1.png)<br>

![img]({{site.imgurl}}/java_datastruct/infix_piorty2.png)<br>

比\+大的優先次序都pop出來後，再把\+放進Stack中。
![img]({{site.imgurl}}/java_datastruct/infix_piorty3.png)<br>

如果str是「加減乘除」其中之一，判斷優先次序，優先次序低的，先把stack中比它大的都pop出來放入List，最後再把str(加減乘除其中之一)，放入stack中。
{% highlight java linenos %}
// 優先次序低的，先把stack中比str大的都pop出來放入List
while (stack1.size() != 0 && 
  piorty(str) < piorty(stack1.peek())) {
  list.add(stack1.pop());
}
// 最後再優先次序低的，放入stack中
stack1.add(str);
{% endhighlight %}

## 計算
之前的步驟，會建立後序的數字與符號，把它遍歷一遍。<br>

建立一個Stack，只裝數字，遇到加減乘除，把Stack頂端前2個數字拿出來計算。<br>

### 計算步驟
![img]({{site.imgurl}}/java_datastruct/pofix_cul1.png)<br>

![img]({{site.imgurl}}/java_datastruct/pofix_cul2.png)<br>

![img]({{site.imgurl}}/java_datastruct/pofix_cul3.png)<br>

遇到加減乘除，把Stack頂端前2個數字拿出來計算。<br>
Stack最上面的，pop出來放\*的右邊。<br>
![img]({{site.imgurl}}/java_datastruct/pofix_cul4.png)<br>

第2個數字pop出來，放\*的左邊。<br>
![img]({{site.imgurl}}/java_datastruct/pofix_cul5.png)<br>

二個數字相乘之後，再把1530放入stack中。<br>
![img]({{site.imgurl}}/java_datastruct/pofix_cul6.png)<br>

遇到加減乘除，把Stack頂端前2個數字拿出來計算。<br>
Stack最上面的，pop出來放\+的右邊。<br>
![img]({{site.imgurl}}/java_datastruct/pofix_cul7.png)<br>

第2個數字pop出來，放\+的左邊。<br>
![img]({{site.imgurl}}/java_datastruct/pofix_cul8.png)<br>

二個數字相加之後，再把1583放入stack中。
![img]({{site.imgurl}}/java_datastruct/pofix_cul9.png)<br>

### 數字
若為數字，就放入Stack。
{% highlight java linenos %}
if (str.matches("\\d+")) {
  stack.add(str);
} 
{% endhighlight %}

### Stack前2個數字
若是加減乘除，把Stack前2個數字拿出來。<br>
遇到減號，`stack[top - 1] - stack[top]`。<br>
top頂端的後面一位的數 - top頂端。<br>
{% highlight java linenos %}
// pop出頂端
int i2 = Integer.parseInt(stack.pop());
// pop出頂端的下一個
int i1 = Integer.parseInt(stack.pop());
{% endhighlight %}

### 加減乘除
判斷加減乘除
{% highlight java linenos %}
int result = 0;
if (str.equals("+")) {
  result = i1 + i2;
} else if (str.equals("-")) {
  result = i1 - i2;
} else if (str.equals("*")) {
  result = i1 * i2;
} else if (str.equals("/")) {
  result = i1 / i2;
}
{% endhighlight %}

### 計算結果放回Stack
{% highlight java linenos %}
stack.add(result + "");
{% endhighlight %}

完整程式碼
{% highlight java linenos %}
public class InfixToPostfix {
  public static void main(String[] args) {
    String infix = "(53+(34 * 45))";
    List<String> infixList = parseInfix(infix);
    System.out.println(infixList);
    Stack<String> stack1 = new Stack<>();
    List<String> list = new ArrayList<>();
    for (String str: infixList) {
      if (str.matches("\\d+")) {
        list.add(str);
      } else if (str.equals("(")) {
        stack1.add(str);
      } else if (str.equals(")")) {
        while (!stack1.peek().equals("(")) {
          list.add(stack1.pop());
        }
        stack1.pop();
      } else {
        while (stack1.size() != 0 && piorty(str) < piorty(stack1.peek())) {
          list.add(stack1.pop());
        }
        stack1.add(str);
      }
    }
    while (stack1.size() != 0) {
      list.add(stack1.pop());
    }
    System.out.println(list);
    int calculate = calculate(list);
    System.out.println(calculate);
  }

  public static List parseInfix(String str) {
    List<String> rtn = new ArrayList<>();
    int i = 0;
    while (i < str.length()) {
      if (str.charAt(i) < '0' || str.charAt(i) > '9') {
        rtn.add(str.charAt(i) + "");
        i++;
      } else {
        String temp = "";
        while (i < str.length() && str.charAt(i) >= '0' && str.charAt(i) <= '9') {
          temp += str.charAt(i);
          i++;
        }
        rtn.add(temp);
      }
    }
    return rtn;
  }

  public static int piorty(String str) {
    if (str.equals("+") || str.equals("-")) {
      return 1;
    } else if (str.equals("*") || str.equals("/")) {
      return 2;
    } else {
      return 0;
    }
  }

  public static int calculate(List<String> list) {
    Stack<String> stack = new Stack<>();
    for (String str: list) {
      if (str.matches("\\d+")) {
        stack.add(str);
      } else {
        int i2 = Integer.parseInt(stack.pop());
        int i1 = Integer.parseInt(stack.pop());
        int result = 0;
        if (str.equals("+")) {
          result = i1 + i2;
        } else if (str.equals("-")) {
          result = i1 - i2;
        } else if (str.equals("*")) {
          result = i1 * i2;
        } else if (str.equals("/")) {
          result = i1 / i2;
        }
        stack.add(result + "");
      }
    }
    return Integer.parseInt(stack.pop());
  }
}
{% endhighlight %}
```
[(, 53, +, (, 34, *, 45, ), )]
[53, 34, 45, *, +]
1583
```