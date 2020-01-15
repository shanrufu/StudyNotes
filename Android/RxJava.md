## RxJava

### 操作符：

#### 1、map()

原理图：

![map操作符原理](http://img.shanrufu.com/rxjava_map.jpg)

实现代码：

```kotlin
Observable.create(ObservableOnSubscribe<Int> {
    it.onNext(1)
}).map(object : Function<Int, String>{
    override fun apply(t: Int): String {
        return ""
    }
}).subscribe {
    Log.d(TAG, "onNext()")
}
```

#### 2、flatMap()

原理图：

![flatmap](http://img.shanrufu.com/rxjava_flmap.jpg)

实现代码：

```kotlin
Observable.create(ObservableOnSubscribe<Int> {
	it.onNext(1)
	it.onNext(2)
	it.onNext(3)
}).flatMap(object : Function<Int, ObservableSource<String>>() {
	override fun apply(): ObservableSource<String> {
		val list = ArrayList<String>()
		for (i in 0..3) {
			list.add(i)
		}
		return Observable.fromIterable(list).delay(10, TimeUtil.MILISECONDS)
	}
}).subscribe{
	Log.d(TAG, it)
}
```

#### 3、concatMap()

严格按照上游发送的事件顺序进行组合发送

#### 4、zip()
原理图：
![zip操作符原理图](http://img.shanrufu.com/rxjava_zip.jpg)
实现代码：
```kotlin
val observable1 = Observable.create(ObservableOnSubscribe<Int>{
    it.onNext(1)
    it.onNext(2)
    it.onNext(3)
})

val observable2 = Observable.create(ObservableOnSubscribe<String> {
    it.onNext("A")
    it.onNext("B")
    it.onNext("C")
})

Observable.zip(observable1, observable2, object : BiFunction<Int, String, String> {
    override fun apply(t1: Int, t2: String): String {
        return "$t1$t2"
    }
}).subscribeOn(Schedulers.io())
.observeOn(AndroidSchedulers.mainThread())
.subscribe {

}
```