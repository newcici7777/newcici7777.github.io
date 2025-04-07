---
title: preferencesDataStore Flow找不到
date: 2023-07-04
keywords: Android, Jetpack compose, preferencesDataStore Flow找不到
---
Grandle
```
implementation "androidx.datastore:datastore-preferences-android:1.1.0-alpha04"
```
![img]({{site.imgurl}}/compose/preferences_datastore1.png)
Flow找不到  
要用到以下
```  
import kotlinx.coroutines.flow.Flow  
import kotlinx.coroutines.flow.map  
```
![img]({{site.imgurl}}/compose/preferences_datastore2.png)
Data Store
要用到以下套件
```
import android.content.Context
import androidx.datastore.core.DataStore
import androidx.datastore.preferences.core.Preferences
import androidx.datastore.preferences.core.booleanPreferencesKey
import androidx.datastore.preferences.core.edit
import androidx.datastore.preferences.core.stringPreferencesKey
import androidx.datastore.preferences.preferencesDataStore
```
![img]({{site.imgurl}}/compose/preferences_datastore3.png)

