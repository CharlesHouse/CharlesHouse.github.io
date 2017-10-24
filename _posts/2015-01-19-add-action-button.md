---
layout: post
title:  为ActionBar 添加Action Buttons
date:   2015-01-19 18:34
category: "Android"
---

前几天做App 的时候，做的比较仓促，就没有添加返回按钮，还有就是MainActivity 的Theme 怎么传递到其他Activity 中，今天无意中看到官方的Train，所以就记下来啦，顺便吧ActionBar，Menu 也复习复习；

## 添加Action Buttons

首先我们要创建``menu`` 文件，我们可以为顶部添加Menu，``showAsAction`` 是显示的类型，ifRoom 会自动匹配是否有实体键，always 是一直隐藏，但是使用Support Library``android:showAsAction="ifRoom"`` 会提示错误，所以在menu 中添加``xmlns:app="http://schemas.android.com/apk/res-auto"`` 属性，改为``app:showAsAction="ifRoom"`` 就没有错误；

``res/menu/main_activity_actions.xml``
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android" >
    <!-- Search, should appear as action button -->
    <item android:id="@+id/action_search"
          android:icon="@drawable/ic_action_search"
          android:title="@string/action_search"
          android:showAsAction="ifRoom" />
    <!-- Settings, should always be in the overflow -->
    <item android:id="@+id/action_settings"
          android:title="@string/action_settings"
          android:showAsAction="never" />
</menu>
```


## 添加ActionBar

```java
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
```

## 为Action Button 添加响应事件

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();
    //当点击不同的menu item 是执行不同的操作
    switch (id) {
        case R.id.action_search:
            openSearch();
            break;
        case R.id.action_settings:
            openSettings();
            break;
        default:
            break;
    }
    return super.onOptionsItemSelected(item);
}
```

## 为ActionBar 顶部添加返回按钮
首先要在AndroidManifest 文件中配置

```xml
<application ... >
    ...
    <!-- The main/home activity (it has no parent activity) -->
    <activity
        android:name="com.example.myfirstapp.MainActivity" ...>
        ...
    </activity>
    <!-- A child of the main activity -->
    <activity
        android:name="com.example.myfirstapp.DisplayMessageActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.myfirstapp.MainActivity" >
        <!-- Parent activity meta-data to support 4.0 and lower -->
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.myfirstapp.MainActivity" />
    </activity>
</application>
```

然后在onCrate 中设置
```java
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_displaymessage);
    // 3.0+ 
    getSupportActionBar().setDisplayHomeAsUpEnabled(true);
    // If your minSdkVersion is 11 or higher, instead use:
    // getActionBar().setDisplayHomeAsUpEnabled(true);
}
```

## 最后的效果图

![http://tikitoo.qiniudn.com/android_training_back_activity.png](http://tikitoo.qiniudn.com/android_training_back_activity.png)

## 为下一个Activity 传递MainActivity Theme
需要注意的是，``setTheme`` 方法在``setContentView`` 之前；其实就是利用Intent 将Theme 传递到下一个Activity；

```java
public class MainActivity extends ActionBarActivity {

    public static int MAIN_THEME_STYLE_ID = R.style.Theme_AppCompat_Light;
    public static String MAIN_THEME_STYLE = "Theme_AppCompat_Light";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setTheme(R.style.Theme_AppCompat_Light);
        setContentView(R.layout.activity_main);
    }

    public void onToBackActivity(View view) {
        Intent intent = new Intent(this, BackActivity.class);
        intent.putExtra(MAIN_THEME_STYLE, MAIN_THEME_STYLE_ID);
        startActivity(intent);
    }
}

```


## 在下一个Activity 获取Theme
```java
public class BackActivity extends ActionBarActivity {
    int style_id;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getMainIntent();
        setTheme(style_id);
        setContentView(R.layout.activity_back);

        getSupportActionBar().setDisplayShowCustomEnabled(true);

    }
    void getMainIntent() {
        Intent intent = getIntent();
        style_id = intent.getIntExtra(MainActivity.MAIN_THEME_STYLE, 0);

    }
}

```
