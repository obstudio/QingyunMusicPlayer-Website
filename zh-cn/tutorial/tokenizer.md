
# **Tokenizer语法说明**

本文用以展示青云播放器的Tokenizer语法规范。将会实时更新。
本次更新日期：2018.1.16。

------------------

## **Tokenizer总体结构**

Tokenizer包含两个属性：`Comments`和`Sections`。其中：
`Comments`用来存放对歌曲文件的注释。
`Sections`用来存放各个乐章的信息。

`Sections`是一个数组，里面包含Section对象。每个Section对象包含三个属性：`Comments`，`GlobalSettings`和`Tracks`。其中：
`Comments`用来存放对该乐章文件的注释。
`GlobalSettings`用来存放该乐章开头声明的全局变量。
`Tracks`用来存放该乐章所包含的音轨。







