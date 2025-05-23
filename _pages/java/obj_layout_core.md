---
title: Java Object Layout
date: 2025-05-23
keywords: java, Java Object Layout, jol-core
---
以下步驟是查看物件佔用記憶體大小(byte)。

下載[jol-core.jar](https://mvnrepository.com/artifact/org.openjdk.jol/jol-core/0.17)，點擊下圖方框的部分。

![img]({{site.imgurl}}/java/jol-core.png)

![img]({{site.imgurl}}/java/sdk1.png)

加入jol-core.jar

![img]({{site.imgurl}}/java/module_add1.png)

![img]({{site.imgurl}}/java/module_add2.png)

![img]({{site.imgurl}}/java/module_add3.png)

包裝類別的佔用記憶體大小
{% highlight java linenos %}
import org.openjdk.jol.info.ClassLayout;

public class Test {
  public static void main(String[] args) {
    System.out.println(ClassLayout.parseInstance((byte)1).toPrintable());      // Byte
    System.out.println(ClassLayout.parseInstance('a').toPrintable());         // Character
    System.out.println(ClassLayout.parseInstance(1).toPrintable());           // Integer
    System.out.println(ClassLayout.parseInstance(1L).toPrintable());          // Long
    System.out.println(ClassLayout.parseInstance(1.0f).toPrintable());       // Float
    System.out.println(ClassLayout.parseInstance(1.0).toPrintable());        // Double
  }
}
{% endhighlight %}
```
=============Boolean===================
# WARNING: Unable to get Instrumentation. Dynamic Attach failed. You may add this JAR as -javaagent manually, or supply -Djdk.attach.allowAttachSelf
# WARNING: Unable to attach Serviceability Agent. You can try again with escalated privileges. Two options: a) use -Djol.tryWithSudo=true to try with sudo; b) echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
java.lang.Boolean object internals:
OFF  SZ      TYPE DESCRIPTION               VALUE
  0   8           (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4           (object header: class)    0x00006390
 12   1   boolean Boolean.value             true
 13   3           (object alignment gap)    
Instance size: 16 bytes
Space losses: 0 bytes internal + 3 bytes external = 3 bytes total

=============Character===================
java.lang.Character object internals:
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x0000002d6b418f01 (hash: 0x2d6b418f; age: 0)
  8   4        (object header: class)    0x00023288
 12   2   char Character.value           a
 14   2        (object alignment gap)    
Instance size: 16 bytes
Space losses: 0 bytes internal + 2 bytes external = 2 bytes total

=============Byte===================
java.lang.Byte object internals:
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x00000033affdfa01 (hash: 0x33affdfa; age: 0)
  8   4        (object header: class)    0x00033da8
 12   1   byte Byte.value                1
 13   3        (object alignment gap)    
Instance size: 16 bytes
Space losses: 0 bytes internal + 3 bytes external = 3 bytes total

=============short===================
java.lang.Short object internals:
OFF  SZ    TYPE DESCRIPTION               VALUE
  0   8         (object header: mark)     0x0000007413b5b301 (hash: 0x7413b5b3; age: 0)
  8   4         (object header: class)    0x00035c40
 12   2   short Short.value               1
 14   2         (object alignment gap)    
Instance size: 16 bytes
Space losses: 0 bytes internal + 2 bytes external = 2 bytes total

=============Integer===================
java.lang.Integer object internals:
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x00000063c12bbe01 (hash: 0x63c12bbe; age: 0)
  8   4        (object header: class)    0x00026830
 12   4    int Integer.value             1
Instance size: 16 bytes
Space losses: 0 bytes internal + 0 bytes external = 0 bytes total

=============Long===================
java.lang.Long object internals:
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x0000001d9ff05801 (hash: 0x1d9ff058; age: 0)
  8   4        (object header: class)    0x00037950
 12   4        (alignment/padding gap)   
 16   8   long Long.value                1
Instance size: 24 bytes
Space losses: 4 bytes internal + 0 bytes external = 4 bytes total

=============Float===================
java.lang.Float object internals:
OFF  SZ    TYPE DESCRIPTION               VALUE
  0   8         (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4         (object header: class)    0x00031228
 12   4   float Float.value               1.0
Instance size: 16 bytes
Space losses: 0 bytes internal + 0 bytes external = 0 bytes total

=============Double===================
java.lang.Double object internals:
OFF  SZ     TYPE DESCRIPTION               VALUE
  0   8          (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4          (object header: class)    0x00032820
 12   4          (alignment/padding gap)   
 16   8   double Double.value              1.0
Instance size: 24 bytes
Space losses: 4 bytes internal + 0 bytes external = 4 bytes total

```