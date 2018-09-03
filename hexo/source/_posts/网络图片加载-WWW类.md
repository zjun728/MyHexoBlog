---
title: 网络图片加载--WWW类
date: 2017-12-05 16:25:22
tags: [unity,WWW]
---
# 前言

使用WWW类加载网络图片

<!--more-->

using UnityEngine;

using System.Collections;

using UnityEngine.UI;

using UnityEditor;

using System.IO;

public class DownloadimageScript : MonoBehaviour

{

// 显示下载好的图片

public Image m_image;

//const 防止下面修改

private const string m_image_URL = "http://www.33lc.com/article/UploadPic/2012-9/201291314425876155.jpg";

private string m_imagePath;

void Start ()

{

// 如果这张图片存在 直接去本地拿 如果不存在就去下载 下载完后保存到本地 这样下次就能直接用

m_imagePath = Application.dataPath + "/Resources/NetPicture.jpg";

// 开启协程去本地拿图片

StartCoroutine (LoadPictureFromLocal ());

}

//优先加载本地资源 读取较快 没有的话在去网上下载

IEnumerator LoadPictureFromLocal ()

{

if (File.Exists (m_imagePath)) {

Debug.Log ("图片已经存在");

// 直接加载本地图片资源

Texture2D texture = (Texture2D)Resources.Load ("NetPicture", typeof(Texture2D));

// 创建sprite

Sprite m_sprte = Sprite.Create (texture, new Rect (0, 0, texture.width, texture.height), new Vector2 (0.5f, 0.5f));

m_image.sprite = m_sprte;

} else {

Debug.Log ("图片文件开始下载");

StartCoroutine (PictureDownloding (m_image_URL));

}

yield return new WaitForSeconds (2f);

AssetDatabase.Refresh ();// 需要引用unityEditor  来解决Project 不显示下载好的图片

//使用协程来解决本地没有图片时下载完后game不显示图片

if (m_image.sprite != null) {

StopCoroutine (LoadPictureFromLocal ());

} else {

StartCoroutine (LoadPictureFromLocal ());

}

}

//根据URL下载图片并加载

IEnumerator PictureDownloding (string url)

{

// 方式1

WWW www = new WWW (url);

while (www.isDone == false) {

print ("下载图片中" + www.progress);

yield return null;

}

if (www.texture != null && string.IsNullOrEmpty (www.error)) {

print ("正在写入本地图片");

byte[] pngData = www.texture.EncodeToPNG ();

File.WriteAllBytes (m_imagePath, pngData);

}

}

}