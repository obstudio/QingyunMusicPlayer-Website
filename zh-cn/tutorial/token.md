
# **Tokenizer语法说明**

本文用以展示青云播放器的Tokenizer语法规范。将会实时更新。
本次更新日期：2018.1.16。

------------------

## **Tokenizer总体结构**

Tokenizer包含两个属性：`Comments`和`Sections`。其中：     
`Comments`用来存放对歌曲文件的注释。     
`Sections`用来存放各个乐章的信息。

`Comments`是一个数组，里面包含有注释信息，以字符串为形式分行存储。    
`Sections`是一个数组，里面包含Section对象。每个Section对象包含三个属性：`Comments`，`GlobalSettings`和`Tracks`。其中：     
`Comments`用来存放对该乐章文件的注释。     
`GlobalSettings`用来存放该乐章开头声明的全局变量。     
`Tracks`用来存放该乐章所包含的音轨。

`GlobalSettings`是一个一维数组，里面包含有各种全局变量声明。    
`Tracks`是一个二维数组，里面包含有每一个音轨的内容。这些内容将在下面逐一介绍。

所以一个Tokenizer的总体结构如下：
```JSON
{
    "Comments": [ //歌曲的注释，允许多行
        "曲名：xxx",
        "作曲：xxx"
        //使用空行作为歌曲注释与第一个音轨注释的分隔
        //如果没有空行，这些信息会被认为是第一个乐章的注释
    ],
    "Sections": [
        { //第一个乐章
            "Comments": [ //乐章的注释，允许多行
                "---------- Section A ----------"
            ],
            "GlobalSettings": [
                //全局设置，使用方法与音轨内相同
            ],
            "Tracks": [
                [
                    //第一个音轨


----------


----------


                ],
                [
                    //第二个音轨
                ]
            ]
        },
        {
            //第二个乐章
        }
    ]
}
```







