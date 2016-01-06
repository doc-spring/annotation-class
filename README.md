# 自定义的注解类
本节将讲述如何自定义一个简单的注解类。
首先给出注解类的定义
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface NeedTest {
	boolean value() default true; //注解成员
}
```
一个注解类以`@interface`关键词来表示，如果只有一个成员，那么该成员必须命名为`value()`。`@Retention`注解表示注解的保留期限，譬如对于`RetentionPolicy.RUNTIME`将保留到运行时期。`@Target`注解表示自定义的注解的应用目标，譬如对于`ElementType.METHOD`，自定义注解可以注解到类方法上。  
>**Tips:** 可以使用`default`关键词来指定注解成员的默认值。

接下来给出使用自定义注解的代码片段
```java
public class ForumService {
	@NeedTest(true)
	public void removeTopic(int topicId) {
		System.out.println("removeTopic:" + topicId);
	}
	
	@NeedTest(false)
	public void removeForum(int forumId) {
		System.out.println("removeForum:" + forumId);
	}
}
```

下面给出一个测试类
```java
public class Main {
	public static void main(String[] args) {
		Method[] methods = ForumService.class.getDeclaredMethods();
		for (Method method : methods) {
			NeedTest needTest = method.getAnnotation(NeedTest.class);
			if (needTest != null) {
				if (needTest.value()) {
					System.out.println(method.getName() + " need test");
				} else {
					System.out.println(method.getName() + " doesnt need test");
				}
			}
		}
	}
}
```
运行结果如下：
```
removeTopic need test
removeForum doesnt need test
```
运行结果显示我们自定义的标注是有效的。
