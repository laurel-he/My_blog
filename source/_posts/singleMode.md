---
title: 单例模式
date: 2019-10-06 10:40:10
tags: 学习
categories: 设计模式
---
# 基础介绍
## 概念介绍
作为对象的创建模式，单例模式确保某一个类只有一个实例，并且对外提供这个全局实例的访问入口。它不会创建实例副本，而是会向单例类内部存储的实例返回一个引用。
## 单例模式三要素
1. 需要一个保存类的唯一实例的静态成员变量。    
2. 构造函数和克隆函数必须声明为私有的，防止外部程序创建或复制实例副本。    
3. 必须提供一个访问这个实例的公共静态方法，从而返回唯一实例的一个引用。    
