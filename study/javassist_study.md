## Javassist 学习
- 基础javassite的Demo
```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
 
import javassist.CannotCompileException;
import javassist.ClassPool;
import javassist.CtClass;
import javassist.CtConstructor;
import javassist.CtField;
import javassist.CtNewMethod;
import javassist.Modifier;
import javassist.NotFoundException;
import javassist.CtField.Initializer;
 
public class JavassistGenerator {
	
	public static void main(String[] args) throws CannotCompileException, NotFoundException, InstantiationException, IllegalAccessException, ClassNotFoundException, SecurityException, NoSuchMethodException, IllegalArgumentException, InvocationTargetException {
		// 创建类
		ClassPool pool = ClassPool.getDefault();
		CtClass cls = pool.makeClass("cn.ibm.com.TestClass");
		
		// 添加私有成员name及其getter、setter方法
		CtField param = new CtField(pool.get("java.lang.String"), "name", cls);
		param.setModifiers(Modifier.PRIVATE);
		cls.addMethod(CtNewMethod.setter("setName", param));
		cls.addMethod(CtNewMethod.getter("getName", param));
		cls.addField(param, Initializer.constant(""));
		
		// 添加无参的构造体
		CtConstructor cons = new CtConstructor(new CtClass[] {}, cls);
		cons.setBody("{name = \"Brant\";}");
		cls.addConstructor(cons);
		
		// 添加有参的构造体
		cons = new CtConstructor(new CtClass[] {pool.get("java.lang.String")}, cls);
		cons.setBody("{$0.name = $1;}");
		cls.addConstructor(cons);
		
		// 打印创建类的类名
		System.out.println(cls.toClass());
		
		// 通过反射创建无参的实例，并调用getName方法
		Object o = Class.forName("cn.ibm.com.TestClass").newInstance();
		Method getter = o.getClass().getMethod("getName");
		System.out.println(getter.invoke(o));
		
		// 调用其setName方法
		Method setter = o.getClass().getMethod("setName", new Class[] {String.class});
		setter.invoke(o, "Adam");
		System.out.println(getter.invoke(o));
		
		// 通过反射创建有参的实例，并调用getName方法
		o = Class.forName("cn.ibm.com.TestClass").getConstructor(String.class).newInstance("Liu Jian");
		getter = o.getClass().getMethod("getName");
		System.out.println(getter.invoke(o));
	}
 
}
```

- 插入source 文本在方法体前或者后

CtMethod 和CtConstructor 提供了 insertBefore()、insertAfter()和 addCatch()方法，它们可以插入一个souce文本到存在的方法的相应的位置。javassist 包含了一个简单的编译器解析这souce文本成二进制插入到相应的方法体里。
Javassist提供了一些特殊的变量来代表方法参数：$1,2 , 2,2,args…

## $0, $1, $2, …
$0代码的是this，$1代表方法参数的第一个参数、$2代表方法参数的第二个参数,以此类推，$N代表是方法参数的第N个。

## $args
$args 指的是方法所有参数的数组，类似Object[]，如果参数中含有基本类型，则会转成其包装类型。需要注意的时候，$args[0]对应的是$1,而不是$0，$0!=$args[0]，$0=this。

## $_
$_代表的是方法的返回值。

## $sig
$sig指的是方法参数的类型（Class）数组，数组的顺序为参数的顺序。

## $class
$class 指的是this的类型（Class）。也就是$0的类型。

## 其他
```bash
https://blog.csdn.net/zero__007/article/details/102408445
```

