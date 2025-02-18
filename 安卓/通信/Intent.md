Intent用于在组件之间通信

# 启动组件
Activity
```java
Intent intent = new Intent(this, RegisterActivity.class);
startActivity(intent);
```
Service
```java
Intent intent = new Intent(this, MusicService.class);
startService(intent);
```
[[Broadcast Receiver]]
```java
Intent intent = new Intent("com.example.MY_CUSTOM_BROADCAST");
sendBroadcast(intent);
```

# 传递数据
数据可以是基本数据类型，也可以是实现了 Parcelable 或 Serializable 接口的对象
基本数据类型
```java
Intent intent = new Intent(this, SecondActivity.class);
intent.putExtra("string_data", "Hello World");
intent.putExtra("int_data", 123);
startActivity(intent);

String stringData = getIntent().getStringExtra("string_data");
int intData = getIntent().getIntExtra("int_data", 0);
```


