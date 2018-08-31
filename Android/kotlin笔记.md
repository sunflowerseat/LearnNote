- 扩展已有类方法

```kotlin
View.getApp() : PanamaApplication = context.app
```



- 扩展已有类属性

```kotlin
var Context.app : 
get(){
	return applicationContext as PanamaApplication
}

set(value){
	app = value
}

```




