---
title: '“委托--C#lambda表达式的使用'
date: 2016-09-04 15:52:44
tags: [unity,C#]
---
# 主题
在unity中使用lambda表达式实现不同脚本之间消息的传递
<!--more-->

实现功能：
当点击 D 键时触发当前脚本（DelegateScript2）的方法，并将消息（参数）传给另一个脚本（DelegateScript1），
并执行另一个脚（DelegateScript1）本的一个方法
DelegateScript2  ---当前执行的脚本
DelegateScript1  ---监听DelegateScript2方法执行的脚本

脚本如下：
DelegateScript2
```javascript
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class DelegateScript2 : MonoBehaviour {

   public  static DelegateScript2 instance;

  public delegate void MyDelegate(string str);

    void Awake() {
        instance = this;
    }

    //CallDelegateEvent方法在脚本调用时能正常运行 
    //同时在点击D的时候，同时将参数传给绑定的方法并执行绑定的方法
    public MyDelegate CallDelegateEvent = (string str) => {

        print(str + "--本脚本调用Lambda委托");
    };
    
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.D))
        {
            CallDelegateEvent(System.DateTime.Now.ToString("yy年MM月dd日HH时mm分ss秒"));
        }
    }
}
  ```

DelegateScript1
```javascript
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DelegateScript1 : MonoBehaviour
{
    void Start()
    {
        DelegateScript2.instance.CallDelegateEvent += aaaa;
    }

    void aaaa(string aa)
    {
        print("我这个脚本--" + aa + "监听到事件的发生并触发aaaa这个方法");
    }

    void OnDestroy()
    {
        DelegateScript2.instance.CallDelegateEvent -= aaaa;
    }
}
```
