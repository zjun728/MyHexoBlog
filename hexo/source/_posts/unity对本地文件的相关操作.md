---
title: unity对本地文件的相关操作
date: 2018-04-20 16:39:47
tags: [unity]
---
# 前言

unity 对本地文件的一些操作，包括:创建文件夹，加载文件夹内图片，删除文件夹文件，等

<!--more-->

# 一、在桌面创建文件夹 C:\Users\Administrator\Desktop(桌面路径)
```javascript

        string path = "c:/Users/Administrator/Desktop/TestFolder"; //TestFolder要创建得文件夹的名字

        if (!Directory.Exists(path))
        {
            Directory.CreateDirectory(path);
        }
```
------------------------------

# 二、(1)读取文件夹中的图片（WWW 类加载） 图片名字为 testPicture.jpg

```javascript
Texture2D texture;

 IEnumerator loadTexture() {

        //这里的文件路径 ，需要加上file///

        string str = "file:///c:/Users/Administrator/Desktop/TestFolder/testPicture.jpg";

        www = new WWW(str);

        texture = www.texture;

        yield return texture;    //返回出去的texture即为文件夹中名为testPicture的图片

    }

//如果想将加载出来的图片作为cube的材质贴到cube上

 StartCoroutine(loadTexture());

 cube.GetComponent().material.mainTexture =texture ;

//(2)根据文件流加载文件夹中的图片

string imagePath = "c:/Users/Administrator/Desktop/TestFolder/testPicture.jpg";

FileStream files = new FileStream(imagePath, FileMode.Open);

byte[] imgByte = new byte[files.Length]; 

files.Read(imgByte, 0, imgByte.Length);

files.Close();

Texture2D newtexture = new Texture2D(100, 100); newtexture.LoadImage(imgByte);

//将加载出来的图片贴到cube上

cube.GetComponent().material.mainTexture = newtexture;

```
----------------------------

# 三、复制一个文件夹中的图片到另一个文件夹中

```javascript
        StartCoroutine(loadTexture()); //刚才加载出来的图片    yield return texture; 

        byte[] imagebytes = texture.EncodeToJPG();  //texture为加载出来的图片

        //新图片存放路径及图片名CopyTestPicture

        string dirPath = "c:/Users/Administrator/Desktop/NewTestFolder/CopyTestPicture.jpg";

        File.WriteAllBytes(dirPath, imagebytes);
		
```
		
------------------------------------

# 四、读取文件夹中文件（图片）个数

```javascript
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
		
```
-------------------------

# 五、得到文件夹下的所有文件的名字

```javascript
        string dirPath = "c:/Users/Administrator/Desktop/TestFolder";

        DirectoryInfo dir = new DirectoryInfo(dirPath);

        FileInfo[] files = dir.GetFiles(); //获取所有文件信息

        foreach (var file in files)
        {
            print(file.Name);
        }
		
```
----------------------------------

# 六、得到文件夹下的所有文件的路径

```javascript
       string dirPath = "c:/Users/Administrator/Desktop/TestFolder";

        string[] dirs = Directory.GetFiles(dirPath);

        for (int j = 0; j < dirs.Length; j++)
        {
            print(dirs[j]);
        }
		
```
---------------------------------------

# 七、删除文件夹中图片

```javascript
        string imagePath = "c:/Users/Administrator/Desktop/TestFolder/testPicture.jpg";

        if (File.Exists(imagePath))
        {
            File.Delete(imagePath);
        }
		
```
---------------------------------------

八、得到当前文件夹根目录下所有的文件夹(子文件夹内的内容不会输出)或文件名字

```javascript
    /// <summary>

    /// 得到当前文件夹根目录下所有的文件夹(子文件夹内的内容不会输出)或文件名字

    /// </summary>

    /// <param name="path"></param>

    public List<string> GetFolderRootFileName(string path)

    {

        List<string> fileNameList = new List<string>();

        string[] directoryEntries = Directory.GetFileSystemEntries(path);

        // DirectoryInfo dir = new DirectoryInfo(dirPath);

        // FileInfo[] files = dir.GetFiles("*", SearchOption.TopDirectoryOnly); //获取所有文件信息       

        for (int i = 0; i < directoryEntries.Length; i++)

        {

            string fileName = directoryEntries[i].Split('\\')[1];

            if (fileName.EndsWith(".meta"))

            {

                continue;

            }

            fileNameList.Add(fileName);

        }

        return fileNameList;

    }
```

---------------------------------------

九、得到当前文件夹下所有的子文件夹内的所有文件的路径（包括子文件夹文件）

```javascript
    /// <summary>

    /// 得到当前文件夹下所有的子文件夹内的所有文件的路径（包括子文件夹文件）FileInfo.FullName/FileInfo.Name

    /// </summary>

    /// <param name="path"></param>

    /// <returns></returns>

  public List<FileInfo> GetAllFileinfoFromPath(string path)

    {

        List<FileInfo> fileinfo= new List<FileInfo>();

        DirectoryInfo dir = new DirectoryInfo(path);

        FileInfo[] files = dir.GetFiles("*", SearchOption.AllDirectories); //获取所有文件信息       

        for (int i = 0; i < files.Length; i++)

        {

            if (files[i].Name.EndsWith(".meta"))

            {

                continue;

            }

          //  print(files[i].FullName); //文件路径

          //  print(files[i].Name);    //文件名字

            fileinfo.Add(files[i]);

        }

        return fileinfo;

    }
```

---------------------------------------

十、将文件夹某一文件复制到另一个文件夹中

```javascript
    /// <summary>

    /// 文件复制

    /// </summary>

    /// <param name="targetDir"==要复制到的文件夹>  </param>

    /// <param name="fileName"==要复制的文件名></param>

    public static void CopyDirectory(string targetDir,string fileName)

    {

        //起始文件夹

        string fromDir= "C:/Users/Administrator/Desktop/zhang/unity外部配置";

        DirectoryInfo source = new DirectoryInfo(fromDir);

        DirectoryInfo target = new DirectoryInfo(targetDir);

        FileInfo[] files = source.GetFiles();

        File.Copy(fromDir+"/"+ fileName, Path.Combine(targetDir, fileName), true);

    }
```