---
title: unity Post请求实现登录
date: 2018-05-31 16:44:46
tags: [unity,post]
---
# 前言

unity 使用post通信方式实现登录注册功能

<!--more-->

```javascript

using System.Collections;

using System.Collections.Generic;

using UnityEngine;

using LitJson;  //需要将LitJson加到Plugins里面

public class JsonTestScript : MonoBehaviour {

//LitJson下载     https://pan.baidu.com/s/1eeJn8H8EO7wRxqcK32J-Ug

    //验证服务器实现登录 url 为后端提供

    string url = "http://gbweb/webapi/login";

    void Start () {

        WWWForm form = new WWWForm();

        form.AddField("tel","12345678900");  //需要提交的数据也由后端提供

        form.AddField("password","123");

        StartCoroutine(SendPost(url,form));

}


    IEnumerator SendPost(string _url, WWWForm _wForm)
    {
        WWW postData = new WWW(_url, _wForm);

        yield return postData;

        if (postData.error != null)
        {
            Debug.Log(postData.error);
        }
        else
        {
            string str = postData.text.ToString();

            JsonData data = JsonMapper.ToObject(str);

            print(data["msg"]);

            print(data["status"]); //由后端提供json数据
        }
    }
}

```

------------------------

后端数据如下

{% asset_img 后端数据.png 后端数据 %}

json数据结构

{% asset_img json结构.png json结构 %}

