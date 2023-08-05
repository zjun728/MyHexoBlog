---
title: 协程
date: 2017-12-08 16:30:13
tags: [unity,协程]
---

# 前言

协程的使用介绍

<!--more-->

```javascript

using UnityEngine;

using System.Collections;

public class CoroutineTestScript : MonoBehaviour
{
///

/// 协程: 一个返回值是IEnumerator 的接口

/// 协程功能: 将一个方法拆成多步执行

/// yield 意思为 放弃 退位  在这里的意思是暂停一下 等待一个条件满足后再从这里继续开始

/// 协程的开启和停止:

/// 开启协程有两种形式  StartCoroutine (PrintMethod ());

/// 或StartCoroutine ("PrintMethod");

/// 协程和线程不是一回事

/// 线程是操作系统级别的 协程是操作编辑器级别的

///停止协程 只能停止字符串开启的协程

///StopCoroutine ();

///

///

void Start ()
{
// 开启协程

StartCoroutine (PrintMethod ());

//或者

//StartCoroutine ("PrintMethod");

// 停止协程 只能停止字符串开启的协程

//StopCoroutine ();
}

void NormalMethod ()
{
print ("普通方法");
}

//协程

IEnumerator PrintMethod ()
{
print ("第一步");

//yield return null;  等待下一帧 直到继续执行为止

yield return new WaitForSeconds (5); // 等待5秒 5秒以后再从这里继续

print ("第二步");

yield return null;

print ("第三步");

yield return new WaitForSeconds (3);

StartCoroutine (DoMethod ());
}

IEnumerator DoMethod ()
{
print ("第二个协程");

yield return 0;

print ("第二个结束");
}

//void Update(){

////调用频率很快 无法等待 可以使用协程解决这个问题
//}
}

```