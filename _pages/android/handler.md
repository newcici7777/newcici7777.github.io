---
title: Handler訊息溝通
date: 2025-04-17
keywords: Android, handler, Message, runOnUiThread, view.post()
---
Prerequisites:

- [Thread][1]
- [Android Main Thread][2]

## Handler
當背景執行緒(background Thread)處理完耗時操作，要通知UI執行緒(主要執行緒Main Thread)處理畫面更新，就需要使用到handler來通知UI執行緒來更新畫面。

建立handler
{% highlight java linenos %}
Handler handler = new Handler();
{% endhighlight %}

Handler主要做二件事:通知、接收訊息

## 建立背景執行緒(background Thread)
建立背景執行緒(background Thread)可以透過new thread或建立runnable物件，二種方式建立背景執行緒(background Thread)

{% highlight java linenos %}
new Thread() {
  @Override
  public void run() {
    ...
  }
}.start();
{% endhighlight %}

{% highlight java linenos %}
new Thread(new Runnable() {
  @Override
  public void run() {
    ...
  }
}).start();
{% endhighlight %}

## 訊息Message
建立訊息語法
```
Message message = new Message();
message.what = 1;
handler.sendMessage(message);
```
所謂的what，可視為訊息的id編號，代表我這個訊息是1號

## Handler通知
背景執行緒background thread可使用handler.sendMessage()通知ui thread要更新ui畫面。  
{% highlight java linenos %}
new Thread() {
  @Override
  public void run() {
  	// 建立訊息
    Message message = new Message();
    // 建立訊息編號id
    message.what = 1;
    // 通知ui thread，要更新ui，sendMessage()參數為先前建立的訊息
    handler.sendMessage(message);
  }
}.start();
{% endhighlight %}

## Handler收到通知
onCreate()是屬於UI Thread，UI Thread中handler收到message通知。  
取出message，再取出what，根據不同的訊息id，處理相對映的更新畫面動作。  
{% highlight java linenos %}
private Handler handler;
@Override
protected void onCreate(Bundle savedInstanceState) {
  handler = new Handler() {
    @Override
    public void handleMessage(@NonNull Message msg) {
      switch (msg.what) {
        case 1:
          Toast.makeText(MainActivity.this, "test", Toast.LENGTH_SHORT).show();
          Log.e(TAG,"handler test");
          break;
      }
    }
  };
{% endhighlight %}

## 完整程式碼
{% highlight java linenos %}
private Handler handler;
@Override
protected void onCreate(Bundle savedInstanceState) {
  // 建立訊息接收者
  handler = new Handler() {
    // 收到訊息
    @Override
    public void handleMessage(@NonNull Message msg) {
      // 根據訊息編號判斷要做什麼畫面更新的流程
      switch (msg.what) {
        case 1: //訊息編號為1
          Toast.makeText(MainActivity.this, "test", Toast.LENGTH_SHORT).show();
          Log.e(TAG,"handler test");
          break;
      }
    }
  };
  // 建立背景執行緒
  new Thread() {
    @Override
    public void run() {
      // 建立訊息
      Message message = new Message();
      // 訊息編號
      message.what = 1;
      // 傳送訊息，通知ui thread更新畫面
      handler.sendMessage(message);
    }
  }.start();// 啟動背景執行緒
 }
{% endhighlight %}

## message.obj
可以在message加上訊息。  
語法如下:  
{% highlight java linenos %}
String data = "12345";
Message message = new Message();
message.what = 1;
message.obj = data
{% endhighlight %}

{% highlight java linenos %}
  public void testHandler() {
    new Thread() {
      @Override
      public void run() {
        String data = "12345";
        // 建立訊息
        Message message = new Message();
        // 建立訊息編號id
        message.what = 1;
        // 建立物件
        message.obj = data;
        // 通知ui thread，要更新ui，sendMessage()參數為先前建立的訊息
        handler.sendMessage(message);
      }
    }.start();
  }
  public void btnClick(View view) {
    testHandler();
  }  
{% endhighlight %}

{% highlight java linenos %}
  handler = new Handler() {
    // 收到訊息
    @Override
    public void handleMessage(@NonNull Message msg) {
      // 根據訊息編號判斷要做什麼畫面更新的流程
      switch (msg.what) {
        case 1: //訊息編號為1
          String data = (String) msg.obj;
          Toast.makeText(MainActivity.this, "test" + data, Toast.LENGTH_SHORT).show();
          Log.e(TAG, "handler test data = " + data);
          break;
        case 2:
          break;
      }
    }
  };
{% endhighlight %}

## sendEmptyMessage()
語法  
```
handler.sendEmptyMessage(整數的編號);
handler.sendEmptyMessage(1);
```
參數為訊息的編號

可以省略new Message()...等等相關流程，一樣有同樣的效果。
{% highlight java linenos %}
  new Thread() {
    @Override
    public void run() {
      // 傳送訊息，通知ui thread更新畫面
      handler.sendEmptyMessage(1);
    }
  }.start();// 啟動背景執行緒
{% endhighlight %}

## sendEmptyMessageDelayed()
幾秒後像handler發送訊息

語法  
```
handler.sendEmptyMessageDelayed(整數的編號, 幾秒後發送);
handler.sendEmptyMessage(2, 2*1000);
```
- 參數1:訊息編號
- 參數2:單位是毫秒，1秒是1000毫秒，此範例使用2秒也就是2000毫秒

ProgressBar xml
{% highlight html linenos %}
<ProgressBar
    android:id="@+id/progressBar1"
    style="?android:attr/progressBarStyleHorizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100"
    android:visibility="gone"
    app:layout_constraintBottom_toTopOf="@+id/button"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent" />
{% endhighlight %}

{% highlight java linenos %}
private ProgressBar progressBar1;
// 進度條變數
private int progress = 0;
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  progressBar1 = (ProgressBar) findViewById(R.id.progressBar1);
  handler = new Handler() {
    // 收到訊息
    @Override
    public void handleMessage(@NonNull Message msg) {
      // 根據訊息編號判斷要做什麼畫面更新的流程
      switch (msg.what) {
        case 1: //訊息編號為1
          Toast.makeText(MainActivity.this, "test", Toast.LENGTH_SHORT).show();
          Log.e(TAG,"handler test");
          break;
        case 2:
          // 每收到訊息一次，progress小於100 progress就+10
          if (progress < 100) {
            // 進度條變數+10
            progress+=10;
            // 將進度條變數放入progressBar中
            progressBar1.setProgress(progress);
            // 這個是重點！循環向handler傳送訊息，每隔2秒
            // progress滿了100就不再發訊息
            handler.sendEmptyMessageDelayed(2, 2*1000);
          }
          break;
      }
    }
  };
  // 發送訊息
  public void testTimer() {
    progressBar1.setVisibility(View.VISIBLE);
    // 2*1000毫秒 = 2 秒 2秒以後傳送handler訊息
    handler.sendEmptyMessageDelayed(2, 2*1000);
  }
  // 按了按鈕才開始
  public void btnClick(View view) {
    testTimer();
  }
{% endhighlight %}

## post(Runnable)
變更畫面的流程，寫進參數Runnable中的run()方法，post()方法會把Runnable物件丟給主要執行緒UI thread，由主要執行緒(UI Thread)去執行Runnable物件中的run()方法。

{% highlight java linenos %}
  public void postTest() {
    new Thread() {
      @Override
      public void run() {
        handler.post(new Runnable() {
          @Override
          public void run() {
            Toast.makeText(MainActivity.this, "post clicked!", Toast.LENGTH_LONG).show();
            Log.d("test:", "post here");
          }
        });
      }
    }.start();// 啟動背景執行緒
  }
  public void btnClick(View view) {
    postTest();
  }
{% endhighlight %}

## view.post(Runnable)
ui元件都繼承view，所以用ui元件.post(new Runnable())，畫面處理的流程寫在Runnable介面中的run()方法，通知UI thread，並根據run()方法中的流程，更新ui畫面。 
{% highlight java linenos %}
  private Button btn;
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    btn = (Button) findViewById(R.id.button);
  }
  public void viewpostTest() {
    new Thread() {
      @Override
      public void run() {
        // btn是view的子類別，使用post()的方法可以通知ui thread
        btn.post(new Runnable() {
          @Override
          public void run() {
            Toast.makeText(MainActivity.this, "view.post clicked!", Toast.LENGTH_LONG).show();
            Log.d("test:", "view.post here");
          }
        });
      }
    }.start();// 啟動背景執行緒
  }
  public void btnClick(View view) {
    viewpostTest();
  }
{% endhighlight %}

## runOnUiThread 更新畫面
除了handler外，runOnUiThread也可以進行畫面的更新。  
在執行緒中添加以下程式碼，並把畫面處理的流程寫在Runnable介面中的run()方法，通知ui thread，並根據run()方法中的流程，更新ui畫面。  
{% highlight java linenos %}
// 通知ui thread更新畫面
runOnUiThread(
  new Runnable() {
    @Override
    public void run() {
      // 畫面更新的流程
      Toast.makeText(MainActivity.this, "ui Test", Toast.LENGTH_SHORT).show();
      Log.e(TAG,"ui Thread Test");
    }
  });
{% endhighlight %}
完整程式碼
{% highlight java linenos %}
  // 建立背景執行緒
  new Thread(new Runnable() {
    @Override
    public void run() {
      // 通知ui thread更新畫面
      runOnUiThread(new Runnable() {
        @Override
        public void run() {
          // 畫面更新的流程
          Toast.makeText(MainActivity.this, "ui Test", Toast.LENGTH_SHORT).show();
          Log.e(TAG,"ui Thread Test");
        }
      });
    }
  }).start();// 啟動背景執行緒
{% endhighlight %}

[1]: {% link _pages/java/thread.md %}
[2]: {% link _pages/android/android_thread.md %}