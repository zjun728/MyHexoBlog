---
title: unity对本地文件的相关操作
date: 2018-04-20 16:39:47
tags: [unity]
---
# 前言

unity 对本地文件的一些操作，包括:创建文件夹，加载文件夹内图片，删除文件夹文件，等

<!--more-->

# 一、在桌面创建文件夹 C:\Users\Administrator\Desktop(桌面路径)

        string path = "c:/Users/Administrator/Desktop/TestFolder"; //TestFolder要创建得文件夹的名字

        if (!Directory.Exists(path))

        {

            Directory.CreateDirectory(path);

        }

------------------------------

# 二、(1)读取文件夹中的图片（WWW 类加载） 图片名字为 testPicture.jpg

Texture2D texture;

 IEnumerator loadTexture() {

        //这里的文件路径 ，需要加上file///

        string str = "file:///c:/Users/Administrator/Desktop/TestFolder/testPicture.jpg";

        www = new WWW(str);

        texture = www.texture;

        yield return texture;    //返回出去的texture即为文件夹中名为testPicture的图片

    }

如果想将加载出来的图片作为cube的材质贴到cube上

 StartCoroutine(loadTexture());

  cube.GetComponent().material.mainTexture =texture ;

(2)根据文件流加载文件夹中的图片

string imagePath = "c:/Users/Administrator/Desktop/TestFolder/testPicture.jpg";

FileStream files = new FileStream(imagePath, FileMode.Open);

byte[] imgByte = new byte[files.Length]; files.Read(imgByte, 0, imgByte.Length);

files.Close();

Texture2D newtexture = new Texture2D(100, 100); newtexture.LoadImage(imgByte);

将加载出来的图片贴到cube上

cube.GetComponent().material.mainTexture = newtexture;

----------------------------

# 三、复制一个文件夹中的图片到另一个文件夹中

        StartCoroutine(loadTexture()); //刚才加载出来的图片    yield return texture; 

        byte[] imagebytes = texture.EncodeToJPG();  //texture为加载出来的图片

        //新图片存放路径及图片名CopyTestPicture

        string dirPath = "c:/Users/Administrator/Desktop/NewTestFolder/CopyTestPicture.jpg";

        File.WriteAllBytes(dirPath, imagebytes);
		
------------------------------------

# 四、读取文件夹中文件（图片）个数

        string dirPath = "c:/Users/Administrator/Desktop/TestFolder";

        //判断给定的路径是否存在,如果不存在则退出

        if (!Directory.Exists(dirPath))

        {

            return;

        }

        int number = 0;

        //定义一个DirectoryInfo对象

        DirectoryInfo folder = new DirectoryInfo(dirPath);

        //通过GetFiles方法,获取di目录中的所有文件的大小

        foreach (FileInfo fi in folder.GetFiles())

        {

            number++;

        }

        print(number);
		
-------------------------

# 五、得到文件夹下的所有文件的名字

        string dirPath = "c:/Users/Administrator/Desktop/TestFolder";

        DirectoryInfo dir = new DirectoryInfo(dirPath);

        FileInfo[] files = dir.GetFiles(); //获取所有文件信息

        foreach (var file in files)

        {

            print(file.Name);

        }

----------------------------------

# 六、得到文件夹下的所有文件的路径

       string dirPath = "c:/Users/Administrator/Desktop/TestFolder";

        string[] dirs = Directory.GetFiles(dirPath);

        for (int j = 0; j < dirs.Length; j++)

        {

            print(dirs[j]);

        }
		
---------------------------------------

# 七、删除文件夹中图片

        string imagePath = "c:/Users/Administrator/Desktop/TestFolder/testPicture.jpg";

        if (File.Exists(imagePath))

        {

            File.Delete(imagePath);

        }

