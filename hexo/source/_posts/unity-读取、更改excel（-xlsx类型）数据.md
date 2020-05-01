---
title: unity 读取、更改excel（.xlsx类型）数据
date: 2020-05-01 20:09:35
tags: [unity,xlsx]
---

# 前言

unity 读取、更改excel（.xlsx类型）数据

<!--more-->

## 一、读取excel
需要引入Excel.dll 和ICSharpCode.SharpZipLib库文件。
dll下载地址：
https://pan.baidu.com/s/1mCt0QAVGephpASBFU4uBKg    提取码：p6x3 

引入命名空间：using Excel;
代码：
```
/// <summary>
    ///返回数据的集合 
    ///数据的格式为 每一行为一条数据
    ///例：赵一|党员|1年|赵一.png| 
    /// </summary>
    /// <returns></returns>
    public List<string> LoadData()
    {
        // StreamingAssets目录下的  党员信息.xlsx文件的路径：Application.streamingAssetsPath + "/党员信息.xlsx" 
        FileStream fileStream = File.Open(Application.streamingAssetsPath + "/党员信息.xlsx", FileMode.Open, FileAccess.Read);
        IExcelDataReader excelDataReader = ExcelReaderFactory.CreateOpenXmlReader(fileStream);
        // 表格数据全部读取到result里(引入：DataSet（
using System.Data;） 需引入 System.Data.dll到项目中去)
        DataSet result = excelDataReader.AsDataSet();

        // 获取表格有多少列 
        int columns = result.Tables[0].Columns.Count;
        // 获取表格有多少行 
        int rows = result.Tables[0].Rows.Count;
        // 根据行列依次打印表格中的每个数据 
        
        List<string> excelDta = new List<string>();

        //第一行为表头，不读取
        for (int i = 1; i < rows; i++)
        {
            value = null;
            all = null;
            for (int j = 0; j < columns; j++)
            {
                // 获取表格中指定行指定列的数据 
                value = result.Tables[0].Rows[i][j].ToString();
                if (value == "")
                {
                    continue;
                }
                all = all + value + "|";
            }
            if (all != null)
            {
                print(all);
                excelDta.Add(all);
            }
        }
        return excelDta;
    }
```
## 二、写入excel数据
需要引入 EPPlus.dll  下载地址同上 
引入命名空间： 
using OfficeOpenXml;
```
/// <summary>
    /// list内容格式
    /// 赵一|党员|1年|赵一.png| 
    /// </summary>
    /// <param name="newList"></param>
    public void WriteExcel(List<string> newList)
    {
        //自定义excel的路径

        string path = Application.streamingAssetsPath + "/党员信息.xlsx";
       // print(Application.dataPath);

        FileInfo newFile = new FileInfo(path);

        if (newFile.Exists)
        {
            //创建一个新的excel文件

            newFile.Delete();

            newFile = new FileInfo(path);
        }
        
        //通过ExcelPackage打开文件
        using (ExcelPackage package = new ExcelPackage(newFile))
        {
            //在excel空文件添加新sheet

            ExcelWorksheet worksheet = package.Workbook.Worksheets.Add("message");
            //添加列名

            worksheet.Cells[1, 1].Value = "姓名";

            worksheet.Cells[1, 2].Value = "职务";

            worksheet.Cells[1, 3].Value = "党龄";

            worksheet.Cells[1, 4].Value = "图片名";
            
            for (int i = 0; i < newList.Count; i++)
            {
                string[] messages = newList[i].Split('|'); //赵一|党员|1年|赵一.png| 
                string itemName = messages[0];
                string itemWork = messages[1];
                string itemYear = messages[2];
                string imageName = messages[3];
                //添加一行数据

                int num = i + 2;

                worksheet.Cells["A"+ num].Value = itemName;

                worksheet.Cells["B" + num].Value = itemWork;

                worksheet.Cells["C" + num].Value = itemYear;

                worksheet.Cells["D" + num].Value = imageName;
            }
            
            //保存excel
            package.Save();
            print("重写完成");
        }
    }
```
为避免打包exe后读取不到excel数据，须将
Unity\Editor\Data\Mono\lib\mono\unity目录下的一系列i18n相关dll导入项目Plugins文件夹中。
完整代码：
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.IO;
using Excel;
using System.Data;
using OfficeOpenXml;
public class LoadExcelData : MonoBehaviour {
    public static LoadExcelData instance;
   
    string value;
    string all;

    private void Awake()
    {
        instance = this;
    }


    private void Start()
    {
      //  LoadData();
    }

    /// <summary>
    ///返回数据的集合 
    ///数据的格式为 每一行为一条数据
    ///例：赵一|党员|1年|赵一.png| 
    /// </summary>
    /// <returns></returns>
    public List<string> LoadData()
    {
        // StreamingAssets目录下的  党员信息.xlsx文件的路径：Application.streamingAssetsPath + "/党员信息.xlsx" 
        FileStream fileStream = File.Open(Application.streamingAssetsPath + "/党员信息.xlsx", FileMode.Open, FileAccess.Read);
        IExcelDataReader excelDataReader = ExcelReaderFactory.CreateOpenXmlReader(fileStream);
        // 表格数据全部读取到result里
        DataSet result = excelDataReader.AsDataSet();

        // 获取表格有多少列 
        int columns = result.Tables[0].Columns.Count;
        // 获取表格有多少行 
        int rows = result.Tables[0].Rows.Count;
        // 根据行列依次打印表格中的每个数据 
        
        List<string> excelDta = new List<string>();

        //第一行为表头，不读取
        for (int i = 1; i < rows; i++)
        {
            value = null;
            all = null;
            for (int j = 0; j < columns; j++)
            {
                // 获取表格中指定行指定列的数据 
                value = result.Tables[0].Rows[i][j].ToString();
                if (value == "")
                {
                    continue;
                }
                all = all + value + "|";
            }
            if (all != null)
            {
                print(all);
                excelDta.Add(all);
            }
        }
        return excelDta;
    }

    /// <summary>
    /// list内容格式
    /// 赵一|党员|1年|赵一.png| 
    /// </summary>
    /// <param name="newList"></param>
    public void WriteExcel(List<string> newList)
    {
        //自定义excel的路径

        string path = Application.streamingAssetsPath + "/党员信息.xlsx";
       // print(Application.dataPath);

        FileInfo newFile = new FileInfo(path);

        if (newFile.Exists)
        {
            //创建一个新的excel文件

            newFile.Delete();

            newFile = new FileInfo(path);
        }
        
        //通过ExcelPackage打开文件
        using (ExcelPackage package = new ExcelPackage(newFile))
        {
            //在excel空文件添加新sheet

            ExcelWorksheet worksheet = package.Workbook.Worksheets.Add("message");
            //添加列名

            worksheet.Cells[1, 1].Value = "姓名";

            worksheet.Cells[1, 2].Value = "职务";

            worksheet.Cells[1, 3].Value = "党龄";

            worksheet.Cells[1, 4].Value = "图片名";
            
            for (int i = 0; i < newList.Count; i++)
            {
                string[] messages = newList[i].Split('|'); //赵一|党员|1年|赵一.png| 
                string itemName = messages[0];
                string itemWork = messages[1];
                string itemYear = messages[2];
                string imageName = messages[3];
                //添加一行数据

                int num = i + 2;

                worksheet.Cells["A"+ num].Value = itemName;

                worksheet.Cells["B" + num].Value = itemWork;

                worksheet.Cells["C" + num].Value = itemYear;

                worksheet.Cells["D" + num].Value = imageName;
            }
            
            //保存excel
            package.Save();
            print("重写完成");
        }
    }
}

```
