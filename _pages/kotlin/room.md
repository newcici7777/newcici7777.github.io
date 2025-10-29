---
title: Room
date: 2025-10-21
keywords: kotlin, Room
---
## build.gradle(app) 設定
### plugin
{% highlight kotlin linenos %}
plugins {
    id 'kotlin-kapt'  // 確保有這行
}
{% endhighlight %}

### implementation
記得一定要加上kapt
{% highlight groovy linenos %}
    def room_version = "2.8.2"
    implementation "androidx.room:room-runtime:$room_version"
    implementation "androidx.room:room-ktx:$room_version"
    kapt "androidx.room:room-compiler:$room_version"   // ← 必須加這行！
{% endhighlight %}

## Room建立步驟
### MyDatabase 建立資料庫
{% highlight kotlin linenos %}
package com.example.coroutine.db
import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase

@Database(entities = [User::class], version = 1, exportSchema = false)
abstract class MyDatabase: RoomDatabase() {
  abstract fun userDao(): UserDao
  companion object {
    private var instance: MyDatabase? = null
    fun getInstance(context: Context): MyDatabase {
      return instance ?: synchronized(this) {
        val newInstance = Room.databaseBuilder(context, MyDatabase::class.java,"user.db").build()
        instance = newInstance
        newInstance
      }
    }
  }
}
{% endhighlight %}

### User 表格
有二個欄位，分別是uid與name。
{% highlight kotlin linenos %}
import androidx.room.ColumnInfo
import androidx.room.Entity
import androidx.room.PrimaryKey
@Entity(tableName = "user")
data class User (
  @PrimaryKey val uid: Int,
  @ColumnInfo(name = "name") val name: String)
{% endhighlight %}

### UserDao 資料庫操作
{% highlight kotlin linenos %}
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.OnConflictStrategy
import androidx.room.Query
import kotlinx.coroutines.flow.Flow

@Dao
interface UserDao {
  @Insert(onConflict = OnConflictStrategy.REPLACE)
  suspend fun insert(user: User)

  @Query("SELECT * FROM user")
  fun getAll(): Flow<List<User>>
}
{% endhighlight %}

### Activity 測試
{% highlight kotlin linenos %}
package com.example.coroutine.activity

import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import com.example.coroutine.R
import com.example.coroutine.db.MyDatabase
import com.example.coroutine.db.User
import kotlinx.coroutines.launch

class MainActivity18 : AppCompatActivity() {
  private val db by lazy { MyDatabase.getInstance(this) }
  private val userDao by lazy { db.userDao() }
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    submit.setOnClickListener {
      lifecycleScope.launch {
        userDao.insert(User(3, "Bob"))
      }
    }
    lifecycleScope.launch {
      userDao.getAll().collect { user ->
        textv.text = "$user"
      }
    }
  }
}
{% endhighlight %}

## 結合viewmodel flow adapter
### viewmodel
{% highlight kotlin linenos %}
import android.app.Application
import androidx.lifecycle.AndroidViewModel
import androidx.lifecycle.viewModelScope
import com.example.coroutine.db.MyDatabase
import com.example.coroutine.db.User
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.flow.catch
import kotlinx.coroutines.flow.flowOn
import kotlinx.coroutines.launch

class UserViewModel(app: Application) : AndroidViewModel(app) {
  fun insert(uid: String, name: String) {
    viewModelScope.launch {
      MyDatabase.getInstance(getApplication())
        .userDao().insert(User(uid.toInt(), name))
    }
  }
  fun getAll(): Flow<List<User>> {
    return MyDatabase.getInstance(getApplication())
      .userDao()
      .getAll()
      .catch { e -> e.printStackTrace() }
      .flowOn(Dispatchers.IO)
  }
}
{% endhighlight %}

### adapter
#### adapter 與 veiwholder
{% highlight kotlin linenos %}
import android.content.Context
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView
import androidx.viewbinding.ViewBinding
import com.example.coroutine.adapter.UserAdapter.UserItemViewHold
import com.example.coroutine.databinding.UserItemBinding
import com.example.coroutine.db.User

class UserAdapter(private val context: Context) : RecyclerView.Adapter<UserItemViewHold>() {
  inner class UserItemViewHold(val binding: ViewBinding) :
    RecyclerView.ViewHolder(binding.root) {}

  private val data = ArrayList<User>()
  fun setData(data: List<User>) {
    this.data.clear()
    this.data.addAll(data)
    notifyDataSetChanged()
  }

  override fun onCreateViewHolder(
    parent: ViewGroup,
    viewType: Int
  ): UserItemViewHold {
    val binding = UserItemBinding.inflate(LayoutInflater.from(context), parent, false)
    return UserItemViewHold(binding)
  }

  override fun onBindViewHolder(
    holder: UserItemViewHold,
    position: Int
  ) {
    val item = data[position]
    val binding = holder.binding as UserItemBinding
    binding.nameTv.text = "${item.uid} , ${item.name}"
  }

  override fun getItemCount(): Int = data.size
}
{% endhighlight %}

#### xml
user_item.xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

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

### Activity
#### Activity程式碼
{% highlight kotlin linenos %}
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import com.example.coroutine.adapter.UserAdapter
import com.example.coroutine.databinding.ActivityDbBinding
import com.example.coroutine.model.UserViewModel
import kotlinx.coroutines.launch
import androidx.activity.viewModels
import androidx.recyclerview.widget.LinearLayoutManager

class MainActivity17 : AppCompatActivity() {
  private val viewModel by viewModels<UserViewModel>()
  private lateinit var binding : ActivityDbBinding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityDbBinding.inflate(layoutInflater)

    setContentView(binding.root)
    val addBtn = binding.addBtn
    val uidEdit = binding.uid
    val nameEdit = binding.name
    addBtn.setOnClickListener {
      viewModel.insert(uidEdit.text.toString(), nameEdit.text.toString())
    }
    val adapter = UserAdapter(this@MainActivity17)
    binding.dbRecycleview.layoutManager = LinearLayoutManager(this)
    binding.dbRecycleview.adapter = adapter
    lifecycleScope.launch {
      viewModel.getAll().collect { value ->
        adapter.setData(value)
      }
    }
  }
}
{% endhighlight %}

#### xml
activity_db.xml
{% highlight css linenos %}
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="40dp"
        android:layout_marginTop="128dp"
        android:text="uid"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/uid"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="48dp"
        android:layout_marginTop="104dp"
        android:ems="10"
        android:inputType="text"
        android:text=""
        app:layout_constraintStart_toEndOf="@+id/textView3"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="68dp"
        android:text="name"
        app:layout_constraintTop_toBottomOf="@+id/textView3"
        tools:layout_editor_absoluteX="40dp" />

    <EditText
        android:id="@+id/name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="52dp"
        android:ems="10"
        android:inputType="text"
        android:text=""
        app:layout_constraintTop_toBottomOf="@+id/uid"
        tools:layout_editor_absoluteX="107dp" />

    <Button
        android:id="@+id/addBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="Button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/name" />

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/db_recycleview"
        android:layout_width="409dp"
        android:layout_height="406dp"
        android:layout_marginStart="1dp"
        android:layout_marginEnd="1dp"
        android:layout_marginBottom="2dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/addBtn" />

</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}