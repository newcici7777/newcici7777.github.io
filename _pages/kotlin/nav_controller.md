---
title: NavController
date: 2025-10-17
keywords: kotlin, NavController
---
## implementation
{% highlight groovy linenos %}
def nav_version = "2.9.5"
// Views/Fragments Integration
implementation "androidx.navigation:navigation-fragment:$nav_version"
implementation "androidx.navigation:navigation-ui:$nav_version"
{% endhighlight %}

## Fragment
### FragA xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/name_tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### HomeFrag xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/nav_btn1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### FragA
{% highlight kotlin linenos %}
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.coroutine.databinding.FragmentMyBinding

class FragA: Fragment() {
  private lateinit var binding: FragmentMyBinding
  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    binding = FragmentMyBinding.inflate(layoutInflater)
    return binding.root
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    binding.nameTv.text = "Hello world"
  }
}
{% endhighlight %}

### HomeFrag
使用以下程式碼，點擊按鈕，導到FragA。<br>
```
findNavController().navigate(R.id.action_homeFrag_to_fragA)
```

注意！R專案下面的R，com.example.coroutine.R
{% highlight kotlin linenos %}
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.coroutine.R
import com.example.coroutine.databinding.FragmentHomeBinding

class HomeFrag : Fragment()  {
  private lateinit var binding: FragmentHomeBinding
  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    binding = FragmentHomeBinding.inflate(layoutInflater)
    return binding.root
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    binding.navBtn1.setOnClickListener {
      findNavController().navigate(R.id.action_homeFrag_to_fragA)
    }
  }
}
{% endhighlight %}

## navgation
設完Fragment，接下來設置navgation。<br>

![img]({{site.imgurl}}/kotlin/nav1.png) <br>

![img]({{site.imgurl}}/kotlin/nav2.png) <br>

![img]({{site.imgurl}}/kotlin/nav3.png) <br>

![img]({{site.imgurl}}/kotlin/nav4.png) <br>

![img]({{site.imgurl}}/kotlin/nav5.png) <br>

![img]({{site.imgurl}}/kotlin/nav6.png) <br>

![img]({{site.imgurl}}/kotlin/nav7.png) <br>

![img]({{site.imgurl}}/kotlin/nav8.png) <br>

## Activity
### xml

![img]({{site.imgurl}}/kotlin/nav_activity1.png) <br>

![img]({{site.imgurl}}/kotlin/nav_activity2.png) <br>

以下二個都要有設置到。<br>
```
app:defaultNavHost="true"
app:navGraph="@navigation/navigation1"
```

{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/fragmentContainerView4"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="729dp"
        app:defaultNavHost="true"
        app:navGraph="@navigation/navigation1"
        tools:layout_editor_absoluteX="1dp"
        tools:layout_editor_absoluteY="1dp" />
</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

{% highlight kotlin linenos %}
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.example.coroutine.databinding.ActivityMain4Binding

class MainActivity16 : AppCompatActivity() {
  private lateinit var binding : ActivityMain4Binding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMain4Binding.inflate(layoutInflater)
    setContentView(binding.root)
  }
}
{% endhighlight %}