---
title: KMP
date: 2025-08-29
keywords: java, KMP
---
{% highlight java linenos %}
public class Kmp {
  public static void main(String[] args) {
    String str = "aabaa";
    String pattern = "aaa";
    int[] next = getNext(pattern);
    System.out.println(Arrays.toString(next));
    int i = 0, j = 0 ;
    int findIndex = -1;
    for(;i < str.length(); i++) {
      while (j > 0 && str.charAt(i) != pattern.charAt(j)) {
        j = next[j-1];
      }
      if (str.charAt(i) == pattern.charAt(j)) {
        j++;
      }
      if (j == pattern.length()) {
        findIndex = i - j + 1;
        break;
      }
    }
    if (findIndex != -1) {
      System.out.println("find index = " + findIndex);
    } else {
      System.out.println("can not find index.");
    }
  }
  public static int[] getNext(String pattern) {
    int[] next = new int[pattern.length()];
    int j = 0;
    next[0] = 0;
    for (int i = 1; i < pattern.length(); i++) {
      while (j > 0 && pattern.charAt(j) != pattern.charAt(i)) {
        j = next[j - 1];
      }
      if (pattern.charAt(i) == pattern.charAt(j)) {
        j++;
      }
      next[i] = j;
    }
    return next;
  }
}
{% endhighlight %}