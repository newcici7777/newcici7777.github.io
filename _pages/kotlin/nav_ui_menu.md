---
title: Nav UI Menu
date: 2025-11-17
keywords: kotlin, Nav UI Menu
---
## xml設定
以下的id = ...，是指按下「設定」，就會移動到fragA這個Fragment。
```
android:id="@+id/fragA"
```

`res/menu/nav_menu.xml`
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/fragA"
        android:icon="@drawable/ic_launcher_background"
        android:title="設定"
        />
</menu>
{% endhighlight %}

## 加上toolbar
Activity的Layout
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <com.google.android.material.appbar.MaterialToolbar
        android:id="@+id/toolbar"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        android:theme="@style/ThemeOverlay.AppCompat.ActionBar"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:fitsSystemWindows="true"
        app:title="My App" />

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/fragmentContainerView4"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="411dp"
        android:layout_height="659dp"
        app:defaultNavHost="true"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/toolbar"
        app:layout_constraintVertical_bias="0.0"
        app:navGraph="@navigation/navigation1" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

MainActivity程式碼
{% highlight kotlin linenos %}

import android.os.Bundle
import android.view.Menu
import android.view.MenuItem
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.NavController
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.AppBarConfiguration
import androidx.navigation.ui.NavigationUI
import com.example.coroutine.R
import com.example.coroutine.databinding.ActivityMain4Binding

class MainActivity16 : AppCompatActivity() {
  private lateinit var binding : ActivityMain4Binding
  private lateinit var navController: NavController
  private lateinit var appBarConfig: AppBarConfiguration
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMain4Binding.inflate(layoutInflater)
    setContentView(binding.root)
    setSupportActionBar(binding.toolbar)  // 注意這行
    // 安全取得 NavController
    val navHostFragment = supportFragmentManager
      .findFragmentById(R.id.fragmentContainerView4) as NavHostFragment
    navController = navHostFragment.navController
    // 設定 ActionBar 與 NavController
    appBarConfig = AppBarConfiguration(navController.graph)
    // 設定上一頁
    NavigationUI.setupActionBarWithNavController(this, navController, appBarConfig)
  }

  // menu layout
  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    menuInflater.inflate(R.menu.nav_menu, menu)
    return true
  }

  // 點擊menu
  override fun onOptionsItemSelected(item: MenuItem): Boolean {
    return NavigationUI.onNavDestinationSelected(item,navController) || super.onOptionsItemSelected(item)
  }

  // 上一頁
  override fun onSupportNavigateUp(): Boolean {
    return NavigationUI.navigateUp(navController,appBarConfig) || super.onSupportNavigateUp()
  }
}
{% endhighlight %}