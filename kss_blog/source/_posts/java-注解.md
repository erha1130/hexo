---
title: 注解
date: 2022-09-28 20:59:23
tags: java
categories: java
---
## 注解的理解
1. 注解(Annotation)也被称为元数据(Metadata),用于修饰解释 包、类、方法、属性、构造器、局部变量等数据信息
2. 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入在代码中的补充信息
3. 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。

## 基本的Annotation介绍
使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation 当成一个修饰符使用。用于修饰它支持的程序元
素  
三个基本的Annotation：  
1. @Override: 限定某个方法，是重写父类方法, 该注解只能用于方法
2. @Deprecated: 用于表示某个程序元素(类, 方法等)已过时
3. @SuppressWarnings: 抑制编译器警告
   
### @Deprecated的注解
1. @Deprecated 修饰某个元素, 表示该元素已经过时
2. 即不在推荐使用，但是仍然可以使用
3. 查看 @Deprecated 注解类的源码
4. 可以修饰方法，类，字段, 包, 参数 等等
5. @Deprecated 可以做版本升级过渡使用
   ```dotnetcli
package com.hspedu.annotation;

/**
 * @author kss
 * @version 1.0
 */
public class Deprecated_ {
    public static void main(String[] args) {
        A a = new A();
        a.hi();
    }
}
//1. @Deprecated 修饰某个元素，表示该元素已经过时
//2. 即不在推荐使用，但是仍然可以使用
@Deprecated
class A {
    public int n1 = 10;
    @Deprecated
    public void hi(){
        System.out.println("hello world");
    }
}
   ```

### @SuppressWarning的注解
```dotnetcli
package com.hspedu.annotation;

import java.util.ArrayList;
import java.util.List;

/**
 * @author kss
 * @version 1.0
 */
public class SuppressWarnings_ {
//    1. 当我们不希望看到这些警告的时候，可以使用 SuppressWarnings注解来抑制警告信息
//    2. 在{" "}中，可以写入你希望抑制(不显示)警告信息
//    3. 可以指定警告类型
//    3. 关于SuppressWarnings 作用范围是和你放置的位置相关
    @SuppressWarnings("all")
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("jack");
        list.add("tom");
        list.add("mary");
        int i;
        System.out.println(list.get(1));
    }
}

    }
}

```
    all，抑制所有警告
    boxing，抑制与封装/拆装作业相关的警告
    cast，抑制与强制转型作业相关的警告
    dep-ann，抑制与淘汰注释相关的警告
    deprecation，抑制与淘汰的相关警告
    fallthrough，抑制与 switch 陈述式中遗漏 break 相关的警告
    finally，抑制与未传回 finally 区块相关的警告
    hiding，抑制与隐藏变数的区域变数相关的警告
    incomplete-switch，抑制与 switch 陈述式(enum case)中遗漏项目相关的警告
    javadoc，抑制与 javadoc 相关的警告
    nls，抑制与非 nls 字串文字相关的警告
    null，抑制与空值分析相关的警告
    rawtypes，抑制与使用 raw 类型相关的警告
    resource，抑制与使用 Closeable 类型的资源相关的警告
    restriction，抑制与使用不建议或禁止参照相关的警告
    serial，抑制与可序列化的类别遗漏 serialVersionUID 栏位相关的警告
    static-access，抑制与静态存取不正确相关的警告
    static-method，抑制与可能宣告为 static 的方法相关的警告
    super，抑制与置换方法相关但不含 super 呼叫的警告
    synthetic-access，抑制与内部类别的存取未最佳化相关的警告
    sync-override，抑制因为置换同步方法而遗漏同步化的警告
    unchecked，抑制与未检查的作业相关的警告
    unqualified-field-access，抑制与栏位存取不合格相关的警告
    unused，抑制与未用的程式码及停用的程式码相关的警告
## jdk的元Annotation
**jdk的元注解用于修饰其他注解**
本身作用不大，方便看源码
### 元注解的种类
1. Retention //指定注解的作用范围，三种 SOURCE,CLASS,RUNTIME
2. Target // 指定注解可以在哪些地方使用
3. Documented //指定该注解是否会在 javadoc 体现
4. Inherited //子类会继承父类注解
#### @Retention注解
**只能用于修饰一个Annotation定义，用于指定该Annotation可以保留多长时间，@Rentention包含一个RetentionPolicy类型的成员变量，使用@Rentention时必须为该value成员变量指定值：@Retention的三种值 ** 
1. RetentionPolicy.SOURCE: 编译器使用后，直接丢弃这种策略的注释
2. RetentionPolicy.CLASS: 编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 不会保留注解。 这是默认
值
3. RetentionPolicy.RUNTIME:编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 会保留注解. 程序可以
通过反射获取该注解
```dotnetcli
@Documented
@Retention(RetentionPolicy.RUNTIME)//这个就是@Retention的取值
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    /**
     * Returns the retention policy.
     * @return the retention policy
     */
    RetentionPolicy value();
}
```
#### @Target注解
**用于修饰Annotation定义用于指定被修饰的Annotation能用于修饰哪些程序元素类型。@Target也包含一个名为value的成员变量。**
```dotnetcli
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)//这里的ANNOTATION_TYPE说明@Target只能修饰注解，ElementType是一个枚举类，枚举对象包含被修饰的注解的注解类型。
public @interface Retention {
    /**
     * Returns the retention policy.
     * @return the retention policy
     */
    RetentionPolicy value();
}
```
#### @Inherited注解
**被它修饰的Annotation将具有继承性。如果某个类使用了被@Inherited修饰的Annotation，则其子类将自动具有该注解**
