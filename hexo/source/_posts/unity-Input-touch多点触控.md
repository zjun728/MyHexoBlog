---
title: unity Input.touch多点触控
date: 2020-05-01 20:16:15
tags: [unity,Touch]
---
# 前言

unity Input.touch多点触控

<!--more-->


原文：[https://blog.csdn.net/AnYuanLzh/article/details/18367941](https://blog.csdn.net/AnYuanLzh/article/details/18367941)

## Touch 管理

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

public class MyTouch : MonoBehaviour {

    /// <summary>  
    /// 定义的一个手指类  
    /// </summary>  
    class MyFinger
    {
        public int id = -1;
        public Touch touch;
        public Transform touchTrans;
         
        static private List<MyFinger> fingers = new List<MyFinger>();
        /// <summary>  
        /// 手指容器  
        /// </summary>  
        static public List<MyFinger> Fingers
        {
            get
            {
                if (fingers.Count == 0)
                {
                    for (int i = 0; i < 5; i++)
                    {
                        MyFinger mf = new MyFinger();
                        mf.id = -1;
                        mf.touchTrans = null;
                        fingers.Add(mf);
                    }
                }
                return fingers;
            }
        }
    }

    // 存储注册进来的Touch 物体
    public List<GameObject> myTouchs;
  
    void Update()
    {
        Touch[] touches = Input.touches;
        
        // 遍历所有的已经记录的手指  
        // --掦除已经不存在的手指  
        foreach (MyFinger mf in MyFinger.Fingers)
        {
            if (mf.id == -1)
            {
                continue;
            }
            bool stillExit = false;
            foreach (Touch t in touches)
            {
                if (mf.id == t.fingerId)
                {
                    stillExit = true;
                    break;
                }
            }
            // 掦除  
            if (stillExit == false)
            {
                mf.id = -1;
                mf.touchTrans = null;
            }
        }
        // 遍历当前的touches  
        // --并检查它们在是否已经记录在AllFinger中  
        // --是的话更新对应手指的状态，不是的加进去  
        foreach (Touch t in touches)
        {
            bool stillExit = false;
            // 存在--更新对应的手指  
            foreach (MyFinger mf in MyFinger.Fingers)
            {
                if (t.fingerId == mf.id)
                {
                    stillExit = true;
                    mf.touch = t;
                    break;
                }
            }
            // 不存在--添加新记录  
            if (!stillExit)
            {
                foreach (MyFinger mf in MyFinger.Fingers)
                {
                    if (mf.id == -1)
                    {
                        mf.id = t.fingerId;
                        mf.touch = t;
                        break;
                    }
                }
            }
        }

        // 记录完手指信息后，就是响应相应和状态记录了  
        for (int i = 0; i < MyFinger.Fingers.Count; i++)
        {
            MyFinger mf = MyFinger.Fingers[i];
            if (mf.id != -1)
            {
                if (mf.touchTrans==null)
                {
                    mf.touchTrans = CheckGuiRaycastObjects(mf.touch.position);
                }
               // Transform hitItem = CheckGuiRaycastObjects(mf.touch.position);
               
                if (mf.touchTrans != null)
                {
                   // Vector3 newpos = new Vector3(mf.touch.position.x - Screen.width / 2, mf.touch.position.y - Screen.height / 2, hitItem.localPosition.z);

                    if (mf.touch.phase == TouchPhase.Began)
                    {
                        // print("Start");
                        // marks[i].SetActive(true);
                        // aaa.position = GetWorldPos(mf.touch.position);

                        mf.touchTrans.GetComponent<TouchItem>().OnTouchBegin(mf.touch);
                        
                       // hitItem.localPosition = newpos;
                    }
                    else if (mf.touch.phase == TouchPhase.Moved)
                    {
                        // hitItem.localPosition = newpos;
                        mf.touchTrans.GetComponent<TouchItem>().OnTouchDrag(mf.touch);

                    }
                    else if (mf.touch.phase == TouchPhase.Ended)
                    {
                        //marks[i].SetActive(false);
                        // hitItem.localPosition = newpos;
                        mf.touchTrans.GetComponent<TouchItem>().OnTouchEnd();
                        mf.id = -1;
                        mf.touchTrans = null;
                    }
                }
            }
        }
    }


    EventSystem eventSystem;
    public GraphicRaycaster RaycastInCanvas;//Canvas上有这个组件
    Transform CheckGuiRaycastObjects(Vector3 point)
    {
        PointerEventData eventData = new PointerEventData(eventSystem);
        eventData.pressPosition = point;
        eventData.position = point;
        List<RaycastResult> list = new List<RaycastResult>();
        RaycastInCanvas.Raycast(eventData, list);
        Transform thistrnas = null;
        if (list.Count > 0)
        {

            bool isEnable = true;
            for (int i = 0; i < list.Count; i++)
            {
                if (list[i].gameObject.name=="Image")
                {
                    isEnable = false;
                    break;
                }
            }
            if (isEnable)
            {
                for (int i = 0; i < list.Count; i++)
                {
                    if (list[i].gameObject.tag == "Player")
                    {
                            thistrnas = list[i].gameObject.transform;
                       
                        break;
                    }
                }
            }
        }
        return thistrnas;
    }

    public Vector3 GetWorldPos(Vector2 screenPos)
    {
        return Camera.main.ScreenToWorldPoint(new Vector3(screenPos.x, screenPos.y, Camera.main.nearClipPlane + 10));
    }
}
```
## 单个需要拖拽的物体组件
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TouchItem : MonoBehaviour {
    
   public bool isBegin = false;
    Vector3 newpos;

    int fingerIndex = -1;

    MyTouch myTouch;

    Vector2 firstMousePos = Vector3.zero;

    //鼠标拖拽的位置

    Vector2 secondMousePos = Vector3.zero;
    Vector3 offsetPos = Vector3.zero;

    private void Awake()
    {
        myTouch = GameObject.FindObjectOfType<MyTouch>();
    }

    private void Start()
    {
        myTouch.myTouchs.Add(this.gameObject);
    }

  public void OnTouchBegin(Touch  mytouch) {
        try
        {
        //    fingerIndex = index;
            isBegin = true;
       
            newpos = new Vector3(mytouch.position.x, mytouch.position.y, transform.localPosition.z);
       
            firstMousePos = newpos;
           }
          catch 
           {
              isBegin =false;
          }
    }
    
   public void OnTouchDrag(Touch mytouch) {
        if (isBegin)
        {
              try
              {
            print("move");
                newpos = new Vector3(mytouch.position.x, mytouch.position.y, transform.localPosition.z);

                secondMousePos = newpos;
                offsetPos = secondMousePos - firstMousePos;

                float x = transform.localPosition.x;

                float y = transform.localPosition.y;
                x = x + offsetPos.x; y = y + offsetPos.y;

                transform.localPosition = new Vector3(x, y, transform.localPosition.z);
                firstMousePos = secondMousePos;
            }
            catch 
            {
                isBegin = false;
            }
        }
    }

   public void OnTouchEnd() {
        isBegin = false;
        fingerIndex = -1;
    }
}
```
## 使用方法：
将需要touch的image挂载TouchItem脚本
并将image的tag设置为 Player。 将MyTouch脚本挂载在canvas上 

