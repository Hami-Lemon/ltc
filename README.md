# LrcToSrt

[![Build](https://github.com/Hami-Lemon/LrcToSrt/actions/workflows/go.yml/badge.svg?branch=master)](https://github.com/Hami-Lemon/LrcToSrt/actions/workflows/go.yml)

用于将LRC歌词文件转换成SRT字幕文件

## 功能
- [x] lrc文件转换成srt文件
- [x] 从网易云音乐或QQ音乐上获取歌词，并转换成srt文件
- [x] 从网易云音乐或QQ音乐上下载歌词

## 下载

- [github](https://github.com/Hami-Lemon/LrcToSrt/releases)
- [阿里云盘](https://www.aliyundrive.com/s/JyoM5guNgJD)  提取码：`0pn9`（不保证版本为最新版）

## 开始使用

```
Usage:
  D:\ProgrameStudy\lrc2srt\lts.exe [OPTIONS]

Application Options:
  -i, --id=                歌曲的id，网易云和QQ音乐均可。
  -I, --input=             需要转换的LRC文件路径。
  -s, --source=[163|qq|QQ] 当设置id时有效，指定从网易云（163）还是QQ音乐（qq）上获取歌词。
                           (default: 163)
  -d, --download           只下载歌词，而不进行解析。
  -m, --mode=[1|2|3]       原文和译文的排列模式,可选值有：[1] [2] [3] (default: 1)
  -v, --version            获取版本信息

Help Options:
  -h, --help               Show this help message
```

### 获取歌曲id

#### 网易云音乐

进入网易云音乐网页版，找到歌曲进入详情页，如下图：

![image-20220218154517793](https://gitee.com/Hami-Lemon/image-repo/raw/master/images/2022/02/18/20220218154518.png)

**其链接中`id=1903635166`中的`1903635166`即为该歌曲的id**

#### QQ音乐

同样进入QQ音乐网页版，找到歌曲并进入详情页，如下图：

![image-20220218154715806](https://gitee.com/Hami-Lemon/image-repo/raw/master/images/2022/02/18/20220218154716.png)

**其链接中的`000cPEL247vktn`即为歌曲id**

### 使用举例

#### 转换lrc文件

进入需要转换的lrc文件所在的目录，运行命令，其中的``file.lrc`为对应的文件名（**必须写出后缀名**，`-I`中的`I`必须大写），`save.srt`为生成的srt文件名，可以省略不写。

```cmd
lts -I file.lrc save.srt
```

#### 从网易云音乐或QQ音乐上获取歌词

按照上述步骤获取到对应歌曲的歌曲id后，通过`-i`(小写的`i`)选项指定`id`，即可获取歌词，并转换成srt文件，其中，`-s`选项指定从哪个平台获取歌词，目前只支持网易云和QQ音乐，**默认为网易云**。

```cmd
# 从QQ音乐上下载传说的世界歌词，并转换成SRT文件
lts -i 000cPEL247vktn -s qq "传说的世界.srt"
# 从网易云音乐上下载传说的世界歌词，并转换成SRT文件，这里省略了-s选项，因为默认为网易云
lts -i 1903635166 "传说的世界.srt"
```

##### *注

当`-I`和`-i`同时指定，即既传入了歌曲的id，又传入了歌词文件时，只有`-i`选项会有用。

#### 下载歌词

使用`-d`选项即可下载原始的LRC歌词文件，**而不会进行转换**

```cmd
# 从QQ音乐上下载传说的世界歌词
lts -i 000cPEL247vktn -s qq -d "传说的世界.lrc"
# 从网易云音乐上下载传说的世界歌词，这里省略了-s选项，因为默认为网易云
lts -i 1903635166 "传说的世界.lrc"
```

#### mode选项

**只有歌词包含译文时才有意义**，mode选项用于控制生成的srt文件中，原文和译文的排列方式，可选值有：

- 1：原文和译文交错排列（默认）

  ```
  7
  00:00:14,430 --> 00:00:17,920
  White shirt now red my bloody nose
  
  8
  00:00:14,430 --> 00:00:17,920
  血流不止的鼻子染红了我的白衬衫
  
  9
  00:00:17,920 --> 00:00:21,430
  Sleepin' you're on your tippy toes
  
  10
  00:00:17,920 --> 00:00:21,430
  你会踮着脚尖趁我安睡时
  
  11
  00:00:21,430 --> 00:00:24,960
  Creepin' around like no one knows
  
  12
  00:00:21,430 --> 00:00:24,960
  偷偷潜到我的身边 仿佛无人察觉
  
  ...
  ```

- 2：原文在上，译文在下

  ```
  原文
  ....
  译文
  ```

- 3：译文在下，原文在上

  ```
  译文
  ....
  原文
  ```

可通过`-m`选项指定mode
```cmd
lts -i 003FJlVU1rxjv8 -m 2 -s qq "ふわふわ时间.srt"
```

## 结束时间处理策略

因为在LRC文件中，并不包含一句歌词的结束时间，所以在转换成SRT文件时，处理策略为，**一句歌词的结束时间为下一句歌词的开始时间，最后一句歌词的结束时间为其`开始时间+10秒`**，所以在打轴时，对进入间奏的地方应该手动调整歌词的结束时间。