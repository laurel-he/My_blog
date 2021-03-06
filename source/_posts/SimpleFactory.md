---
title: 简单工厂模式
date: 2019-07-24 10:40:10
tags: 概念介绍
categories: 设计模式
---
## 引入
### 实现计算器
#### 代码实现
1 实现一个基础的计算器功能，代码见https://github.com/laurel-he/design_pattern/blob/master/simpleFactory/calculator01.php
#### 问题分析
（1）错误处理只判断了除数是否为0，对于字符超长，不可计算等都未处理，可以加上try catch；    
（2）代码不可复用，耦合性很高
#### 使用面向对象处理
（1）使用面向对象的方式实现，将输入输出流和逻辑代码分离，可以提高代码复用性，降低耦合，代码见https://github.com/laurel-he/design_pattern/blob/master/simpleFactory/Calculate2.php
#### 紧耦合vs松耦合
思考：什么情况下使用继承和多态（各种运算可以继承自运算基类，便于扩展，多态考虑输入的不同类型，对于字符串怎样运算）    
根据以上思考，完成有继承和多态的代码如下：
https://github.com/laurel-he/design_pattern/blob/master/simpleFactory/Calculate03.php      
思考：以上代码实现方式虽然使用到了继承，但是如何知道应该调用哪个类呢？难道像之前预估的一样，还是要使用switch判断？
#### 简单工厂模式
解决问题，实例化谁，将来会不会增加实例化的对象等容易变化的地方，考虑用一个单独的类来做这个创造实例的过程
在此基础上实现一个简单工厂类，代码如下：    
https://github.com/laurel-he/design_pattern/blob/master/simpleFactory/Calculate04.php
如果需要修改运算，可以只修改对应的类，如果需要添加运算，只需要添加运算类，并在工厂中添加对应的分支就可以了    
简单工厂模式的工厂类一般是使用静态方法，通过接受的参数的不同来返回不同的对象实例
#### 工厂方法模式
1 简单工厂模式优点：    
（1）简单工厂包含必要的判断逻辑，实现了对象的创建和使用的分离；    
（2）客户端无需知道所创建的具体产品类的类名，只需要具体产品类对应的参数即可；    
（3）在不修改任何客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性    
2 简单工厂模式缺点：    
（1）工厂类职责过重，它出问题整个系统都会崩溃     
（2）添加新的类的时候，系统中的简单工厂类都要修改，违反了开放-封闭原则    
（3）简单工厂的静态方法，使得工厂角色无法形成基于继承的等级结构     
工厂方法模式每一种算法都对应一种工厂，
工厂方法模式优点：    
（1）
#### 抽象工厂
