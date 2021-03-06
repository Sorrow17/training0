- [Introduction](#sec-1)
- [命令行](#sec-2)
  - [Windows 系统基础](#sec-2-1)
    - [Working Directory](#sec-2-1-1)
    - [绝对路径和相对路径](#sec-2-1-2)
    - [一条cmd命令的基本格式](#sec-2-1-3)
    - [`dir` 命令](#sec-2-1-4)
    - [`cd` 命令](#sec-2-1-5)
  - [OSX系统基础](#sec-2-2)
  - [命令行进阶使用](#sec-2-3)
    - [`ls` 查看文件](#sec-2-3-1)
    - [切换working directory](#sec-2-3-2)
    - [`cp` 复制文件](#sec-2-3-3)
    - [`mv` 移动文件](#sec-2-3-4)
    - [`rm` 删除文件](#sec-2-3-5)
  - [环境变量 `path`](#sec-2-4)
- [参考资料](#sec-3)

# Introduction<a id="sec-1" name="sec-1"></a>

这篇教程会讲解一个计算机基本工具的使用。

# 命令行<a id="sec-2" name="sec-2"></a>

Note: Linux用户不在教程考虑范围，因为能使用Linux的同学基本不需要看这篇教程了，不过本文仍会提及linux的许多与OSX系统相同的用法。

命令行（Command-line Terminal）是一种与计算机进行交互的基本工具，其基本工作流程就是你在一个窗口里面输入一行命令，计算机做出对这个命令的回应。很多童鞋可能会问了，既然现在的操作系统们都有了图形界面，为什么要对着一个黑乎乎的窗口用这么low的方式莱和计算机交互呢？

其实这么做是基于以下几点的考虑：

-   当完成大量重复性的工作时，很难使用图形界面来进行自动化的操作方式来完成一个任务。比如：计导TA要把收上来的100个人的作业程序都编译一遍，那么依次打开每个工程并编译时必定是十分令人沮丧的工作（请珍爱你们的计导TA们，虽然我不是计导TA。。。）。这个时候如果TA能够在命令行里面输入短短几句控制台命令，就能完成“批量编译”的过程，那么将大大降低TA批作业的时间以及大家等着出分的时间。
-   并不是所有的计算机都会有图形界面，例如大多数网络服务器就不会提供图形界面甚至是显卡，因为服务器主要是提供计算和存储资源的设备。这个时候我们和那些服务器之间的沟通方式也只有基于字符串传递的命令行了。
-   命令行能够高度配合其他的一些已有的系统工具从而大大提高（码农们的）工作效率，例如我经常使用的emacs编辑器可以通过F4切出一个命令行终端，如果要在当前目录下载一个数据包文件 XXX.avi ，就可以通过在命令行当中输入 `wget XXX.avi` 的方式来让下载文件和我当前编辑另一个文件的工作无缝衔接，而不是先切到浏览器或者迅雷再进行下载文件的操作，从而大大提高了工作效率。类似的操作也可以从一部分同学非常熟悉的sublime text编辑器通过 `` Ctrl+` `` （键盘1左边那个键）切出来一个控制台来实现。关于如何使用命令行形成“工作流”的细节是后话。

说了这么多，那我们来看看怎么用典型的方式来使用控制台吧：

## Windows 系统基础<a id="sec-2-1" name="sec-2-1"></a>

先从最基本的cmd命令说起。

无论是win几的系统，我们都是需要进入一个MS-DOS的环境，这个MS-DOS应该算是windows的命令行版本，相信很多同学至少在配置宿舍网络的时候使用过MS-DOS（ipconfig）。我一直认为按 `win键+r` 然后在运行里面输入cmd三个字母是打开MS-DOS命令行最方便的方式，当然，这里的cmd是一个模拟当年MS-DOS环境的程序（只不过伪装得太深了），所以你也可以去 `C:\windows\system32\` 这个目录下面找到程序 `cmd.exe` 然后绑定个快捷键上去（其实具体我也不知道怎么绑）。（以下统称命令行为cmd）

打开了cmd之后，我们可以看到开头的两行字：

    Microsoft Windows [Version X.X.XXXX]
    (c) 20XX Microsoft Corporation. All rights reserved.

### Working Directory<a id="sec-2-1-1" name="sec-2-1-1"></a>

紧接着一行是一个文件目录名+一个大于号 `>` 例如我打开之后的cmd就是这样：

    C:\Users\Sorrow17>_

之前这个目录名对我们使用cmd很关键，这个目录名的正统名称为 **“working directory“** （工作目录）。通常来说，一个directory（目录）在windows系统当中由一个字符串表示，格式为 `盘符（在哪个盘）：（冒号）\（反斜杠）一级目录\二级目录\...` , 举个例子来讲，我将Firefox浏览器安装在了D盘，那么这个安装目录就是 `D:\Mozilla Firefox\` 。相似的，Firefox的运行文件路径即为 `D:\Mozilla Firefox\Firefox.exe` 。回到 working directory的问题，如果我们把cmd看成是一个打开的“资源管理器”，那么这个working directory就是我们当前 <span class="underline">在资源管理器当中能够看到的那一层目录</span> 。如果你使用的是 `win+r` 然后输入cmd的快捷键组合来打开cmd，那么出现的目录名字就应该是 `C:\Users\你的用户名` 这个目录，以下假设我们教程当中的系统用户名是 <span class="underline">john</span> 。

### 绝对路径和相对路径<a id="sec-2-1-2" name="sec-2-1-2"></a>

在正式进入使用cmd命令之前，我们需要了解一个较为关键的概念：相对和绝对路径的问题。

所谓绝对路径，就是一种更什么外部因素都没有关系的方式表示一个目录/文件，我们上文提到的所有表达目录的方式都是绝对表示法，例如刚刚提到的 `D:\Mozilla Firefox\Firefox.exe` 。所谓“绝对”的含义是我们从哪个盘开始通过反斜杠 `\` 分隔开的 一级一级目录一步一步找到最终我们需要的目录的过程。然而，在很多场合下这种方式不够简便灵活，所以在操作系统中我们还有第二种选择来表示一个目录：相对路径。

所谓相对路径，就是一种跟外部环境相关的表示路径的方式。什么叫做“外部环境”呢？也许在上一段你就会有这个疑问，其实我们从现在开始可以把“外部环境”看成是一个大型目录集合，以下统称“上下文目录”（这是我发明的黑话），里面存放了许多我们经常会访问的目录。特殊地，在我们运行cmd的时候， **working directory 也会被“加载”到这个集合当中** ，假设当前的working directory是 `D:\Mozilla Firefox\` ，那么如果我跟cmd说（就是在cmd当中输入） “ `Firefox.exe` ”这个文件，cmd就会思考一些深奥的问题，比如，这个 `Firefox.exe` 看着好像不像刚才提到的绝对路径，那么他会做一件事，就是翻出那张“上下文目录”的集合，逐一地在集合中的每一个目录查找这个输入的文件，直到第一次找到了这个文件，比如我输入的 `Firefox.exe` 恰好在我的working directory（ `D:\Mozilla Firefox\` ）下的时候，系统会明白，我是要打开这个目录下的firefox浏览器。所以这个时候我们获得了一种省略前缀目录来定位一个目录/文件的方法，所以可以看成是“相对”于一些已有的路径（尤其是 working directory）来表示一个目录/文件的表示法。这种设定有一个好处，那就是我们可以少打很多字- -，当然，这么做我们还获得了较大的灵活性，这一点我们以后可以慢慢谈。

不过我们注意到很多时候我们要找的目录/文件其实并不会在“上下文目录”集合中，那么这个时候我们只能够使用绝对目录来对一个目录/文件来进行定位了，所以我们会在实际任务当中交替地使用绝对和相对路径表示法。

### 一条cmd命令的基本格式<a id="sec-2-1-3" name="sec-2-1-3"></a>

熟悉一些编程语言的同学们都知道，当我们在调用一个函数的时候，一般来说是这样的格式：

```C
foo(arg0, arg1, arg2, ...)
```

其中 `foo` 是我想要调用的函数， `arg0` 是这个函数的第一个参数， `arg1` 是第二个参数， 以此类推。类似地，如果我们把一条cmd命令看成是调用一个函数（实际上就是这样），那么上面那个函数调用就可以写成

```shell
foo arg0 arg1 arg2 ...
```

我们注意到唯一不同的地方就是括号 `()` 和逗号 `，` 都消失了，取而代之的是空格。所以我们假设一条实际上不存在的命令 `add` ，它返回所有输入参数数字的加和，那么我们需要在cmd中输入

```shell
add 1 2 3
```

它就应该返回一个合理的结果

```shell
6
```

这就是cmd当中调用一条命令的格式，下面将讲解一些windows当中常用的命令。

### `dir` 命令<a id="sec-2-1-4" name="sec-2-1-4"></a>

在cmd当中输入

```shell
C:\Users\john>dir
```

并猛击回车，我们会看到当前 `C:\Users\john\` (即working directory) 目录下所有的文件的简略信息（例如修改时间，文件类型和文件名），以我的电脑为例：

     Volume in drive C is Windows8_OS
     Volume Serial Number is 0614-5C8F
    
     Directory of C:\Users\sorrow17
    
    2014/12/25  02:23    <DIR>          .
    2014/12/25  02:23    <DIR>          ..
    2014/12/27  14:56    <DIR>          .backup
    2014/12/25  02:23             9,252 .emacs
    2014/12/28  21:15    <DIR>          .emacs.d
    2014/12/03  00:14    <DIR>          .gimp-2.8
    2014/12/04  16:20                54 .gitconfig
    2014/11/01  18:04    <DIR>          .idlerc
    2014/12/17  13:08    <DIR>          .lein
    2014/10/25  23:57    <DIR>          .m2
    2014/12/19  01:20    <DIR>          .matplotlib
    2014/12/02  23:16    <DIR>          .thumbnails
    2014/10/24  19:40                 0 agent.log
    2014/11/14  15:08    <DIR>          Contacts
    2014/12/28  18:59    <DIR>          Desktop
    2014/12/10  15:35    <DIR>          Documents
    2014/12/28  20:51    <DIR>          Downloads
    2014/11/14  15:08    <DIR>          Favorites
    2014/11/14  15:08    <DIR>          Links
    2014/11/28  11:52    <DIR>          mplayer
    2014/11/14  15:08    <DIR>          Music
    2014/12/28  18:59    <DIR>          OneDrive
    2014/12/08  20:34    <DIR>          Pictures
    2014/11/26  14:53    <DIR>          pip
    2014/11/14  15:08    <DIR>          Saved Games
    2014/11/14  15:08    <DIR>          Searches
    2014/12/28  14:45    <DIR>          Videos
                   3 File(s)          9,306 bytes
                  24 Dir(s)  66,317,733,888 bytes free

Note: 这个”文件列表”的前两行有两个文件名分别为 `.` 和 `..` 的目录，这两个文件夹分别指向当前的目录（ `C:\Users\john\` ）和当前目录的上一层目录（ `C:\Users\` ）。

当然，只能查看当前的文件夹里面有什么文件是远远不够的，我们还需要查看其他文件夹的文件的内容，所以这个时候我们应该在 `dir` 命令后面加入一个“参数”来表达我们到底想要查看哪一个目录的内容，具体而言，如果我们想要查看C盘根目录里面的内容，我们可以键入以下的命令：

    C:\Users\john>cd c:\

Note：windows当中不会区分一个目录/文件名的大小写。所以 `C:\` 和 `c:\` 代表了一样的路径，但是我们最好在所有场合将同样的目录的大小写情况保持一致。

### `cd` 命令<a id="sec-2-1-5" name="sec-2-1-5"></a>

在大量使用cmd之后，我们很多时候就会产生改变working directory的想法，因为顾名思义，working directory意为我们要开展工作的目录，如果我们要频繁对一个目录当中的文件进行操作，那么最方便的方式就是把working directory直接切换到那个目录工作，而这种切换由 `cd` 命令实现，举个例子，如果我现在打开了一个cmd，工作目录停在 `C:\Users\john\` ，现在，想打开我们的老朋友firefox浏览器，可以使用如下方式

```shell
C:\Users\john>d:
D:\>cd "mozilla firefox"
D:\Mozilla Firefox>firefox
```

这三句命令当中有4个需要注意的地方：

1.  当你要切换到的working directory跟当前的working directory不在一个盘符的时候（例如c盘切到d盘），需要先输入目标盘符的名字加一个冒号 `:` 。例如
    
    ```shell
    C:\Users\john>d:
    ```
    
    这句话表明我们想要切到d盘，如果直接输入 `cd d:\mozilla firefox\` ，是起不了任何效果的。一句话概括就是，想要切换到另一个盘符的目录，就要分为切盘符和切目录两部。
2.  输入 `cd（空格）[目标目录]` 命令就能把working directory切换到我们先要切换的目录（cd的意思就是change directory），这里我们使用相对目录的表达方式
    
    ```shell
    D:\>cd "mozilla firefox"
    ```
    
    这个 `“mozilla firefox”` 的表达方式就是一个基于working directory 已经是 `D:\` 的相对目录（假设我的firefox直接装在D盘的时候，会议刚才提到过的相对表示），当然，我们这里也可以使用严格的绝对表示法：
    
    ```shell
    D:\>cd "D:\mozilla firefox"
    ```
    
    也完全没有问题。
3.  观察力好的同学应该会发现刚才表示目录的时候多了一对引号 `“mozilla firefox\”` ，这么做并不是毫无缘由，原因就是 `mozilla firefox\` 这个路径当中包含一个空格，如果我直接输入以下这句话：
    
    ```shell
    D:\>cd mozilla firefox
    ```
    
    由于在cmd命令当中空格符是作为语法分隔符的，所以cd命令读到 mozilla之后就会”断句“，执行将目录切换到”mozilla“的命令，而我的D盘下并没有”mozilla“这个文件夹，所以命令会执行失败，另一种可能的情况是 `cd` 认为它只能执行带有一个空格的命令，所以整句话在 `cd` 的语法里面是错的，所以它也会报错，总之这么做的后果是我们100%达不到预期的效果。这个时候我们必须在目录两边加一对引号来表明接下来的目录名字是一个整体，例如 `”mozilla firefox\“` ， 而中间的空格是目录中自带的空格，不应该执行”断句“的语义，这就是我们要在两边加引号的原因。如果你输入的目录不带有空格，那么也可以不加引号，就像我们刚才一直输入的那样。
4.  注意到我们在”绝对路径和相对路径“一节当中打开firefox浏览器的方式是输入 `firefox.exe` 然后敲回车，这里为什么能直接打 `firefox` 就能运行浏览器了呢？因为 **按照惯例** ， 当你输入一条命令
    
    ```shell
    foo arg0 arg1 ...
    ```
    
    如果系统找了半天找不到这个 `foo` 命令到底是什么命令，他就会尝试在”上下文目录“（见”绝对路径和相对路径“一节）寻找一个名字叫 `foo` 的可执行文件， **同时忽略了后面的扩展名.exe** ，这里我们的working directory `D:\mozilla firefox\` 正好位于”上下文目录“当中，同时 `firefox.exe` 这个可执行文件正好位于我们的working directory当中，于是这个可执行文件 `firefox.exe` 就被顺理成章地被找到了。

我们可以看出，以上所讲的东西不光是一些关于 `cd` 命令的使用方法，而且包含了很多关于目录格式的进一步细节。笔者实在看到太多因为错误使用目录名称而造成的bug了（包括我自己写的东西），后来觉得以 **标准** 的方式使用目录和命令实在要算作基础且重要的基本功，所以花费这么多笔墨来说明这些细节问题。

## OSX系统基础<a id="sec-2-2" name="sec-2-2"></a>

## 命令行进阶使用<a id="sec-2-3" name="sec-2-3"></a>

Note: 以下将要介绍的命令在win，OSX，linux系统通用。在windows系统中，我们要使用“更为高级”的cmd程序powershell来完成这些命令，打开方式： `win+r` 然后在运行窗口中输入 `powershell` 之后会看到一个深蓝色背景的窗口。e.g. 我的初始几行是这样的：

```shell
Windows PowerShell
Copyright (C) 2013 Microsoft Corporation. All rights reserved.

PS C:\Users\sorrow17>_
```

### `ls` 查看文件<a id="sec-2-3-1" name="sec-2-3-1"></a>

对于使用windows的同学，我们同样可以在 `powershell` 中使用 `ls` 命令来查看文件，就像cmd当中的 `dir` 命令一样。而是用OSX的同学应该已经通过刚才基础部分的介绍熟悉了怎么使用 `ls` 命令。

### 切换working directory<a id="sec-2-3-2" name="sec-2-3-2"></a>

在三个系统当中我们都可以使用 `cd` 命令来切换工作目录。

### `cp` 复制文件<a id="sec-2-3-3" name="sec-2-3-3"></a>

若要将文件A(x.avi)复制为文件B(新的文件名是y.avi)，（例如A=x.avi，在windows中假设B=D:\Users\jphn\desktop\y.avi,在OSX和linux当中假设B=/usr/share/y.avi），我们可以这么做：
（省略working directory）

```shell
>cp A B
```

这样就完成了文件复制的操作。

### `mv` 移动文件<a id="sec-2-3-4" name="sec-2-3-4"></a>

若要将文件A(x.avi)”剪切“到新的位置B(新的文件名是y.avi)，（例如A=x.avi，在windows中假设B=D:\Users\jphn\desktop\y.avi,在OSX和linux当中假设B=\usr\share\y.avi），我们可以这么做：

```shell
>mv A B
```

这样就完成了文件复制的操作。
特殊的，如果我们想把一个文件A改名为文件B，那么一样执行 `mv A B` 命令就可以达到想要的效果。

### `rm` 删除文件<a id="sec-2-3-5" name="sec-2-3-5"></a>

若要将文件A(x.avi)删除，我们可以这么做：

```shell
>rm A
```

这样就删除了working diretory下面的文件A。如果我们想删除一个文件夹下面所有文件和文件夹呢？

-   在windows下，我们可以这么办：
    
    ```shell
    >rm *
    ```
    
    这里的 `*` 即 **通配符** ，表示”所有文件“的含义，所以这么一来我们就完成了对working directory的清空操作。
-   在OSX/linux下，我们可以这么办：
    
    ```shell
    >rm -r *
    ```
    
    注意到我们在 `rm` 命令后面多加了一个参数 `-r` ，这是因为在OSX/linux 系统当中我们最多能删除文件而不是文件夹，若要删除掉一个文件夹时，需要插入一个 `-r` （意为”recursively“）参数来表示我们是要”递归地“后面所给的目录名称。这里的 `*` 即 **通配符** ，表示”所有文件“（包括文件夹），如果当前工作目录包含子文件夹，那么不加 `-r` 参数的 `rm` 命令将无法正确运行，因为就像刚才说的，我们只能删除文件，而不是文件夹，只有删除操作进入”递归模式“的时候才能处理掉作为”递归文件夹“的子文件夹。

如果要清空别的目录，我们也可以如法炮制：

-   windows powershell：
    
    ```shell
    >rm C:\Users\john\desktop\*
    ```
-   OSX/linux：
    
    ```shell
    >rm -r /usr/share/*
    ```

Note：这里的清空命令仅供参考，请在自己的电脑当中安全的文件夹中尝试，造成的其他严重后果后果作者概不负责=。=
关于通配符的使用，[可以参考wikipedia-通配符](http://zh.wikipedia.org/zh-cn/%E9%80%9A%E9%85%8D%E7%AC%A6)。

## 环境变量 `path`<a id="sec-2-4" name="sec-2-4"></a>

重要的事情要放到后面的说。还记得之前几次出现的那个“上下文目录”么？我们已经知道，working directory是存在于这个“上下文目录”的集合当中的，所以我们才会能直接输入working directory当中的文件名，而不是带上那些文件名之前的目录。但是这并不代表“上下文目录”当中只包含working directory这一种目录。

那么一个很自然的问题就是，这个“上下文目录”大集合的主力军到底是什么呢？答案就是“ `path` ”（读作环境变量写作 `path` ）。

也许很多同学在之前配置各种脚本、编程语言以及系统工具的时候多多少少接触过环境变量 `path` ， `path` 在一个操作系统当中属于一种类似于 “速查手册”的存在，当我们输入一条命令的时候（假想）

```shell
foo args
```

会有80%的情况，这个 `foo` 是我们常用的系统工具命令，例如linux系统当中常用的 `grep` 命令（搜索哪些文件当中包含特定的一段话），从我们上面学到的两种运行命令的方式，我们要么是找到这个grep程序（注意，命令就是程序）的路径然后输入绝对路径，要么是 `cd` 到grep所在的目录然后键入grep命令。 由于我们经常使用的系统或者后来安装的工具命令常常散步于系统的各个角落，频繁切换目录的工作势必会大大降低我们的工作效率，而且当这些工具的目录发生变动的时候我们就必须修改我们写好的调用这些命令的程序去适应那些新的目录。。。综合考虑起来操作系统引入了环境变量 `path` 的设计。 `path` 是一个目录的集合，
当我们的命令 `foo` 既不是一个作为绝对目录的可执行文件，也不是一个位于working directory当中作为相对目录的可执行文件，系统便会在预设好的 `path` 集合当中查找这个程序到底存不存在，如果存在了系统就会执行所查找到的程序，而大多数系统工具程序都存放在 `path` 所包含的目录下。

在windows操作系统中，我们可以通过输入

```shell
>path
```

来获得 `path` 的值，在OSX/linux系统当中，我们可以通过输入

```shell
>echo $path
```

来获得 `path` 的值。
例如，我的电脑中执行 `path` 命令会得到如下结果：

```shell
C:\Users\sorrow17>path
PATH=D:\ruby213\bin;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;C:\Pr
ogram Files (x86)\Intel\iCLS Client\;C:\Program Files\Intel\iCLS Client\;C:\wind
ows\system32;C:\windows;C:\windows\System32\Wbem;C:\windows\System32\WindowsPowe
rShell\v1.0\;C:\Program Files\Intel\Intel(R) Management Engine Components\DAL;C:
\Program Files\Intel\Intel(R) Management Engine Components\IPT;C:\Program Files
(x86)\Intel\Intel(R) Management Engine Components\DAL;C:\Program Files (x86)\Int
el\Intel(R) Management Engine Components\IPT;C:\Program Files\Lenovo\Bluetooth S
oftware\;C:\Program Files\Lenovo\Bluetooth Software\syswow64;D:\bin;D:\python;C:
\Program Files (x86)\Windows Kits\8.1\Windows Performance Toolkit\;C:\Program Fi
les\Microsoft SQL Server\110\Tools\Binn\;C:\Program Files (x86)\Java\jdk1.7.0_60
\bin;D:\miktex\miktex\bin;C:\Program Files (x86)\Microsoft SDKs\TypeScript\1.0\;
D:\python\Scripts;D:\Graphviz_2.28\bin;C:\Program Files (x86)\Git\cmd;D:\mplayer
;C:\ProgramData\chocolatey\bin;C:\Program Files (x86)\Subversion\bin;D:\MinGW\bi
n;D:\gnuwin32\bin;C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\To
ols\;C:\Program Files (x86)\Subversion\bin;
```

至此，我们完成了对命令行当中一些基本的概念的介绍。

# 参考资料<a id="sec-3" name="sec-3"></a>

这篇教程只是对命令行中的基本概念做一些正规的介绍，下面是一些比较有趣的参考资料：

-   [Command Line Crash-一个命令行快速教程](http://cli.learncodethehardway.org/book/)
-   [Windows Command Prompt in 15 Minutes](http://www.cs.princeton.edu/courses/archive/spr05/cos126/cmd-prompt.html)
-   [Learning the linux shell](http://linuxcommand.org/learning_the_shell.php)
-   [Introduction to the Mac OS X Command Line](http://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line)
