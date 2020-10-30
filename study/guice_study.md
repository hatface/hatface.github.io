## guice_study

## 概述

Google guice是google开发的轻量级依赖注入框架

|框架名称|文档是否丰富|使用是否广泛|是否有开源软件使用|
|----|----|----|----|
|Google guice|否|否|有|

因为有开源软件，例如shiro在使用guice，所以研究一下Google guice

## Google guice注入的几种方式
### 1、 单个注入

```java
public static void main(String[] args) {
    Injector injector = Guice.createInjector(new BindModule());
    DogEgg dogEgg = injector.getInstance(DogEgg.class);
    System.out.println(dogEgg.service);
}

interface Service {
}

public static class DefaultService implements Service {
}

public static class DogEgg {
    @Inject // 告诉Guice，这里要注入东西，具体的注入规则从Module里找吧
    public Service service;
}

public static class BindModule implements Module {
    @Override
    public void configure(Binder binder) {
        // 在注入的时候，遇到Service接口类型，全部注入成DefaultService实例
        binder.bind(Service.class).to(DefaultService.class);
    }
}
```

使用BindModule来配置注入的映射，这里映射接口A到接口A的实现

### 2、 多个注入

如果一个接口有多种实现怎办？guice提供了一种使用注解的方式。

```java
@BindingAnnotation
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.TYPE})
public @interface DefaultService {
}

public static void main(String[] args) {
    Injector injector = Guice.createInjector(new BindModule());
    DogEgg dogEgg = injector.getInstance(DogEgg.class);
    System.out.println(dogEgg.service);
    System.out.println(dogEgg.service2);
}

interface Service {}

public static class DefaultService implements Service {}

public static class DefaultService2 implements Service {}

public static class DogEgg {
    @Inject
    @DefaultAnnotation // 使用自定义注解标识该字段需要DefaultService实例
    public Service service;

    @Inject // 没有自定义注解，那就注入DefaultService2实例
    public Service service2;
}

public static class BindModule implements Module {
    @Override
    public void configure(Binder binder) {
        binder.bind(Service.class).annotatedWith(DefaultAnnotation.class)
                .to(DefaultService.class).in(Scopes.SINGLETON);

        binder.bind(Service.class).to(DefaultService2.class);
    }
}
```

在BindModule中配置映射模式，如果有注解配置到DefaultService.class，如果没有注解配置到DefaultService2.class

### 3、 多个接口使用名字命名

每次定义一个注解费时费力，有没有其他的方法？

```java
public static void main(String[] args) {
    Injector injector = Guice.createInjector(new BindModule());
    DogEgg dogEgg = injector.getInstance(DogEgg.class);
    System.out.println(dogEgg.service);
    System.out.println(dogEgg.service2);
    System.out.println(dogEgg.service3);
}

interface Service {}

public static class DefaultService implements Service {}

public static class DefaultService2 implements Service {}

public static class DogEgg {
    @Inject
    @Named("service1") // 注意这里的@Named注解
    public Service service;

    @Inject
    @Named("service2") // 注意这里的@Named注解
    public Service service2;

    @Inject
    public Service service3; // 注意这里的没有@Named注解！！！
}

public static class BindModule implements Module {
    @Override
    public void configure(Binder binder) {
        // 被@Named("service1")修饰的，给注入DefaultService实例
        binder.bind(Service.class).annotatedWith(Names.named("service1"))
                .to(DefaultService.class);
        // 被@Named("service2")修饰的，给注入DefaultService2实例
        binder.bind(Service.class).annotatedWith(Names.named("service2"))
                .to(DefaultService2.class);
        // 默认给注入DefaultService实例
        binder.bind(Service.class).to(DefaultService.class);
    }
}

```

使用Named标签，并配置相关映射即可，方便很多。

### 4、 多个接口使用名称并配置单例模式

如果我想配置类只有一个实例怎么办？这样显然更符合一些场景下的模型定义，使用in(Scopes.SINGLETON)

```java
public static class DogEgg {
    @Inject
    @Named("service1")
    public Service service;

    @Inject
    @Named("service1")
    public Service service5;

    @Inject
    @Named("service2")
    public Service service2;

    @Inject
    public Service service3;

    @Inject
    public Service service4;
}

public static class BindModule implements Module {
    @Override
    public void configure(Binder binder) {
        binder.bind(Service.class).annotatedWith(Names.named("service1"))
                .to(DefaultService.class).in(Scopes.SINGLETON); // 这里指定了Scope
        binder.bind(Service.class).annotatedWith(Names.named("service2"))
                .to(DefaultService2.class);
        binder.bind(Service.class).to(DefaultService.class).in(Scopes.SINGLETON); // 这里也指定了Scope
    }
}
```

输出如下

```java
DefaultService@ed9d034
DefaultService@ed9d034 // service1和service5是一个对象
DefaultService2@6121c9d6
DefaultService@87f383f
DefaultService@87f383f // service3和service4是一个对象，但是与service1和service2不相同
```

可见虽然是绑定到同一个DefaultService.class，但是条件不同annotatedWith和没有anotatedWith，实例化不是同一个实例。

上面这波输出也告诉我们，Guice的单例是按照binding划分的。

如果要全局单例，可以使用@Singleton注解，代码如下：

```java
@Singleton // 这个注解告诉Guice，该实现全局单例
public static class DefaultService implements Service {
}

public static class DefaultService2 implements Service {
}

public static class DogEgg {
    @Inject
    @Named("service1")
    public Service service;

    @Inject
    @Named("service1")
    public Service service5;

    @Inject
    @Named("service2")
    public Service service2;

    @Inject
    public Service service3;

    @Inject
    public Service service4;
}

public static class BindModule implements Module {
    @Override
    public void configure(Binder binder) {
        binder.bind(Service.class).annotatedWith(Names.named("service1"))
                .to(DefaultService.class); // 这里没有再指定Scope
        binder.bind(Service.class).annotatedWith(Names.named("service2"))
                .to(DefaultService2.class);
        binder.bind(Service.class).to(DefaultService.class);  // 这里没有再指定Scope
    }
}
```

输出如下：

```java
InjectMoreSingleton$DefaultService@5bb21b69
InjectMoreSingleton$DefaultService2@6b9651f3 //定义的是DefaultService2.class，故和其他不同
InjectMoreSingleton$DefaultService@5bb21b69
InjectMoreSingleton$DefaultService@5bb21b69
InjectMoreSingleton$DefaultService@5bb21b69
```

定义为全局一致的输出的类是一样。

### 5、 构造函数注入

```java
public static void main(String[] args) {
    Injector injector = Guice.createInjector(new BindModule());
    DogEgg dogEgg = injector.getInstance(DogEgg.class);
    System.out.println(dogEgg.service);
    System.out.println(dogEgg.service5);
    System.out.println(dogEgg.service2);
    System.out.println(dogEgg.service3);
    System.out.println(dogEgg.service4);
}

interface Service {}

@Singleton
public static class DefaultService implements Service {

    private String name;
    private TestArg testArg;

    public DefaultService(String name, TestArg testArg) { // 构造函数
        this.name = name;
        this.testArg = testArg;
    }

    public DefaultService() {} // 无参构造函数

    @Override
    public String toString() { // 这里专门加了一个hashCode()，用于区分是不是一个对象
        return hashCode() +" DefaultService{" +
                "name='" + name + '\'' +
                ", testArg=" + testArg +
                '}';
    }
}

public static class DefaultService2 implements Service {}

public static class DogEgg {
    @Inject
    @Named("service1")
    public Service service;

    @Inject
    @Named("service1")
    public Service service5;

    @Inject
    @Named("service2")
    public Service service2;

    @Inject
    public Service service3;

    @Inject
    public Service service4;
}

public static class TestArg {
}

public static class BindModule implements Module {
    @Override
    public void configure(Binder binder) {
        // 碰到@Named("service1")注解修饰的注入，继续走无参构造函数
        binder.bind(Service.class).annotatedWith(Names.named("service1")).to(DefaultService.class);
        binder.bind(Service.class).annotatedWith(Names.named("service2"))
                .to(DefaultService2.class);
        // binder.bind(Service.class).to(DefaultService.class); 这与下面走构造函数的binding冲突，Guice不知道该走哪个构造方法
        try {
            binder.bind(Service.class) // 没有@Named("service1")注解修饰的注入，走构造方法
                    .toConstructor(DefaultService.class
                            .getConstructor(String.class, TestArg.class));
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
    }
}
```