---
title: 枚举
date: 2022-09-15 10:36:39
---
## 自定义类实现枚举

1.不需要提供set方法，因为枚举对象值通常为只读
2.构造器私有化
3.本类内部创建对象
4.对外暴露对象。对枚举对象/属性使用final + static 共同修饰，实现底层优化。
5.枚举对象名通常使用全部大写，常量的命名规范。
6.枚举对象根据需要，也可以有多个属性
```dotnetcli
package com.hspedu.enum_;

/**
 * @author kss
 * @version 1.0
 */
public class Enumeration1 {
    public static void main(String[] args) {
        System.out.println(season.SPRING);
        System.out.println(season.AUTUMN);
        System.out.println(season.SUMMER);
        System.out.println(season.WINTER);
    }
}
class season{
    private String name;
    private String desc;
    private season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }
    public String getDesc() {
        return desc;
    }
    public final static season SPRING = new season("spring","warm");
    public final static season SUMMER = new season("summer","hot");
    public final static season AUTUMN = new season("autumn","fun");
    public final static season WINTER = new season("winter","cool");

    @Override
    public String toString() {
        return "season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
--------------------------------------------------
D:\jdk8\jre\lib\resources.jar;D:\jdk8\jre\lib\rt.jar;D:\idea_java_project\chapter11\out\production\chapter11 com.hspedu.enum_.Enumeration1
season{name='spring', desc='warm'}
season{name='autumn', desc='fun'}
season{name='summer', desc='hot'}
season{name='winter', desc='cool'}
```
## 关键字实现枚举
1.使用关键字enum替代class
2.直接使用SPRING("spring","warm")创建对象，创建对象必须在enum类的最前面，如果有多个常量对象，使用“,”号间隔
```dotnetcli
package com.hspedu.enum_.Enumeration04;

/**
 * @author kss
 * @version 1.0
 */
public class Enumeration04 {
    public static void main(String[] args) {
        System.out.println(Season.SPRING);
        System.out.println(Season.SUMMER);
        System.out.println(Season.AUTUMN);
        System.out.println(Season.WINTER);
    }
}
enum Season{
    SPRING("spring","warm"),SUMMER("summer","hot"),
    AUTUMN("autumn","fun"),WINTER("winter","cool");
    private String name;
    private String desc;
    Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }
    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```
notice:
当我们使用enum关键字开发一个枚举类时，默认会继承Enum类，而且是一个final类
![ps1.png](https://s2.loli.net/2022/09/18/GhnCXvkqT2azjLb.png)
2.如果使用的是无参构造器，创建常量对象，则可以省略()
3.调用枚举对象 不用加() ;enum名.对象名
```dotnetcli
public class exercise01 {
    public static void main(String[] args) {
    gender boy = gender.BOY;
    gender boy1 = gender.BOY;
        System.out.println(boy1 == boy);//true
        System.out.println(boy);//BOY
    }
}
enum gender{
    BOY,GIRL;
}
```
4.enum实现的枚举类，仍然是一个类，所以还是可以实现接口的。
```dotnetcli
package com.hspedu.enum_.EnumExercise;

/**
 * @author kss
 * @version 1.0
 */
public class EnumDetail {
    public static void main(String[] args) {
        Music.classicMusic.playing();
    }
}
interface   IPlaying{
    public void playing();
}
enum Music implements IPlaying{
    classicMusic;

    Music() {
    }
    public void playing(){
        System.out.println("播放音乐。。。。");
    }
}
```
## enum常用方法
notice:使用关键字enum时，会隐式继承Enum类。
1. **toString**：Enum 类已经重写过了，返回的是当前对象名,子类可以重写该方法，用于返回对象的属性信息
2. **name**：返回当前对象名（常量名），子类中不能重写
3. **ordinal**：返回当前对象的位置号(第几个枚举对象)，默认从 0 开始
4. **values**：返回当前枚举类中所有的常量
5. **valueOf**：将字符串转换成枚举对象，要求字符串必须
为已有的常量名，否则报异常！
6. **compareTo**：比较两个枚举常量，比较的就是编号  

```dotnetcli
package com.hspedu.enum_.EnumerationMethods;

/**
 * @author kss
 * @version 1.0
 */
public class EnumMethod {
    public static void main(String[] args) {
        Season2 autumn = Season2.autumn;
        System.out.println(autumn.name());
        //ordinal()输出的是该枚举对象的次序/编号
        System.out.println(autumn.ordinal());
//        从反编译可以看出values方法，返回Season2[]
//        含有定义的所有枚举对象
        Season2[] values = Season2.values();
//        增强for循环
        System.out.println("==========遍历取出枚举对象========");
        for(Season2 season:values){
            System.out.println(season);
        }
        System.out.println();

//      valueof：将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常
//        执行流程
//        1.根据你输入的“autumn”字符串到Season2的枚举对象去查找
//        2.如果找到了，就返回，如果没有找到，就报错
//        3.此处的autumn1与Season2的autumn是同一个对象
        Season2 autumn1 = Season2.valueOf("autumn");
        System.out.println("autumn1=" + autumn1);
        System.out.println();
        System.out.println(autumn == autumn1);

//        compareTo:比较两个枚举常量，比较的就是编号
//        1.就是把Season2.autumn 枚举对象的编号 和Season2.summer枚举对象的编号进行相减
//        返回一个int值
        System.out.println(Season2.autumn.compareTo(Season2.summer));

    }
}

```
