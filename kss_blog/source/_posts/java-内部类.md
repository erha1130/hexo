---
title: 内部类
date: 2022-09-11 23:27:00
tags: java
categories: java
---

## 局部内部类
1.局部内部类是定义在外部类的局部位置，通常在方法，代码块中
2.可以直接访问外部类的所有成员，包含私有的
3.不能添加访问修饰符，但是可以使用final修饰
4.作用域：仅仅在定义它的方法或代码块中(相当于局部变量)
5.外部类在方法中，可以创建内部类对象，然后调用方法即可访问
6.外部其他类不能访问局部内部类（因为局部内部类地位是一个局部变量）
7.如果外部类和局部内部类的成员重名，默认遵循就近原则，如果想访问外部类的成员，则可以使用 外部类名.this.成员 去访问
HOMEWORK
1. 编一个类A，在类中定义局部内部类B，B中有一个私有final常量name，有一个方法show()打印常量name。进行测试
2. 进阶：A中也定义一个私有的变量name，在show方法中打印测试

## 匿名内部类
1.基本语法：new 类或接口(参数列表）{
类体
};
```example
package com.hspedu.innerclass;

public class AnonymousInnerClass {
    public static void main(String[] args) {
        Outer04 outer04 = new Outer04();
        outer04.method();
    }
}
class Outer04 { //外部类
    private int n1 = 10;//属性
    public void method() {//方法
        //基于接口的匿名内部类
        //3.老韩需求是 Tiger/Dog 类只是使用一次，后面再不使用
        //4. 可以使用匿名内部类来简化开发
        //5. tiger的编译类型 ? IA
        //6. tiger的运行类型 ? 就是匿名内部类  Outer04$1
        /*
            我们看底层 会分配 类名 Outer04$1
            class Outer04$1 implements IA {
                @Override
                public void cry() {
                    System.out.println("老虎叫唤...");
                }
            }
         */
        //7. jdk底层在创建匿名内部类 Outer04$1,立即马上就创建了 Outer04$1实例，并且把地址
        //   返回给 tiger
        //8. 匿名内部类使用一次，就不能再使用
        IA tiger = new IA() {
            @Override
            public void cry() {
                System.out.println("老虎叫唤...");
            }
        };
        System.out.println("tiger的运行类型=" + tiger.getClass());
        tiger.cry();
        tiger.cry();
        tiger.cry();
        //演示基于类的匿名内部类
        //分析
        //1. father编译类型 Father
        //2. father运行类型 Outer04$2
        //3. 底层会创建匿名内部类
        /*
            class Outer04$2 extends Father{
                @Override
                public void test() {
                    System.out.println("匿名内部类重写了test方法");
                }
            }
         */
        //4. 同时也直接返回了 匿名内部类 Outer04$2的对象
        //5. 注意("jack") 参数列表会传递给 构造器
        Father father = new Father("jack"){

            @Override
            public void test() {
                System.out.println("匿名内部类重写了test方法");
            }
        };
        System.out.println("father对象的运行类型=" + father.getClass());//Outer04$2
        father.test();

        //基于抽象类的匿名内部类 必须重写抽象类的所有方法
        Animal animal = new Animal(){
            @Override
            void eat() {
                System.out.println("小狗吃骨头...");
            }
        };
        animal.eat();
    }
}
interface IA {//接口
    public void cry();
}
class Father {//类
    public Father(String name) {//构造器
        System.out.println("接收到name=" + name);
    }
    public void test() {//方法
    }
}
abstract class Animal { //抽象类
    abstract void eat();
}

```
2.匿名内部类既是一个类的定义，同时它本身也是一个对象。
3.可以直接访问外部类的所有成员，包含私有的
4.不能添加访问修饰符
5.作用域：定义它的方法或代码块中
6.匿名内部类—-访问——>外部类成员（直接访问）
7.外部其他类—不能访问———>匿名内部类
8.如果外部类和局部内部类的成员重名，默认遵循就近原则，如果想访问外部类的成员，则可以使用 外部类名.this.成员 去访问
9.动态绑定
10.当做实参直接传递，简洁高效
```example
package com.hpedu.inner_;

public class AnnyInnerClassExercise {
    public static void main(String[] args) {
        cellphone cellphone = new cellphone();
        cellphone.alarm(new Bell() {
            @Override
            public void ring() {
                System.out.println("pig get up");
            }
        });
        cellphone.alarm(new Bell() {
            @Override
            public void ring() {
                System.out.println("dog get up");
            }
        });
    }
}
interface Bell {
    public void ring();
}
class cellphone {
    public void alarm(Bell bell){
        System.out.println("bell's hashcode is "+bell);
        bell.ring();
    }
}
```
HOMEWORK
1. 计算器接口具有work方法，功能是运算，有一个手机类Cellphone，定义方法testWork测试计算功能，调用计算接口的work方法
2. 要求调用Cellphone对象的testWork方法，使用上匿名内部类
```homework
package com.hspedu.homework;

/**
 * @author kss
 * @version 1.0
 */
public class Homework04 {
    public static void main(String[] args) {
        Cellphone cellphone = new Cellphone();
        cellphone.testWork(new ICalculate() {
            @Override
            public double work(double n1, double n2) {
                return n1 + n2;
            }
        },10,10);
    }
}
interface ICalculate {
//    work方法 是完成计算，但是题没有具体要求，所以自己设计
//    至于该方法完成怎样的计算，交给匿名内部类完成
    public double work(double n1,double n2);
}
class Cellphone {
//    当我们调用testWork方法时，直接传入一个实现了ICalculate接口的匿名内部类即可
//    该匿名内部类，可以灵活的实现work，完成不同的计算任务
   public void testWork(ICalculate iCalculate,double n1,double n2){
      double result = iCalculate.work(n1,n2);
       System.out.println("计算后的结果是=" + result);
   }
}

```
## 成员内部类
notices:定义在外部类的成员位置，没有static修饰
1.可以添加任意修饰符
2.成员内部类—访问—->外部类成员（直接访问）
3.外部类—访问——–>成员内部类（创建对象再访问）
4.外部其他类—–访问—–>成员内部类（第一种方法：Outer08.Inner08 inner08 = outer08.new Inner08();相当于把new Inner08（）当做是outer08的成员 第二种方法：在外部类中编写一个返回值为成员内部类对象的方法 public Inner08 getInner08Instance(){ return new Inner08; } 在main方法中调用 Outer08.Inner08 inner08Instance = outer08.getInner08Instance(); ）
5.如果外部类和局部内部类的成员重名，默认遵循就近原则，如果想访问外部类的成员，则可以使用 外部类名.this.成员 去访问
## 静态内部类
notices：定义在外部类的成员位置，并且有static修饰
1.可以添加任意访问修饰符，因为它的地位就是一个成员
2.作用域：和其他的成员，为整个类体
3.静态内部类—-访问—->外部类（直接访问所有静态成员）
4.外部类—–访问—–>静态内部类（创建对象，再访问）
5.外部其他类——访问——->静态内部类
```example
//方式 1
//因为静态内部类，是可以通过类名直接访问(前提是满足访问权限)
Outer10.Inner10 inner10 = new Outer10.Inner10();
inner10.say();
//方式 2
//编写一个方法，可以返回静态内部类的对象实例. Outer10.Inner10 inner101 = outer10.getInner10();
System.out.println("============");
inner101.say();
```
6.如果外部类和静态内部类的成员重名时，静态内部类访问时，默认遵循就近原则，如果想访问外部类的成员，则可以使用（外部类名.成员)去访问