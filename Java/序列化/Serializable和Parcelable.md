Serializable：
更适合用于将对象持久化到存储设备（如文件）或通过网络传输，因为 Java 的序列化机制具有较好的跨平台性。

Parcelable
主要用于在 Android 组件之间进行对象的传递，如在 Intent 中传递对象，不适合用于持久化存储和网络传输。


