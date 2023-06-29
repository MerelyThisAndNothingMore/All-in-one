---
tags: 
alias:
---
https://blog.csdn.net/u011686167/article/details/124132719

https://www.jianshu.com/p/7604d27080d6

https://juejin.cn/post/6844904192780271630#heading-1
## 获取类的实例对象
```cpp
void get_class(JNIEnv* env, jobject object) {
	// get class from object
	jclass clazz = env->GetObjectClass(object);

	// get class from path
	jclass claxx = env->FindCLass("java/lang/NullPonterException");
}
```

## 反射调用Java变量
```cpp
jclass clazz = env->GetObjectClass(object);
jfieldID fieldID = env->GetFieldID(clazz, "level", "I");
env->SetIntField(object, fieldId, 8);
```



