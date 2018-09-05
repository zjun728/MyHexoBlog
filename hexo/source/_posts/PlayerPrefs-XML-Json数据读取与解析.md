---
layout: w
title: PlayerPrefs_&_XML_&_Json数据读取与解析
date: 2017-09-05 10:28:44
tags: [unity,json,xml,PlayerPrefs]
---
# 主题
unity 常用的数据读取与存储的方法（PlayerPrefs、XML、Json）解读
<!--more-->

# PlayerPrefs 
PlayerPrefs是unity中一个用于本地持久化保存与读取的类。它以键值对的形式将数据保存在文件中，
然后程序可以根据这个名称取出上次保存的数值。
PlayerPrefs类支持3中数据类型的保存和读取，浮点型，整形，和字符串型。
 分别对应的函数为：
SetInt();保存整型数据；
GetInt();读取整形数据；
SetFloat();保存浮点型数据；
GetFlost();读取浮点型数据；
SetString();保存字符串型数据；
GetString();读取字符串型数据；

用法如下：
```javascript

//PlayerPrefs 的存储功能

  PlayerPrefs.SetInt ("yong", 110);
  
  PlayerPrefs.SetString (m_UserName, m_PassWord);
  //读取数据功能
  
  print (PlayerPrefs.GetInt ("yong"));
  
  // 输出为110
  
  //提供的其他方法

PlayerPrefs.DeleteKey (key) //删除指定数据

PlayerPrefs.DeleteAll() //删除全部键 

PlayerPrefs.HasKey (key)//判断数据是否存在的方法

if (PlayerPrefs.HasKey("isShow"))
{
  //TODO
}
  
```
# XML
## 1、引入命名空间
```javascript

using System.Xml;

```
## 2、创建XML文档
```javascript

// 创建XML文档
  void CreatXML ()
  {
    // 创建一个声明
    XmlDeclaration dec = doc.CreateXmlDeclaration ("1.0", "UTF-8", null);
    // 将声明拼接到文档中去
    doc.AppendChild (dec);

    //创建一个根元素节点
    XmlNode rootNode = doc.CreateNode (XmlNodeType.Element, "Transform", null);
    // 对这个根元素节点添加属性
    XmlAttribute attr = doc.CreateAttribute ("name");
    attr.Value = "YourCube";   //给属性赋值
    //将刚刚创建的属性放进根元素节点标签中去
    rootNode.Attributes.SetNamedItem (attr);

    // 拼接到xml文档里去
    doc.AppendChild (rootNode);
    // 效果展示  <Transform name="MyCube" tag="CubeTag">

    // 创建一个子元素 添加到根元素节点中
    XmlElement pos = doc.CreateElement ("Position");

    XmlElement pos_X = doc.CreateElement ("x");
    XmlElement pos_Y = doc.CreateElement ("y");
    XmlElement pos_Z = doc.CreateElement ("z");

    pos_X.InnerText = "78";
    pos_Y.InnerText = "30";
    pos_Z.InnerText = "56";

    pos.AppendChild (pos_X);
    pos.AppendChild (pos_Y);
    pos.AppendChild (pos_Z);

    // 拼接到根节点中去
    rootNode.AppendChild (pos);

    // 记得保存文档
    //Application.dataPath 路径就是Assets文件夹
    // 记得在文件前写/ 表示在这个文件夹下
    doc.Save (Application.dataPath + "/XML/MonoCodeXML.xml");
  } 

```
## 3、解析XML文档
```javascript

// 解析XML文档
  void AnalyzeXML ()
  {
    // 首先拿到文档 [用xml文档实例去加载这个文档]
    doc.Load (Application.dataPath + "/XML/MonoCodeXML.xml");

    //根据文档的数据结构, 来分析如何获取想要的数据

    // 拿到根元素节点
    XmlElement root = doc.DocumentElement;
    //print (root.Name);
    // root.FirstChild 就是这一层级下面的一个子元素节点
    XmlNode pos_X = root.FirstChild.FirstChild;

    // X子节点中存储的数据内容
    string pos_X_String = pos_X.InnerText;
    //print (pos_X_String);

    //或者通过层级路径拿到这个节点的内容
    XmlNode pos_x = root.SelectSingleNode ("/Transform/Position/x");
    print (pos_x.InnerText);
  } 

```
## 4、完整脚本
```javascript


using UnityEngine;
using System.Collections;
using System.Xml;

public class CreatXMLScript : MonoBehaviour
{

  // 创建一个文档
  XmlDocument doc = new XmlDocument ();

  void Start ()
  {
    CreatXML ();
    AnalyzeXML ();
  }

  // 创建XML文档
  void CreatXML ()
  {
    // 创建一个声明
    XmlDeclaration dec = doc.CreateXmlDeclaration ("1.0", "UTF-8", null);
    // 将声明拼接到文档中去
    doc.AppendChild (dec);


    //创建一个根元素节点
    XmlNode rootNode = doc.CreateNode (XmlNodeType.Element, "Transform", null);
    // 对这个根元素节点添加属性
    XmlAttribute attr = doc.CreateAttribute ("name");
    attr.Value = "YourCube";   //给属性赋值
    //将刚刚创建的属性放进根元素节点标签中去
    rootNode.Attributes.SetNamedItem (attr);

    // 拼接到xml文档里去
    doc.AppendChild (rootNode);
    // 效果展示  <Transform name="MyCube" tag="CubeTag">


    // 创建一个子元素 添加到根元素节点中
    XmlElement pos = doc.CreateElement ("Position");

    XmlElement pos_X = doc.CreateElement ("x");
    XmlElement pos_Y = doc.CreateElement ("y");
    XmlElement pos_Z = doc.CreateElement ("z");

    pos_X.InnerText = "78";
    pos_Y.InnerText = "30";
    pos_Z.InnerText = "56";

    pos.AppendChild (pos_X);
    pos.AppendChild (pos_Y);
    pos.AppendChild (pos_Z);

    // 拼接到根节点中去
    rootNode.AppendChild (pos);


    // 记得保存文档
    //Application.dataPath 路径就是Assets文件夹
    // 记得在文件前写/ 表示在这个文件夹下
    doc.Save (Application.dataPath + "/XML/MonoCodeXML.xml");

  }


  // 解析XML文档
  void AnalyzeXML ()
  {
    // 首先拿到文档 [用xml文档实例去加载这个文档]
    doc.Load (Application.dataPath + "/XML/MonoCodeXML.xml");

    //根据文档的数据结构, 来分析如何获取想要的数据

    // 拿到根元素节点
    XmlElement root = doc.DocumentElement;
    //print (root.Name);
    // root.FirstChild 就是这一层级下面的一个子元素节点
    XmlNode pos_X = root.FirstChild.FirstChild;

    // X子节点中存储的数据内容
    string pos_X_String = pos_X.InnerText;
    //print (pos_X_String);

    //或者通过层级路径拿到这个节点的内容
    XmlNode pos_x = root.SelectSingleNode ("/Transform/Position/x");
    print (pos_x.InnerText);
  }
}
 
```
# Json 数据解析
## 1、JsonUtility untiy 自带的解析工具 无需引用命名空间

```javascript

using UnityEngine;

using System.Collections.Generic;

//此脚本使用的是System.Json 来操作JSON数据的
//将System.Json.dll 拉到unity  Plugins 文件夹下
// System.Json.dll 文件位置   unity安装位置下
//  Editor\Data\Mono\lib\mono\unity\System.Json.dll
using System.Json;

//数据流的写入和读取
using System.IO;

//让编辑器刷新资源
using UnityEditor;

using System.Text;

using System;

public class CreatJsonScript : MonoBehaviour
{
  void Start()
    {
        // 使用代码创建JSON 保存数据
        //CreatJSONData ();
        // 解析JSON
        ParseJSONData();
    }

    // 创建json
    void CreatJSONData()
    {
        // 写一个大括号
        JsonObject Students = new JsonObject();
       
        // Q W E R
        JsonObject message_1 = new JsonObject();
        JsonObject message_2 = new JsonObject();
        JsonObject message_3 = new JsonObject();
        JsonObject message_4 = new JsonObject();

        // 填充数据
        message_1.Add("Subject", "语文");
        message_1.Add("Stage", "一考");
        message_1.Add("Score", "93");
        message_1.Add("Ranking", "7");

        message_2.Add("Subject", "数学");
        message_2.Add("Stage", "一考");
        message_2.Add("Score", "95");
        message_2.Add("Ranking", "5");

        message_3.Add("Subject", "英语");
        message_3.Add("Stage", "一考");
        message_3.Add("Score", "96");
        message_3.Add("Ranking", "4");

        message_4.Add("Subject", "物理");
        message_4.Add("Stage", "一考");
        message_4.Add("Score", "99");
        message_4.Add("Ranking", "2");

        // 信息数组
        JsonArray MessageArr = new JsonArray();
        MessageArr.Add(message_1);
        MessageArr.Add(message_2);
        MessageArr.Add(message_3);
        MessageArr.Add(message_4);

        Students.Add("Messages", MessageArr);
        Students.Add("StudentName", "小明");
        Students.Add("StudentGrade", 6);
        Students.Add("StudentID", 258);

        print (Students.ToString ());

        // 拓展-- 数据留存档
        // 创建数据流的写入
        StreamWriter writer = new StreamWriter(Application.dataPath + "/Json/MySystemJSON.txt");
        Students.Save(writer);
        //自动装载缓冲区
        writer.AutoFlush = true;

        writer.Close();

        //让编辑器刷新资源
        AssetDatabase.Refresh();
    }

    void ParseJSONData()
    {
        // 本地文件的读取
        FileInfo file = new FileInfo(Application.dataPath + "/Json/MySystemJSON.txt");
        StreamReader reader = new StreamReader(file.OpenRead(), Encoding.UTF8);
        string str = reader.ReadToEnd();

       // print (str);
        reader.Close();
        // 释放资源
        reader.Dispose();
        // 开始解析
        StudentInfo m_StudentInfo = JsonUtility.FromJson<StudentInfo>(str);
        
        for (int i = 0; i < m_StudentInfo.Messages.Count; i++)
        {
            print(m_StudentInfo.Messages[i].Score); //93 95 96 99
        }
    }
    //创建数据模型 接受解析数据 [因为我们是面向对象开发的]
    // 将我们在需要数据的地方直接使用类就可以点出这个模型

    //序列化  你发个qq 消息别人接收你的qq消息 不会分别将不同的数据类型发送或接收
    // 序列化后 相当于将消息编码加密 解密
    //[SerializeField] 修饰私有字段的 [强制将脚本中的私有变量在unity面板中出现 可以实现引用拖拽 但是其他脚本还是拿不到这个变量]


    //[Serializable]  修饰对象 作用 将对象序列化为二进制数据流 byte 类型  传输数据或解析数据

    [Serializable]
    public class Message
    {
        public string Subject = "";
        public string Stage = "";
        public string Score = "";
        public string Ranking = "";
    }

    [Serializable]
    public class StudentInfo
    {
        public List<Message> Messages = new List<Message>();
        public string StudentName = "";
        public float StudentGrade;
        public float StudentID;
    }
}
 
```

## system.json创建的json数据结构如下：
```javascript

{
    "Messages": [{
		"Subject": "语文",
		"Stage": "一考",
		"Score": "93",
		"Ranking": "7"
	}, {
		"Subject": "数学",
		"Stage": "一考",
		"Score": "95",
		"Ranking": "5"
	}, {
		"Subject": "英语",
		"Stage": "一考",
		"Score": "96",
		"Ranking": "4"
	}, {
		"Subject": "物理",
		"Stage": "一考",
		"Score": "99",
		"Ranking": "2"
	}],
	"StudentName": "小明",
	"StudentGrade": 6,
	"StudentID": 258
}

```



## 2、JsonMapper  需引用命名空间 using LitJson; //需要将LitJson加到Plugins里面
### LitJson下载     https://pan.baidu.com/s/1eeJn8H8EO7wRxqcK32J-Ug

```javascript

using UnityEngine;
using System.Collections;
using LitJson;
using System.IO;
using UnityEditor;
using System.Text;
using System;

public class LitJSONScript : MonoBehaviour
{
      void Start()
    {
        //使用LitJSON创建JSON数据
      //  CreatJSONByLit ();

        // 使用LitJSON解析数据
        ParseJSONLit();
    }

    void CreatJSONByLit()
    {
        //创建一个JSON结构的实例
        JsonData Students = new JsonData();
        Students["StudentName"] = "xioaming";
        Students["StudentGrade"] =7;
        Students["StudentID"] = 122;

        Students["Messages"] = new JsonData();

        // 小技能
        JsonData message_1 = new JsonData();

        message_1["Subject"] = "Chinese";
        message_1["Stage"] = "First";
        message_1["Score"] = "115";
        message_1["Ranking"] = "6";

        JsonData message_2 = new JsonData();

        message_2["Subject"] = "Maths";
        message_2["Stage"] = "First";
        message_2["Score"] = "119";
        message_2["Ranking"] = "2";

        Students["Messages"].Add(message_1);
        Students["Messages"].Add(message_2);

        print(Students.ToJson());

        // 保存到本地 并刷新
        SaveJson(Students.ToJson());
    }

    private void SaveJson(string json)
    {
        string path = Application.dataPath + "/Json/MyLitJsonByMono.txt";
        // 文件流创建一个文本文件
        FileStream file = new FileStream(path, FileMode.Create);
        //得到字符串的UTF8 数据流
        byte[] bts = System.Text.Encoding.UTF8.GetBytes(json);
        // 文件写入数据流
        file.Write(bts, 0, bts.Length);
        if (file != null)
        {
            //清空缓存
            file.Flush();
            // 关闭流
            file.Close();
            //销毁资源
            file.Dispose();
        }
    }

    void ParseJSONLit()
    {
        //
        StreamReader reader = new StreamReader(Application.dataPath + "/Json/MyLitJsonByMono.txt");
        string str = reader.ReadToEnd();
        //print (str);
        reader.Close();
        reader.Dispose();
        //开始解析数据
        JsonData data = JsonMapper.ToObject(str);
        // 拿到JSON的对象
        print(data["StudentName"]);    //xioaming

        for (int i = 0; i < data["Messages"].Count; i++)
        {
            print(data["Messages"][i]["Subject"]);     //Chinese     Maths
        }
    }
}

```

## litjson创建的json如下：
```javascript

{
    "StudentName": "xioaming",
	"StudentGrade": 7,
	"StudentID": 122,
	"Messages": [{
		"Subject": "Chinese",
		"Stage": "First",
		"Score": "115",
		"Ranking": "6"
	}, {
		"Subject": "Maths",
		"Stage": "First",
		"Score": "119",
		"Ranking": "2"
	}]
}

```

























