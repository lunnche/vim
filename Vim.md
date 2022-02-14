# Vim基础

```
$ git clone https://github.com.cnpmjs.org/scrooloose/nerdtree.git ~/.vim/pack/plugins/start/nerdtree  "装插件的方法
```

git clone 慢的解决办法

使用国内镜像，目前已知Github国内镜像网站有[github.com.cnpmjs.org](https://github.com.cnpmjs.org/)和[git.sdut.me/](https://git.sdut.me/)。速度根据各地情况而定，在clone某个项目的时候将github.com替换为github.com.cnpmjs.org即可。

```
//这是我们要clone的
git clone https://github.com/Hackergeek/architecture-samples
 
//使用镜像
git clone https://github.com.cnpmjs.org/Hackergeek/architecture-samples
 
//或者
 
//使用镜像
git clone https://git.sdut.me/Hackergeek/architecture-samples
```

所有资料都可以在官方GitHub仓库PacketPublishing/Mastering-Vim中找到。



原来的Vi版本是针对远程终端开发出来的，那时的带宽和网速都有限。但正是这些限制使Vi的文本编辑流程变得高效和专业。

原书GitHub

https://github.com/PacktPublishing/Mastering-Vim/tree/master/Chapter01  

无模式（modeless）：每个界面元素只有一个功能，每个按钮都对应于屏幕上的一个字母或某种其他操作，每个按键（或组合键）总是做同样的事：此应用程序总是以单一模式来执行操作。

模式（modal）界面：根据上下文不同，每个行为可能对应于不同操作。

Vim就是一款模式编辑器，上下文不同，单击一个按钮会产生不同的效果。例如，在insert模式下，输入o得到o，在另一个模式下，输入o在光标下添加新行。

是用Vim就像在与编辑器对话，比如：

```
d3w    delete 3 words  删除后面3个单词
ci     change inside [quotes] 改变引号里面的文本
```

使用Vim的好处：

* 快
* 处理文本手不离键盘，不用时不时去摸鼠标
* 不需要按17次方向键到达页面某个位置
* 不需要在复制粘贴时用鼠标小心翼翼地选择文本

## Installation

Linux和macOS中自带，但老旧需更新

### Setting up on Windows

两种方式：

* 在Cygwin中安装命令行版的Vim
* 安装图形界面gVim（也有在cmd.exe上运行的终端版本）

### Unix-like experience with Cygwin

Cygwin致力于将强大的UNIX Shell命令行及相关的支撑工具带到Windows中

1 安装Cygwin

去官网下载64位版安装文件，安装过程中选择：

Download source: Install from Internet
Root directory: C:\cygwin64 (or a recommended default)
Install for: all users
Local package directory: C:\Downloads (or a recommended default)
Internet connection: Use System Proxy Settings
Download site: http://cygwin.mirror.constant.com (or any available
option)  

然后，选择软件包，安装以下包：

Editors类中的vim、gvim、vim-doc

Net类中的curl

Devel类中的git

Utils类中的dos2unix

安装方法是：在搜索栏搜关键字，点Bin？下的倒三角选版本，Src？不用选

选好后点Next

安装到autorebase.dash时长时间不动，继续等就好，网上一个大哥说等了10多个小时感觉瞎掰的手动狗头

然后再默认next几下就装好了。

后续如果还需要安装其他包，再次打开安装包可执行文件即可，然后选择需要的软件包。

2 使用Cygwin

如果在Vim中遇到^M字符无法识别，则对相应的文件执行dos2unix命令就可以解决。

### Visual Vim with gVim

到vim官网下载gvim

启用Create.bat files for command line use，这个选项让你可以在Windows命令提示符中使用vim。

设置如下选项：

* Select the type of install: Typical, (after Create .bat files for command line use
  is enabled, type of install value changes to Custom automatically).
* Do not remap keys for Windows behavior
* Right button has a popup menu, left button starts visual mode
* Destination Folder: C:\Program Files (x86)\Vim (or a recommended
  default)

装完了。

## 安装结果验证和故障排除

输入

```
$ vim --version
```

确认Vim相关功能是否启用

+号表示启用，-号未启用。如图显示支持python2 不支持python3 要想解决这个问题，可以：

1 重新编译Vim并启用python3

2 寻找一个支持python3的Vim发布版本。

Vim可以支持的所有功能列表参见：help feature-list.

假设想重新编译一个支持python3的Vim，在Linux中录入：

```
$ git clone https://github.com/vim/vim.git
$ cd vim/src
$ ./configure --with-features=huge --enable-python3interp
$ make
$ sudo make install
```

传入`--with-features=huge`编译选项，是为了启用Vim大部分功能。但此选项不涉及语言的绑定，因此需要显式地启用python3。

如果你的Vim运行不如预期，可能是因为缺功能，在网上搜 `Install Vim <version> with +<feature> on <operating system>`可能会有帮助。

## Configuring Vim with your .vimrc

在类UNIX系统中，以句点`.`开头的文件为隐藏文件。为了看到这些文件，可以运行ls -a命令行。

Linux和MacOS中，.vimrc位于用户根目录中（完整路径为/home/<用户名>/.vimrc）。可以通过如下命令进入：

```
$ echo $HOME
```

Windows系统不允许文件名出现句点，因此配置文件名为_vimrc，其路径通常为C:\Users\<用户名>\\\_vimrc。可通过如下命令进入：

```
$ echo %USERPROFILE%
```

如果遇到问题，可以打开Vim，输入  ：（前面这个冒号是要输的）echo $MYVIMRC，然后按Enter键。Vim会显示它正在使用的.vimrc的路径。

找到操作系统存储Vim配置文件的目录，将如下代码放入：

```
syntax on " Enable syntax highlighting.
filetype plugin indent on " Enable file type based indentation.
set autoindent " Respect indentation when starting a new line.
set expandtab " Expand tabs to spaces. Essential in Python.
set tabstop=4 " Number of spaces tab is counted for.
set shiftwidth=4 " Number of spaces to use for autoindent.
set backspace=2 " Fix backspace behavior on most terminals.
colorscheme murphy " Change a colorscheme.
```

上述代码中，双引号开头的是注释，会被Vim忽略。上面的设置包括语法高亮和一致缩进，还解决了不同环境中退格键可能出现不同行为的问题。

编写Vim配置文件时，可以先尝试相应的设置，再写入.vimrc文件。操作为：先输入冒号，然后输入相应的命令，再按Enter。如set autoindent。如果想知道某种设置当前的值，可以在命令后面加上问号，如：set tabstop?命令会显示出当前的tabstop值。

Vim8自带如下主题配色。blue,darkblue, default, delek, desert, elflord, evening, industry,koehler, morning, murhpy, pablo, peachpuff, ron, shine, slate,torte, zellner.   输入:colorscheme \<name> 来设置颜色。

也可以在所有可用配色主题之间循环切换，方法是输入：colorscheme ，然后输入一个空格，并多次按Tab键。

## 在不同环境中找到vimrc

vimrc在哪？

在$HOME/.vimrc

在Linux下的Vim中输入:version命令（可能你使用Linux下的vi命令打开编辑器，不过在大多数Linux中，vi命令打开的就是Vim），显示大概如下：

```
:version
VIM - Vi IMproved 8.0 (2016 Sep 12, compiled Jun 26 2017 11:44:34)
Included patches: 1-678
Compiled by Easwy Yang <easwy.......>
   system vimrc file: "VIM/vimrc"
     user vimrc file: "HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "HOME/.exrc"
  system gvimrc file: "VIM/gvimrc"
    user gvimrc file: "$HOME/.gvimrc"
2nd user gvimrc file: "~/.vim/gvimrc"
...
```

在上面，我们看到列出了几个 vimrc 文件，有一个系统的 vimrc 文件，还有用户的 vimrc 文件，以及系统和用户 gvimrc 文件。出于和vi兼容的目的，vim也支持vi的exrc配置文件。

接着，在 Windows 系统中输入 `:version` 命令，可以看到如下输出(这里使用的是预编译的 Vim 8.0)：

```
VIM - Vi IMproved 8.0 (2016 Sep 12, compiled Apr 23 2017 20:01:28)
MS-Windows 32-bit GUI version with OLE support
Included patches: 1-586
Compiled by mool@tororo
   system vimrc file: "VIM\vimrc"
     user vimrc file: "HOME\_vimrc"
 2nd user vimrc file: "HOME\vimfiles\vimrc"
 3rd user vimrc file: "VIM\_vimrc"
      user exrc file: "HOME\_exrc"
  2nd user exrc file: "VIM\_exrc"
  system gvimrc file: "VIM\gvimrc"
    user gvimrc file: "HOME\_gvimrc"
2nd user gvimrc file: "HOME\vimfiles\gvimrc"
3rd user gvimrc file: "VIM\_gvimrc"
```

比较一下上面两个 `:version` 命令的输出，我们发现：
– 在windows下，有两个可选的用户 vimrc 文件，一个是 `$HOME\_vimrc`，另外一个是 `$VIM\_vimrc`。 Vim 启动时，会先尝试执行系统的 vimrc 文件(通常此文件不存在)，然后将按照上述顺序查找用户 vimrc，并使用所找到的第一个用户 vimrc 中的配置，忽略其余的用户 vimrc。
– 在Linux下使用的 vimrc 文件名为 `.vimrc`，而在 Windows 下因为不支持以点(.)开头的文件名，vimrc 文件的名字使用 `_vimrc`。不过，在Linux下，如果未找到名为 `.vimrc` 的文件，也会尝试查找名为 `_vimrc` 文件；而在 Windows 下也是这样，只不过查找顺序颠倒一下，如果未找到名为 `_vimrc` 的文件，会去查找 `.vimrc`。
– 从这里可以看出，vimrc 的执行先于 gvimrc。所以我们可以把全部 vim 配置命令都放在 vimrc 中，不需要用 gvimrc。

对于vim初学者，如果不知道 `$HOME` 或者 `$VIM` 具体是哪个目录，可以在 vim 中用下面的命令查看：

```
:echo $VIM
:echo $HOME
```

Windows 版本的 Vim 在安装时，缺省会安装一个 `$VIM/_vimrc`，你可以直接修改这个 `_vimrc`，加入你自己的配置(使用 `:e $VIM/_vimrc` 即可打开此文件)。或者，你也可以在 Windows 中增加一个名为 `HOME` 的环境变量(`控制面板` -> `系统` –> `高级` –> `环境变量`)，然后把你的 vimrc 放在 HOME 环境变量所指定的目录中。从上面 `:version` 命令的输出看到，`$HOME/_vimrc` 如果存在，就会执行这个文件中的配置，从而跳过 `$VIM/_vimrc`。

如果使用 `vim -u filename` 命令来启动 Vim，则会用你指定的 `filename` 作为 Vim 的配置文件(在调试你的 vimrc 时有用)；如果用 `vim -u NONE` 命令启动 Vim，则不读取任何 vimrc 文件：当你怀疑你的 vimrc 配置有问题时，可以用这种方式跳过 vimrc 的执行。

## 常用操作

### Opening files

在命令行终端中，输入

```
$ vim animal_farm.py
```

如果上述文件存在就打开它，否则创建它。

如果Vim已经打开，如何加载文件？输入：

```
:e anmimal_farm.py
```

输入冒号表示进入命令行模式，此模式下输入的文字会被Vim解析为命令。：e表示编辑（edit）

Vim的帮助文档中通常将Enter键记为\<CR>（carriage return，回车）

## 修改文字

Vim默认处于正常模式（normal mode），此时每个键都对应于某个命令。

输入i将进入插入模式（insert mode），此时行为和其他无模式编辑器相似。

输入文字。

输入完后按Esc键可以返回到正常模式。

### 保存和关闭文件

保存文件   ：w

：w后可接一个文件名，会另存为新文件，且当前文件也切换到新文件。

退出           ：q

保存并退出     ：wq

修改后不保存强制退出     ：q!

Vim很多命令都有长短两个版本。比如

:e        :edit

:w        :write

:q         :quit

Vim手册中，命令的可选部分用中括号表示，如w[rite]，e[dit]

退出Vim后

```
$ ls
$ python animal_farm.py
$ python animal_farm.py cat dog sheep
```

在UNIX中，ls表示列出当前目录内容。

python animal_farm.py 表示用python解释器执行这个脚本

python animal_farm.py cat dog sheep 表示执行脚本，并传入（cat,dog,sheep）3个参数。

> 可能遇到的问题：
>
> 输入 $python animal_farm.py后
>
> 显示：-bash: /cygdrive/c/Users/user/AppData/Local/Microsoft/WindowsApps/python: Permission denied
>
> 解决办法：运行Cygwin64 Terminal时以管理员身份运行

## 交换文件

Vim用交换文件（swap files）跟踪文件的变化情况。当用户编辑文件时，Vim会自动产生交换文件。

其作用是恢复文件内容，以防用户的Vim、SSH会话或系统崩溃。一旦出现上述问题，或因失误意外退出Vim，

再次用Vim打开同一个文件时，就会出现交换文件选项，此时输入：

r    从交换文件恢复文件

d   直接忽略交换文件

如果决定从交换文件中恢复，可以执行如下操作防止下次打开文件时再次提示恢复：

删除<文件名>.swp。

默认情况，Vim在原始文件所在目录下生成类似于\<filename>.swp或\.\<filename>.swp文件，为避免这些交换文件污染文件系统，可以修改这个默认行为，使Vim将所有交换文件都统一存放在同一个目录中。要实现这个设置，可以在.vimrc文件中加入如下内容。

```
set diredctory=$HOME/.vim/swap//
```

如果使用Windows系统，设置命令为：

```
set directory=%USERDATA%\.vim\swap//（注意最后两个斜线的方向）
```

或者，也可以选择完全禁止交换文件，在.vimrc中加入set noswapfile即可。

## Moving around

| 按键 | 替代键 | 行为 |
| ---- | ------ | ---- |
| h    | 左箭头 | 左移 |
| j    | 下箭头 | 下移 |
| k    | 上箭头 | 上移 |
| l    | 右箭头 | 右移 |

上述命令前加数字，表示重复这条命令的次数，这条语法适用很多命令。

| 按键 | 行为                           |
| ---- | ------------------------------ |
| w    | 逐个狭义单词移动               |
| e    | 向前移动知道最近狭义单词的结尾 |
| W    | 逐个广义单词移动               |
| E    | 向前移动直到广义单词的结尾     |
| b    | 向后移动到狭义单词开头         |
| B    | 向后移动到广义单词开头         |

| 命令 | 行为             |
| ---- | ---------------- |
| {    | 向后移动一个段落 |
| }    | 向前移动一个段落 |

## 插入模式下的简单编辑

修改命令c：在删除一部分文字后立刻进入插入模式。该命令是一个复合命令，它后面必须指定其他命令，用于告诉Vim修改哪一部分。

| 命令 | 执行前                           | 执行后                        |
| ---- | -------------------------------- | ----------------------------- |
| cw   | farm = add_animal(`f`arm,animal) | farm = add_animal(`,`animal)  |
| c3e  | farm = add_animal(fa`r`m,animal) | farm = add_animal(fa)         |
| cb   | farm = add_animal(farm,ani`m`al) | farm = add_animal(farm,`m`al) |
| c4l  | farm = add_animal(farm,`a`nimal) | farm = add_animal(farm,`a`l)  |
| cW   | farm = add_animal(farm,`a`nimal) | farm = add_animal(farm,` `    |

注意：

1 逗号视为一个词。

2 cw和ce行为类似，这是Vim前身Vi的历史遗留问题。



<命令><数字><移动或一个文本对象>是普适的语法结构，可以将数字放在命令之前或之后。



若想将farm = add_animal(farm,animal)修改为farm = add_animal(farm,creature)，可依照下表依次执行命令：

| 代码行                                   | 行为                                     |
| ---------------------------------------- | ---------------------------------------- |
| ==f==arm = add_animal(farm,animal)       | 将光标置于行首                           |
| farm = add_animal(farm,==a==nimal)       | 按3w让光标跳过广义单词到达animal的首字母 |
| farm = add_animal==(==farm,==)==         | 按cw删除单词animal，然后立刻进入插入模式 |
| farm = add_animal==(==farm,creature==)== | 输入creature                             |
| farm = add_animal(farm,creatur==e==)     | 按Esc键回到正常模式                      |

只删除，不插入，用命令d，只不过这时候的w和e的行为会标准得多。

| 命令 | 执行之前                           | 执行之后                        |
| ---- | ---------------------------------- | ------------------------------- |
| dw   | farm = add_animal(==f==arm,animal) | farm = add_animal(==,==animal)  |
| d3e  | farm = add_animal(fa==r==m,animal) | farm = add_animal==(==fa==)==   |
| db   | farm = add_animal(farm,ani==m==al) | farm = add_animal(farm,==m==al) |
| d4l  | farm = add_animal(farm,==a==nimal) | farm = add_animal(farm,==a==l)  |
| dW   | farm = add_animal(farm,==a==nimal) | farm = add_animal(farm, ` `     |

还有两个快捷命令用于修改或删除一整行。

| 命令 | 行为                                                         |
| ---- | ------------------------------------------------------------ |
| cc   | 消除郑行，然后进入插入模式。保持当前的缩进水平，这在编程时很有用 |
| dd   | 删除整行                                                     |

如果在选择正确的光标移动命令时存在困难，也可以用可视（visual)模式来选择待修改的文本。按v键进入可视模式，然后通过常用光标移动命令调整选择的文本。一旦选择完毕，就可以运行相应的命令（如用c键来修改或用d键来删除）。

## 持久性的撤销和重复

按u撤销最后一次操作（Vim的撤销历史记录不是线性的），按Ctrl+r可以重做此操作。

Vim还支持在不同会话间持久保存撤销历史，从而允许撤销几天前的操作。

可以在.vimrc中进行如下设置来启用持久性撤销。

```
set undofile
```

这会在系统中为每个编辑过的文件保留一个撤销历史记录文件，显得很混乱。也可以将这些文件保存在同一目录中，配置如下：

```
" 为所有文件设置持久性撤销
set undofile
if !isdirectory("$HOME/.vim/undodir")
  call mkdir("$HOME/.vim/undodir","p")
endif
set undodir="$HOME/.vim/undodir"
```

对于Windows系统，将上述设置中的目录换成%USERPROFILE%_vim。

且Windows下的配置文件是_vimrc，而不是.vimrc。

## 通过：help阅读Vim手册

通过翻页键（PageUp和PageDown）可以浏览手册内容（Ctrl+b和Ctrl+f组合键也能起到翻页的效果）

想深入了解某个命令比如cc，输入：

```
:h cc
```

注意：

```
:h search
```

并不表示如何在Vim中搜索一个字符串，它会进入表达式评估（expresssion evaluation）页面。

要找到正确的词条，输入 :h search（先不要回车），然后按Ctrl+D，得到一个包含search的标签列表。其中一项为search-commands，这才是我们需要的。

关于搜索功能，可以在帮助页面（或在Vim中打开的任何文件）中输入“/关键字”进行正向搜索，或输入“？关键字”进行反向搜索。输入`:noh`来结束高亮。

***

***

# Advanced Editing and Navigation

用到的GitHub页面：https://github.com/PacktPublishing/Mastering-Vim/blob/master/Chapter02  

## 安装插件

1 创建一个存储插件的目录，执行下列命令

```
$ mkdir -p ~/.vim/pack/plugins/start
```

> 如果在Windows系统中使用gVim，则需要在用户目录（通常是C:\Users\<用户名>）下创建vimfiles目录，然后在其中创建子目录pack\plugins\start。

2 使Vim能够自动加载每个插件的文档（Vim默认不会这么做）。在~/.vimrc 文件（在Windows系统中为用户目录下的_vimrc文件）中添加下列代码。sc

```
packloadall   “加载所有插件
silent! helptags ALL  “为所有插件加载帮助文档
```

然后，每次安装插件都可按照下列步骤进行。

1 在GitHub上找到想要安装的插件。比如，安装scrooloose/nerdtree（注意，这里的scrooloose/nerdtree为该GitHub仓库的唯一标识，实际地址为https://github.com/scrooloose/nerdtree.git）。若已经安装了Git，则可以找到此Git仓库的克隆地址，然后运行：

```
$ git clone https://github.com/scrooloose/nerdtree.git ~/.vim/pack/plugins/start/nerdtree
```

> 如果没有安装git，或者在Windows系统中使用gVim，则可以在GitHub页面上找到克隆或下载（Clone or download）按钮，下载ZIP压缩包，然后将其解压到相应的插件目录中，比如在Linux系统中为目录 ~/.vim/pack/plugins/start/nerdtree，而在Windows系统中为用户目录下的子目录vimfiles/pack/plugins/start/nerdtree。

2 重启Vim之后，即可使用插件进行相关操作。

## 组织工作区

* Vim内部用缓冲区来表示文件，通过缓冲区，读者可以在不同文件之间快速切换。
* Vim用多个窗口在同一屏幕显示多个文件
* Vim用标签页对窗口进行分组
* Vim用折叠效果来隐藏或展开一个文件的部分内容、从而让读者可以更容易地浏览文件内容。

以+--开头的行表示折叠，它隐藏了文件的部分内容。

### 缓冲区

缓冲区是文件的内部表示，每个打开的文件都有一个缓冲区。比如，通过命令行vim animal_farm.py打开一个文件，然后可以用：ls命令看到现有的缓冲区列表。

很多命令都有别名或的等价命令，：ls也不例外，它和:buffers及:files实现同样的功能。

缓冲区信息含义如下：

* 1为缓冲区编号，在整个Vim会话中，它的值保持不变。
* %表示该缓冲区位于当前窗口中
* a表示该缓冲区处于活动状态，即它已被加载并可见。
* “animal_farm.py”为文件名。
* line 30 表示当前光标位置

现在，用下列命令打开另一个文件

```
:e animals/cat.py
```

可看到，之前打开的文件已经被当前文件所取代。不过，animal_farm.py仍然存储在某个缓冲区中，可以再次用:ls命令将其显示出来。

怎样跳转到之前的文件呢？

Vim通过数字和名称来标识每个缓冲区，在同一个Vim会话中，他们都是唯一的（除非退出Vim)。为了在不同的缓冲区之间切换，可使用：b命令，其参数为缓冲区的编号数字。

```
:b 1
```

:b 1命令中的空格可以省略，得到简化版的命令:b1

缓冲区还可以用文件名来标识，因此读者可以用文件名的一部分来切换缓冲区。下列命令打开animals/cat.py的缓冲区。

```
:b cat
```

不过如果名称匹配了多个缓冲区，Vim就会报错。

如

```
:b py
```

Vim状态栏中会显示错误

```
E93: More than one match for py
```

为解决这个问题，可以使用Tab键补全文件名，从而实现在不同缓冲区之间循环切换。

也可以用:bn(:bnext)和：bp(:bprevious)命令循环遍历缓冲区。

当不再需要某缓冲区的时候（如不再需要编辑该文件），可以将其删除。通过如下命令可以将一个缓冲区从打开列表中删除，而无需退出Vim。

```
:bd
```

## 插件——unimpaired

该插件为很多内置命令添加映射。该插件可以在GitHub仓库tpope/vim-unimpaired中找到。

vim-unimpaired提供的部分映射。

| 命令 |      | 映射                                         |
| ---- | ---- | -------------------------------------------- |
| ]b   | [b   | 循环遍历缓冲区                               |
| ]f   | [f   | 循环遍历同一目录中的文件，并打开为当前缓冲区 |
| ]l   | [l   | 遍历位置列表                                 |
| ]q   | [q   | 遍历快速修复列表                             |
| ]t   | [t   | 遍历标签列表                                 |

此插件还支持用少数几次按键来切换某些选项，如==yos==切换拼写检查，==yoc==切换光标行高亮显示。

更多功能参加:help unimpaired 查看完整映射和功能清单。

### 窗口

Vim将缓冲区加载到窗口中。

1 窗口的创建、删除和跳转

打开animal_farm.py，用如下命令将窗口分割成两个，其中一个显示新文件。

 ```
 :split animals/cat.py
 ```

:split命令可简化为:sp

也可用下面命令按水平方向分割窗口。

```
:vsplit farm.py
```

:vs 是 :vsplit的简化版

为使光标能在不同窗口间移动，先按Ctrl+w，然后输入一个方向键：h、j、k、l中的一个或键盘方向键。

按Ctrl+w组合键，再按Ctrl+j组合键（Ctrl键可以不松开，记为Ctrl+w，j组合键），光标会进入下面的窗口，而Ctrl+w，k则进入上面的窗口。

如果经常使用窗口，可按照如下配置绑定快捷键。

```
"使用<Ctrl> + hjkl快速在窗口间跳转
noremap <c-h> <c-w><c-h>
noremap <c-j> <c-w><c-j>
noremap <c-k> <c-w><c-k>
noremap <c-l> <c-w><c-l>
```

然后，就可以用Ctrl+h组合键跳到左边的窗口，用Ctrl+j组合键跳到底部窗口，依此类推。

用下列方式，关闭窗口：

* Ctrl+w，q关闭当前窗口
* 使用:q命令关闭窗口并卸载缓冲区；不过，当只有一个窗口打开时，这会导致退出Vim。
* 使用:bd命令删除当前缓冲区，并关闭当前窗口。
* 使用Ctrl+w,o（或:only，或:on命令）关闭除当前窗口之外的所有窗口。

当打开了多个窗口时，可通过:qa关闭所有窗口并退出。也可以结合:w命令，即:wqa，它会先保存所有打开的文件，再退出Vim。

如果只想关闭缓冲区，而保留它所在的窗口，则可以在.vimrc文件中加入如下配置

```
"关闭缓冲区而不关闭窗口
command! Bd :bp | :sp | :bn | :bd
```

然后，就可以使用:Bd来关闭缓冲区，而保留分割窗口。

## 窗口的移动

并不需要记住所有这些命令，只要知道Vim支持哪些窗口操作，用时查文档。是用:help window-moving和:help window-resize

窗口命令的快捷键都要先按Ctrl+w，后面跟一个大写的方向键：

| Ctrl+w,H  | 当前窗口移动到屏幕最左边 |
| --------- | ------------------------ |
| Ctrl+w，J | 底部                     |
| Ctrl+w，K | 顶部                     |
| Ctrl+w，L | 最右边                   |

若想修改一个窗口的内容，跳转到这个窗口用：b来选择所需的缓冲区。

也有一些快捷键可以用于交换两个窗口的内容。

* 使用Ctrl+w,r将当前行或当前列（行优先于列）中的每个窗口的内容向右或向下移动。使用Ctrl+w，R以相反方向执行类似操作。
* 使用Ctrl+w，x将当前窗口与下一个窗口的内容交换（如果当前窗口是最后一个，则与前一个交换）。

Vim内部用数字来标识窗口。不过，与缓冲区不同，窗口的编号是随着布局变化而改变的，而且并没有直接的方法来修改窗口编号。有一条原则：窗口编号顺序为由上至下、由左至右递增。

### 改变窗口的大小

Vim窗口默认宽高比为50/50

Ctrl+w,=能够将所有打开窗口的宽和高调整为一致。

：resize会增加或减少当前窗口的高度，而：vertical resize将调整窗口的宽度

* :resize +N将当前窗口的高度增加N行
* :resize -N将当前窗口的高度减少N行
* :vertical resize +N 将当前窗口的宽度增加N列。
* :vertical resize -N 将当前窗口的宽度减少N列。

:resize和:vertical resize可分别简写为：res和:vert res

将窗口高度和宽度改变一行/列的快捷键：Ctrl+w,-和Ctrl+w,+用于调整高度，Ctrl+w,>和Ctrl+w,<用于调整宽度。

两种命令都可以将宽度/高度设置为具体的行数/列数。

* :resize N 用于将窗口高度设置为N
* :vertical resize N 用于将窗口宽度设置为N

## 标签页

如果希望在不同项目或同一项目的不同上下文之间切换，标签页是个不错的选择。

用户愿意使用标签页的另一个原因可能与Vim的diff功能有关，因为diff作用于一个标签页内。

在一个新标签页中打开一个空缓冲区的命令如下：

：tabnew

在新标签页打开一个已有文件的命令为:tabnew<文件名>

 在一个标签页中，可以通过常用的方式（:e<文件名>）来加载文件，也可以用:b命令在不同缓冲区之间切换。

为了在不同标签页之间跳转，可以使用如下命令：

* 快捷键gt或:tabnext命令用于切换到下一个标签页。
* 快捷键gT或:tabprevious命令用于切换到上一个标签页。
* 标签页可通过:tabclose命令来关闭，标签页关闭也会导致其中的窗口关闭（如果只剩一个标签页，则需要用:q来关闭）

:tabmove N 命令将当前标签页移动到第N个标签页之后（如果N为0，则变成第一个标签页）。

### 折叠

Vim为大型文件提供的一个强大工具是折叠。折叠功能支持文件部分内容隐藏，隐藏的依据可以是预定义的规则，也可以是手动添加的折叠标记。

1 折叠Python代码

需要在.vimrc文件中将foldmethod设置为indent。

```
set foldmethod=indent
```

不要忘记重新加载~/.vimrc，方法是重启Vim或在Vim中执行：`source $MYVIMRC`命令。

设置foldmethod为indent，使Vim基于缩进来折叠代码。

将光标移动到其中一个折叠行上，输入zo可以代开当前折叠。

只要光标在一个潜在的折叠文本中，输入zc都会将此折叠关闭。

为了方便看清折叠的位置，可以使用:set foldcolumn=N命令，其中N的取值为0~12。这会告诉Vim用从屏幕左边开始的N列来标记折叠，其中符号-表示一个打开的折叠，符号|表示打开的折叠的内容，符号+表示关闭的折叠。

输入za可切换折叠状态（打开关闭的折叠或关闭打开的折叠）。输入zR和zM分别用于同时打开和关闭所有折叠。

将foldmethod设置为自动类型（如indent)会默认所有文件折叠。

也可以选择在打开新文件时打开折叠。在.vimrc文件中添加`autocmd BufRead * normal zR` 会在打开新文件时令折叠处于打开状态，即Vim在读取新的缓冲区时执行zR命令（打开所有折叠）。

2 折叠的类型

折叠的方法由.vimrc文件中的foldmethod选项来指定。Vim支持如下折叠方式。

* manual：手动折叠，这种方法对于长文本而言并不适用
* indent：基于缩进的折叠，这对于依赖缩进的编程语言非常合适（不管哪种语言，标准的编码风格中总是会采用某种一致性的缩进。因此，当想要快速隐藏不关心的代码时，indent折叠方式不失为一种高效的选择）。
* expr：基于正则表达式的折叠。如果想用复杂的规则来定义折叠，那么可以选择这种方式。
* marker：使用文本中特殊的标记来定义折叠，比如{{{和}}}。这种方法对于管理很长的.vimrc文件非常有效，但是在Vim之外不常用，因为这种方式需要修改文件内容。
* syntax提供了可识别语法的折叠，但它并非对所有语言都开箱即用（不支持Python）。
* diff：当Vim处于diff模式时会自动采用这种折叠方式，diff模式下需要展示两个文件的不同之处，而相同之处往往需要隐藏起来

设置折叠方法为在.vimrc文件中加入set foldmethod=<折叠方法>

## 文件树的浏览

### 目录浏览器Netrw

Netrw是Vim的内置文件管理器（Vim自带的一个插件）。Netrw支持对目录和文件的浏览。

使用方法    ：Ex（完整命令为：Explore）

因为Netrw与Vim集成在一起，所以编辑一个目录时（如用：e命令打开当前目录），实际打开的是Netrw。

主要功能键：

* Enter键用于打开文件和目录
* -键进入上一层目录
* D键删除一个文件或目录
* R重命名一个文件或目录

Netrw的窗口也可以在分割窗口或标签页中打开。

* ：Vex以左右分割方式打开Netrw。
* ：Sex以上下分割方式打开Netrw。
* ：Lex以左右分割方式打开Netrw，当前Netrw窗口位于最左边，且高度占满整个屏幕。

Netrw非常强大，它还支持远程编辑。比如，通过下列命令可以列出SFTP下远程目录的内容。

```
:Ex sftp://<domain>/<directory>/
```

这里的：Ex命令也可以换成：e，想过相同。当然，：e也可以打开SCP下的远程文件。

```
:e scp://<domain>/<directory>/<file>
```

### 支持文件菜单的：e命令

另一种浏览文件树的方式为在.vimrc文件中设置set wildmenu。此选项将在增强模式下产生一个自动补全的文件名菜单（wildmenu)，并在状态栏中快速按Enter键呈现所有可能的自动补全选项。

启用wildmenu后，输入：e命令（不按 Enter键，输入一个空格），然后按Tab键。状态栏中会显示一个文件列表，可以用Tab键遍历选择这些文件，或用Shift+Tab反向遍历（左方向键和右方向键可起到同样作用）。选择完成后按Enter键，就打开相应的文件或目录了。按向下方向键将进入光标选中的目录，而向上方向键则进入上一级目录。

wildmenu支持部分路径，输入:e <文件名开始的字符>，然后按Tab键，会触发wildmenu。

.vimrc中常用的wildmenu设置如下：

```
set wildmenu   ”启用增强的Tab自动补全

set wildmode=list:longest,full   "补全为允许的最长字符串，然后打开wildmenu
```

上述设置允许在第一次按Tab键时将部分路径补全为最长的匹配字符串（并且还会展示所有可能项）；第二次按Tab键时遍历wildmenu中的文件。

### 插件——NERDTree

NERDTree模拟IDE行为，在屏幕边缘用一个分割的缓冲区来展示文件树。NERDTree可从GitHub仓库scrooloose/nerdtree中找到。

安装NERDTree之后可通过:NERDTree命令启动NERDTree。

使用h、j、k和l键或方向键可以浏览文件树结构，按Enter键或o键可打开选中的文件。还有一些其他有用的快捷键，可以用Shift+？组合键查看。

NERDTreee支持书签。可通过:Bookmark命令来收藏当前光标在NERDTree中选择的目录。在NERDTree窗口中，按B键可以在窗口顶部列出所有书签。

可以在NERDTree窗口中使书签保持显示，方法是在.vimrc文件中设置NERDTreeShowBookmarks选项，如下：

`let NERDTreeShowBookmarks = 1 “启动NERDTree时显示书签`

隐藏NERDTree窗口的命令：NERDTreeToggle

如果希望在编辑时令NERDTree窗口保持打开，可以在.vimrc中加入如下设置。

```
autocmd VimEnter * NERDTree  "Vim启动时打开NERDTree
```

当NERDTree窗口变成最后一个窗口时并不会自动关闭，可通过.vimrc文件中的如下设置来改变这种行为。

```
"当NERDTree窗口是最后一个窗口时自动关闭
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree")&&
\ b:NERDTree.isTabTree()) | q | endif
```

NERDTree优点是可以在工作区显示项目文件概览，很多人习惯使用图形化用户界面，因此会喜欢NERDTree。但当用户的编辑习惯被Vim改变时，可能会觉得文件列表容易让人分心，转而又会返回到更简洁的Netrw。

### 插件——Vinegar

它解决项目侧边栏与分割窗口无法同时工作的问题。

若已打开三个窗口，第四个NERDTree窗口在最左边。如果在NERDTree窗口中按Enter键，则光标选中的文件将出现在之前三个窗口中哪一个里呢？答案是NERDTree总是在最后一个创建的窗口中打开文件。

Vinegar插件使Netrw拥有无缝集成的用户体验。插件在GitHub仓库tpope/vim-vinegar中找到。

如果同时安装了NERDTree和Vinegar，则Vinegar会使用NERDTree窗口，而不是Netrw。为了避免NERDTree取代Netrw（而且使-键之类的命令保持可用），需要在.vimrc文件中设置：

```
let NERDTreeHijackNetrw = 0
```

Vinegar提供了一种新的快捷键映射：-键，用于在当前目录打开Netrw。

此插件隐藏了Netrw顶部的帮助栏。按I键，帮助栏又会出现。

另一个快捷键是Shift + ~组合键，它将打开用户主目录，很多人习惯在主目录保存项目。

### 插件——CtrlP

它是个模糊补全插件，可以在只知道部分关键字时快速打开所需文件。可在GitHub仓库ctrlpvim/ctrlp.vim中找到。

安装后，在Vim中按下Ctrl + p ，屏幕下方会出现CtrlP窗口，其中列出了项目目录中的文件列表。然后，输入文件名或目录中的关键字，列表中将只剩下匹配项。可以用Ctrl+j和Ctrl+k来上下遍历列表，并用Enter键来打开选中的文件。按Esc退出CtrlP窗口

除当前目录下的文件外，CtrlP还支持在缓冲区或最近使用的文件之间切换。在打开CtrlP窗口的情况下，使用Ctrl + f 和 Ctrl + b 可以在3种不同的搜索模式之间循环切换。

可以用命令来选择一种搜索模式，:CtrlPBuffer命令会列出缓冲区供选择。CtrlPMRU会列出最近使用的文件。：CtrlPMixed同时显示文件、缓冲区和最近使用的文件。

也可以在.vimrc中绑定其他快捷键。如：将：CtrlPBuffer命令绑定到Ctrl+b组合键，可以使用如下设置：

```
nnoremap <C-b> :CtrlPBuffer<cr> "将CtrlP缓冲区模式绑定到Ctrl+b"
```

### 文本浏览

当前行内操作：

* h表示向左，l表示向右
* t（取自until中的t）后面接一个字符，用于在当前行内搜索该字符，并将光标置于此字符之前；T则用于反向搜索。
* f（取自find中的f）。。。。。置于此字符之上；F反向搜索。
* _将光标放到行首，$将光标置于行尾。

> 狭义单词（word）由数字、字母和下划线组成。广义单词（WORD）表示由除空白字符（如空格、制表符或换行符）之外的所有字符构成的字符串。

几种自由跳转方式：

* j和l分别表示上下移动光标
* w将光标移动到下一个单词的开头（W用于广义单词）
* b将光标移动到上一个单词的开头（B...）
* e将光标移动到下一个单词的结尾（E...）
* ge将光标移动到上一个单词的结尾（gE...）
* Shift + { 和 Shift + } 分别将光标移动到段落开头和结尾。
* Shift + （和 Shift + ）分别将光标移动到句子的开头和结尾。
* H将跳转到当前窗口的顶部，而L将跳转到当前窗口的底部。
* Ctrl+f（或PageDown键）在当前缓冲区中向下翻页，而Ctrl+b（或PageUp键）为上翻页。
* /接一个字符串，用于在文档中搜索该字符串，而？（Shift+/）用于反向搜索。
* gg用于跳转到文件开头。
* G用于跳转到文件结尾。

还可以按行号移动光标。为了显示行号，运行:set nu命令，或在.vimrc中添加:set number。

* ：N跳转到第N行，其中N为绝对行号。

还可以选择在Vim中打开一个文件时立刻将光标置于特定的行。如：vim animal_farm.py +14 打开并跳转到第14行。

Vim支持相对移动。:+N用于向下移动N行，:-N反之。Vim还可以在左侧侧边栏显示当前行的相对行号，设置命令为：

```
set relativenumber
```

### 切换到插入模式

使用i进入插入模式，出入文本的位置为光标的位置。

其他插入模式的便捷方法：

* a键 在光标后进入插入模式
* A键 在当前行行尾进入插入模式（等价与$a）
* I键用于在当前行行首（在缩进之后）进入插入模式（等价于_i）
* o键 光标下面新增一行，在新的一行里进入插入模式。
* O键 在光标上面新增一行，在新的一行里进入插入模式。
* gi 在最后退出的位置进入插入模式。

通过修改命令c可以删除文本后进入插入模式，还有一些其他的先修改再插入的组合命令：

* c用于删除光标右边的文字（直到行尾），然后进入插入模式
* cc或S（大写）用于删除当前行的内容，然后进入插入模式，但会保留缩进。
* s用于删除单个字符（如果前面加了数字，则会删除多个字符），然后进入插入模式。

### 用/和？搜索

一般来说，Vim中最快的文本浏览方式是搜索指定的字符串，方法是按/键（从正常模式进入命令模式）然后输入待搜索的字符串。按Enter键之后，光标将移动到第一个匹配的地方。

按n键可以在同一个缓冲区中遍历所有匹配的位置。N反向遍历。

搜索过程中常用的一个选项是set hlsearch（可以考虑添加到.vimrc文件），因为这个选项会在屏幕上高亮显示每个匹配项。

可用==:noh==命令清除高亮

还有个技巧是使用set_incsearch。这个选项会在还未完整输入搜索命令时，就将光标动态跳转到第一个匹配处。

如果想反向搜索，则将/换成？即可。

1 跨文件搜索

vim提供了两个命令来实现跨文件搜索，分别是：grep和：vimgrep。

* ：grep调用的是系统命令grep，这是非常强大的工具，特别是对于熟悉grep用法的人而言
* ：vimgrep属于Vim的一部分，如果不熟悉grep，则：vimgrep命令会更容易掌握。

：vimgrep语法为:vimgrep<模式><路径>。

其中<模式>既可以是一个字符串，也可以是Vim风格的正则表达式。

<路径>通常为一个通配符，当<路径>为\**时，表示表示对目录递归搜索（或者用\*\*/\*.py搜索具体的文件类型）。

例如，使用下面的命令在Python工程中搜索animal字符串

```
：vimgrep animal **/*.py
```

然后，Vim会跳转到第一个匹配处，并在屏幕底部显示匹配个数。

然后可以用:cn或：cp来逐个浏览各匹配项。

也可执行==:copen==命令，底部会出现一个新窗口，其中与可视化显示的快速恢复（quickfix）列表

在快速恢复列表中，可用j和k来上下移动光标，然后按Enter键跳转到相应的匹配处。

2 ack

在Linux操作系统中，Vim可以结合ack工具来搜索代码库。ack继承了grep的思想，它更关注于搜索代码。可以用自己喜欢的包管理器来安装ack，下面是使用apt-get的安装示例：

```
$ sudo apt-get install ack-grep
```

访问Beyond grep网站，可以了解到ack的更多介绍及其安装方法。

比如，可以在命令行中用ack来递归搜索所有包含单词Animal的Python文件，命令如下：

```
$ ack --python Animal
```

ack的运行结果类似于grep。

Vim中包含一个集成ack的插件，可以将ack的结果显示在快速恢复窗口中。

这个插件的地址为GitHub仓库mileszs/ack.vim。

安装此插件后，可在Vim中执行如下:ack命令。

```
:ack --python Animal
```

此命令会运行ack，然后显示快速恢复窗口。

### 利用文本对象

文本对象是Vim中额外增加的对象类型，比如括号内或引号内的文本都可以称作文本对象。

通过文本对象，可以更方便地处理代码。

文本对象智能与其他操作符组合起来使用，比如修改命令、删除命令或可视模式。

```
di） "删除括号内的文本
```

```
c2aw（修改两个单词）"会删除光标后的两个单词（包括周围的空格），然后进入插入模式
```

文本对象有两种风格：内对象（以i开头）和外对象（以a开头）。内对象不包含空白字符（或周围其他字符），而外对象是包含的。

文本对象的完整列表可通过`:help text-objects`命令来查看，这里列出一些有趣的文本对象。

* w和W分别表示狭义单词word和广义单词WORD。
* s表示句子
* p表示段落
* t表示HTML/XML标签

编程语言中常用的成对的字符全部可以表示为文本对象，包括\\'、"、""、()、[]、<>和{}，因而可以选择这些字符之间的文本。

| 动词      | 数目（可选） | 形容词    | 名词       |
| --------- | ------------ | --------- | ---------- |
| d（删除） | ——           | i（内部） | ）（括号） |
| c（修改） | 2            | a（外部） | w（单词）  |

### 插件——EasyMotion

该插件简化了文本浏览过程，可以准确且快速地跳转到指定位置。插件的网址为GitHub仓库easymotion/vim-easymotion。

安装完成之后，连续按两次先导键\，然后按某个移动键，可以启动此插件。

> Vim插件常用先导键来实现额外的快捷键映射。默认情况下，Vim的先导键为\

\\\w命令将触发逐个单词的移动方式。每个单词的开头都被替换为一个字母（或两个字母，EasyMotion命令用完了多有英文字母之后就会使用两个字母）。按一个字母（或依次两个字母），光标将跳转到指定位置。

EasyMotion默认支持下列移动命令（所有命令都要加上两个先导符）

* f 向右查找一个字符，F反之
* t 向右移动直到找到该字符，T反之
* w 向后跳过一个狭义单词（W广义）
* b 向前跳过一个狭义单词（B广义）
* e 向前跳到狭义单词末尾（E广义）
* ge 向后跳到狭义单词的末尾（gE广义）
* k和j 分别跳到上一行或下一行的开关
* n和N 分别跳到上一个或下一个搜索结果（在用/或？搜索之后）

EasyMotion还保留了很多快捷键没有使用，因此读者可以构建自己的快捷键映射。

`:help easymotion`命令可查看EasyMotion的所有功能。

### 使用寄存器进行复制和粘贴

用y命令（yank）来复制文本，后面接一个移动命令或一个文本对象。

选择一些文本后也可以在可视模式下使用y来复制。

（这里的复制和粘贴只限于Vim内部，复制是将文本存入相应的Vim寄存器中，而粘贴是从某个Vim寄存器中取出文字并插入当前光标位置。如果想要和系统的粘贴板进行交互，需要使用特殊的寄存器。）

除所有的标准移动命令之外，也可以使用yy命令来复制当前行。

ye 复制文本直到下一个单词结尾

p 粘贴

删除和修改操作同时也复制了相关的文本，这些文本可以用于后续的粘贴操作中。另外，

粘贴操作p的前面可以加数字，从而实现多次粘贴。

### 寄存器

在Vim中复制和粘贴文本时，文本是储存在Vim寄存器里面的。Vim支持多种寄存器，每个寄存器用字母、数字或特殊符号来标识。

寄存器的访问方式是引号键"，后面接寄存器的标识符，然后是针对指定寄存器的操作。

a~z所标识的寄存器用于手动复制数据。如，将一个单词复制到a寄存器，可用`"ayw`命令，而粘贴

命令为`"ap`

寄存器还可用于录制宏

前面介绍的复制和粘贴操作使用的都是默认的无名寄存器。这个寄存器用双引号“来标识，

可以用这个标识符显式访问寄存器。如""p用于从无名寄存器中粘贴文本，等同于p。

用数字编号的寄存器存储的是最后10次删除操作的历史记录。0寄存器存储的是最后一次删除的文本，1为上上次，类推。“7p命令会将7次粘贴操作之前的粘贴操作 再次粘贴出来。

还有一些很有用的只读寄存器：%存储了当前文件名，#存储了上次打开的文件名，.中为最后插入的

文本，：为最后执行的命令。

也可在正常模式之外与缓冲区进行交互。Ctrl+r组合键允许在插入模式或命令模式下粘贴某个寄存器的内容。

如，在插入模式下，==Ctrl+r，"==会在光标处粘贴无名寄存器中的内容。

可在任何时候通过：reg <寄存器标识符>命令来访问某个寄存器的内容。

如 :reg a b可同时得到a和b寄存器的内容。

还可以用：reg 命令列出所有寄存器的内容。

y命令会重写寄存器内的内容，而字母命令的寄存器（a~z）可以附加内容，方法是使用大写的寄存器标识符。如，想附加一个单词到寄存器a中，可先将光标置于此单词开头，然后执行`"Ayw`命令

### 从外部复制文本到Vim中

Vim中有如下两种内置的寄存器用于和外部世界交互。

* \*寄存器表示系统的主粘贴板（macOS和Windows系统中的默认粘贴板，在Linux系统中为终端的鼠标选择内容）
* \+寄存器（只针对Linux）用于Windows风格的Ctrl+c组合键和Ctrl+v组合键操作[称为粘贴板选择器（Clipboard selecton）]

`"*p` 从系统主粘贴板中将文本粘贴进来

`"+yy`将一行文本复制到Linux粘贴板选择器中。

如果希望默认使用这些寄存器，可以在.vimrc文件中设置clipboard变量，将其设置为unamed时，表示默认使用*寄存器进行复制和粘贴。

```
set clipboard=unamed    "复制到系统寄存器（*）
```

将clipboard设置为unnamedplus，将默认使用+寄存器。

```
set clipboard=unnamedplus "复制到系统寄存器（+）
```

利用如下命令可以同时使用这两个寄存器。

```
set clipboard=unamed,unnamedplus "复制到系统寄存器（*，+）
```

在.vimrc文件中完成设置后，y和p命令将分别从指定的默认寄存器中复制和粘贴。

> 当在插入模式下从系统粘贴板中将文字粘贴到缓冲区中。在老版本Vim或某些终端模拟器中，这样粘贴会出问题，因为Vim会在粘贴过程中自动缩进代码或添加注释。为避免这种情况，在粘贴代码之前先禁用缩进和自动注释，方法是运行`:set paste`命令，粘贴完成后，恢复的命令为`:set nopaste`。从8.0版本开始，Vim默认启动括号化粘贴模式（bracketed paste mode），从而基本解决了这些问题

# 使用先导键——插件管理

### vim-plug

GitHub仓库地址为junegunn/vim-plug

vim-plug优点：

* 轻量级，单个文件且支持一些直观的安装选项。
* 支持并行插件加载（要求Vim编译带有Python或Ruby支持，这几乎已经是现代Vim的标配）
* 支持大多数插件的延迟加载，即只为特定命令或文件类型触发必要的插件。

> 之前介绍了手动安装插件。这里提供一种更佳的插件安装体验，用过后大概会选择把之前手动安装的插件都删除。
>
> Linux中删除命令为`rm -rf $HOME/.vim/pack`（Cygwin同），Windows系统中为`rmdir /s %USERPROFILE%\vimfiles\pack

安装vim-plug的方式非常简单。

* 下载插件文件
* 保存为`$HOME/.vim/autoload/plug.vim

1 为了从GitHub上下载文件，在Linux或macOS系统中可使用curl或wget命令，或直接在浏览器中打开链接，然后“右键->另存为"。比如。比如UNIX用户可以用下列命令下载文件。

```
$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs
https://raw.github.com/junegunn/vim-plug/master/plug.vim
```

2 修改.vimrc文件，加入vim-plug初始化的代码，如下：

```
"使用vim-plug管理插件
call plug#begin()
call plug#end()
```

3 在这两行之间加入一些插件，其中的地址格式为GitHub地址的最后两部分(<用户名>/<仓库>,比如

https://github.com/scrooloose/nerdtree记为scrooloose/nerdtree)，用于唯一标识插件，如下：

```
"使用vim-plug管理插件
call plug#begin()

Plug 'scrooloose/nerdtree'
Plug 'tpope/vim-vinegar'
Plug 'ctrlpvim/ctrlp.vim'
Plug 'mileszs/ack.vim'
Plug 'easymotion/vim-easymotion'

call plug#end()
```

4 保存.vimrc文件，然后重载（命令为:w | source $MYVIMRC）（意思是先输入：w 回车，再输入:source...）或重启Vim，以使这些修改生效。执行:PlugInstall来安装这些插件。然后上面提到的插件将会自动从GitHub上下载下来。

vim-plug有两个主要的命令

* :PlugUpdate用于更新所有已安装的插件
* :PlugClean用于删除.vimrc中已经移除的插件。如果不执行:PlugClean，则没有激活的插件（.vimrc中删除或注释掉的那些Plug...行）将仍然保存在文件系统中。

运行:PlugUpdate将更新vim-plugin所管理的插件，但不包括它自己。如果想更新vim-plug，需要运行:PlugUpgrade命令，然后重载.vimrc文件（执行:source $MYVIMRC 或 重启 Vim）

延迟加载是一种避免插件拖延Vim运行速度的有效技术，这一点可通过Plug指令的可选参数来实现。比如，如果想要在:NERDTreeToggle命令执行时再加载NERDTree，可以使用on参数，示例如下

```
Plug 'scrooloose/nerdtree',{'on':'NERDTreeToggle'}
```

如果只想对特定文件类型加载某个插件，可以使用for参数，如下所示。

```
Plug 'junegunn/goyo.vim',{'for':'markdown'}
```

由于vim-plug采用单文件安装方式，因此它的帮助文档并未安装到Vim中，如果想要用:help vim-plug来查看文档，则需要将Plug 'junegunn/vim-plug'添加到插件安装列表中，然后运行：PlugInstall命令。

在vim-plug的GitHub仓库junegunn/vim-plug中，读者可以在它的README文件中找到vim-plug支持的所有参数列表。

对于Linux或macOS系统用户（以及Cygwin用户），可以将下列代码添加到.vimrc文件中，当该.vimrc被移植到新机器上时，它会自动安装vim-plug

```
"如果没安装过vim-plug,则下载安装
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
     \https://raw.GitHub.com/junegunn/vim-plug/master/plug.vim
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif
```

再次打开Vim时，vim-plug（以及所有用Plug指令列出的插件）就安装好了。

如果想同时支持Windows和UNIX系统的配置，代码会稍长一些。

参见： https://www.rosipov.com/blog/cross-platform-vim-plug-setup

### 荣誉推荐

#### 1 Vundle

Vundle是vim-plug的前身，它们工作方式相似，比如，插件安装是同步的（不是后台运行），

只不过Vundle的插件打包方式更重量级一些。

另一个不同在于插件的搜索方式，Vundle支持直接从Vim中搜索插件，还支持用户在安装插件之前

先进行测试。

Vundle及其安装指令可以在它的GitHub仓库VundleVim/Vundle.vim中找到。

Vundle和vim-plug工作法方式类似：PlugInstall、:PluginUpdate、:PluginClean这几个命令作用相同。

Vundle支持用:PluginSearch<字符串>命令来搜索插件，这也许是Vundle最吸引人的功能。如，用下列命令

搜索注释管理插件。

```
:PluginSearch comment
```

然后得到一个匹配列表，把光标移动到一个名为tComment的小插件上，它提供了一些与注释相关的快捷键。

然后按i键（或执行:PluginInstall命令）安装插件。之后无需提交或重载该插件即可在Vim中使用。

tComment支持用gcc来对当前行注释或取消注释。

注意：上面的操作只是测试，只要将插件添加到.vimrc文件中，插件才能永久安装。

#### 2 手动方式

由于大部分插件都可以在GitHub上找到，因此使插件保持更新的主要方法是将它们安装成Git的子模块。

可以在.vim目录中初始化一个Git仓库，然后在此仓库中安装插件，作为Git仓库的子模块。

Vim8提供了一种原生的插件加载方式，即在.vim/pack目录中搜索插件目录树。Vim8支持如下目录结构。

* .vim/pack/<目录名>/opt/中保存的插件用于手动加载
* .vim/pack/<目录名>/start/中保存的插件用于始终加载

首先需要选择一个好记得名字，作为.vim/pack/的子目录。比如，plugins就是一个不错的选择。

然后，<目录名>/start/目录用于存储那些总是需要加载的插件。

opt/目录中的插件只会执行:packadd<目录名>命令之后再加载，因此可以在.vimrc中用packadd命令有选择性地加载插件。

opt/目录和packadd命令可以实现插件的延迟加载（与vim-plug相同）

```
"通过：Ack命令来加载和运行ack.vim插件
command ! -nargs=* Ack :packadd ack.vim | Ack <f-args>
"当打开Markdown文件时加载并运行Goyo插件
autcmd! filetype markdown packadd goyo.vim | Goyo
```

此外，在.vimrc中加入如下代码，可以加载所有插件的文档。

```
packloadall   "加载所有插件
silent! helptags ALL "为所有插件加载帮助文档
```

执行packloadall命令使Vim加载start/目录中的所有插件（Vim会在加载.vimrc之后自动执行这个步骤，这里是提前执行）。helptags ALL加载所有插件的帮助文档，而silent!前缀则是为了隐藏加载帮助文档过程中的所有输出和错误信息。

可以通过Git子模块自行管理插件（稍微有点麻烦），即用Git下载插件并保持更新。

首先，在.vim目录中初始化一个Git仓库（只需要操作一次）。

```
$ cd ~/.vim
$ git init
```

然后，添加一个插件，比如nerdtree，作为子模块。

```
$ git submodule add https://github.com/scrooloose/nerdtree.git
pack/plugins/start/nerdtree
$ git commit -am "Add NERDTree plugin"
```

每次更新插件时，执行下面命令。

```
$ git submodule update --recursive
$ git commit -am "Update plugins"
```

若要删除插件及相应的子模块，可执行下列命令。

```
$ git submodule deinit -f -- pack/plugins/start/nerdtree
$ rm -rf .git/modules/pack/plugins/start/nerdtree
$ git rm -f pack/plugins/start/nerdtree
```

#### 3 Pathogen

Pathogen更像是一种runtimepath管理器，而非插件管理器。但在实践中，rantimepath管理可以相当好地过渡为插件管理。在Vim8之后，已经不需要通过操作runtimepath来安装插件。如果你还在使用Vim 8 之前的版本，或者不愿意使用重量级的包管理器，则Pathogen可以作为简便的插件管理器来使用。

Pathogen是插件管理的先行者之一，并在很大程度影响了后来者。Pathogen可以在它的GitHub仓库tpope/vim-pathogen中找到

### 分析运行慢的插件

某些未优化的插件可能因插件缺陷或插件与系统之间独特的交互方式 导致Vim运行速度减慢。Vim有内置的插件分析功能，可用于检测这些问题。

#### 1 启动时间分析

Vim启动时，若加上--startuptime<文件名>选项，则Vim会将启动时的每个行位记录到一个文件中。比如，下列命令将启动日志记录到文件startuptime.log中。

```
$ vim --startuptime startuptime.log
```

> gVim也可以用类似的方式启动：`gVim --startuptime startuptime.log`这个命令行对于Windows和Linux都是一样的。

退出Vim，打开startuptime.log，可以看到一些时间戳（大部分用三列表示），时间戳单位是毫秒，第一列表示自Vim启动后的毫秒数，第三列表示每个行为占用的时长。第三列是我们最感兴趣的，可以从中发现异常。

当插件加载时间达到500ms（半秒）以上时，才会在Vim启动时明显感觉到慢。

#### 2 特定行为的分析

发现某个行为慢，可以对特定几个行为进行分析。

考虑一种典型场景：从GitHub下载Python和Vim仓库，然后尝试运行CtrlP插件提供的：CtrlP命令。该命令试图从当前目录开始递归为所有文件建立索引，对于文件数众多的这两个仓库而言，速度显然是非常慢的。

为了分析这个行为，在启动Vim后，执行：

```
:profile start profile.log
:profile func *
:profile file*
```

Vim将分析这里执行的每个行为。当一个命令运行速度较慢时，比如这里的:CtrlP命令（或按Ctrl+P组合键）。等待这个操作完成之后，退出Vim（：q）。

打开profile.log (折叠文件设置set foldmethod=indent)

按G跳转到文件结尾，可看到排好序的函数列表，排序依据为执行所需的时间。

看到的可能是大部分运行速度较慢的函数都以CtrlP#开头，显然CtrlP是拖延Vim运行速度的罪魁祸首。

如果看不出这些函数的来源，可以通过函数名来搜索它们源自哪些文件。比如，\<SNR>29_GlobPath()需要约3.9s，把光标置于此函数名上，按==*==键搜索光标所在的单词，可看出CtrlP引用这个函数的次数，从引用情况看，

这个函数很有可能与插件的延迟加载有关。

因此，profile.log可用于深入分析哪个插件最有可能导致Vim运行缓慢。

### 模式详解

Vim有7种模式

#### 正常模式

normal mode，打开Vim时默认为正常模式，在其他模式中按一次Ese键（有时候需要按两次），也会返回正常模式。

#### 命令行模式和ex模式

命令行模式（command-line mode）进入方式是输入冒号（：）（或者用/或？进行搜索），进入命令行模式之后，可以输入一条命令，然后按Enter键执行命令。命令行模式提供了以下几个常用的快捷键。

* 上下箭头键（或Ctrl+p组合键和Ctrl+n组合键）可以逐个命令地浏览历史记录。
* ==Ctrl+b组合键和Ctrl+e组合键可以跳转到命令的开头（beginning）和结尾（ending）==
* Shift键和Ctrl键结合左右箭头键可以逐个单词移动光标。

==Ctrl+f可以打开一个可编辑的命令行窗口，其中显示的是之前运行过的命令的历史记录。==

命令历史记录窗口只是普通的缓冲区，可以在其中找到曾经执行过的命令，并进一步编辑（使用Vim中正常的文本编辑方式），然后再次执行。按Enter键可以执行光标所在的命令行，按Ctrl+c组合键可以编辑此缓冲区。

查看帮助文档:help cmdline-editing可以了解到更多命令行模式的使用方法。

Vim还有一个命令行模式变体，称为ex模式，进入方式是按Q键。ex模式是为了兼容Vim前身Ex。ex模式支持执行多个命令而无须退出，但这个模式现在已经很少使用。

#### 插入模式

inset mode用于输入文本，除此无其他功能。此模式下可使用Ctrl+o组合键来执行正常模式下的命令，然后再回到插入模式。

#### 可视模式和选择模式

可视模式（visual mode）支持文本的任意选择。如果需要选择的文本不属于已定义的文本对象（单词、句子、段落等），则这个模式非常有用。进入可视模式有以下几种方式。

* v进入字符可视模式（状态栏标识文本 --VISUAL--）。
* V进入行可视模式（状态栏标识文本--VISUAL LINE--).
* Ctrl + v组合键进入块可视模式（状态栏标识文本--VISUAL BLOCK--）。

进入可视模式，可通过移动光标来改变选择范围。

可通过如下操作来控制可视模式下的文本选择。

* 按o键跳转到高亮选中的文本的另一端（因而支持在另一端控制选择的范围）。
* 对于块可视模式，按o键可跳转到当前行的另一端。

Vim还有一个**选择模式（select mode）**，输入任意可打印字符可立即删除选中文本，然后进入插入模式。这个模式和ex模式相同，只应用于特定和受限的场合。

正常模式下按gh键进入选择模式，也可在可视模式下按Ctrl+g组合键，退出方式为按Esc键。

### 替换模式和虚拟替换模式

在其他编辑器里，按Insert键进入替换模式，其效果是擦除其他文本。在Vim下，替换模式也类似，输入的文本会覆盖已有文本（而不像插入模式下，输入文本会移动已有文本。）当不想改变原始文件中的字符数时，这种模式非常有用。

替换模式的进入方式是按R键。在状态栏上的标识文本为--REPLACE--。

按r可进入单字符替换模式，在此模式下，替换单个字符后会马上切换回正常模式。

Vim还提供了虚拟替换模式，它和替换模式类似，只不过操作的对象是屏幕上的显示，而非直接针对文件中的字符。主要的区别包括Tab键会替换多个字符（而替换模式下只会替换单个字符）；按Enter键不会新增一行，但会移动到下一行。虚拟替换模式的进入方式是输入gR，详见帮助文档：help vreplace-mode。

### 终端模式

terminal mode 出现在Vim8.1版本中，此模式支持在一个分割窗口中运行一个终端环境。进入命令如下：

```
:terminal
```

:terminal命令可简化为:term

该命令会在水平分割窗口中打开系统的命令行系统（Linux系统中为默认的Shell，Windows系统中为cmd.exe）

终端模式封装了系统的终端，可像往常一样使用命令行。此窗口可用Ctrl+w命令进行切换，只不过此窗口中只有插入模式。可考虑使用Linux或macOS系统下的tmux命令或screen命令，与Vim的终端模式配合使用。

还可以使用：term执行单个命令，并把输入存放于一个缓冲区中。比如，可以用如下方式运行animal_farm.py。

```
:term python3 animal_farm.py cat dog
```

此命令将出现在一个水平分割的窗口中。

怎么关闭终端窗口？

```
Ctrl+w | Ctrl+c
```

### 命令的重映射

Vim可无穷扩展，几乎支持每一个操作的重映射。

Vim支持将某些键映射为其他键，:map和:noremap提供了这样的功能。

* :map用于递归映射。
* :noremap用于非递归映射。

这意味经:map重映射的命令可以识别自定义映射，而:noremap则针对系统默认映射。

> 当决定创建一个新映射时，最好先确认一下此键或按键序列是否已经被映射为其他用途了。内置的按键绑定列表可通过:help index命令查看。而:map命令则可以查看插件和自定义的映射。比如，:map g将显示所有以g键开头的映射。

如，在.vimrc中将按如下方式自定义映射。

```
noremap ; : "用分号取代默认的冒号，用于输入命令
```

这样每次切换命令模式的时候无须按Shift键了，不过这样也有坏处，那就是没有命令可用于重复最后的t、f、T或F操作（这4个命令用于查找字符）。

这里使用noremap表示希望用分号进入命令行模式，而无论冒号是否已经被重映射都不会影响这一点。

> 如果想显式移除自定义或插件定义的映射，可使用:unmap命令。还有一个重要选项是:mapclear，它会将自定义的映射和默认映射都清除掉。

还可以在映射中使用特殊字符和命令，比如下列代码：

```
"noremap <c-u> :w<cr> " 使用<Ctrl-u>来保存文件（u表示更新）
```

\<c-u>表示Ctrl+u组合键。Vim中的Ctrl修饰符表示为<c-\_>，其中_为某个字符。

* <a-\_>或<m-\_>表示Alt键加上某个键
* <s-\_>表示Shift加某键

命令后面接\<cr>表示回车，不按回车，命令不会被执行。

* \<space>表示空格
* \<esc>表示Esc键
* \<cr>和\<enter>表示Enter键
* \<tab>表示制表符Tab键
* bs表示退格键
* \<up>、\<down>、\<left>和\<right>表示箭头键
* \<pageup>、\<pagedown>表示上下翻页键。
* \<f1>~\<f12>表示12各功能键
* \<home>、\<insert>、\<del>和\<end>分别表示Home、Insert、Delete、和End键。

还可以将一个键映射为\<nop>（无操作no operation的缩写），表示这个键不起任何作用。有时候，希望习惯使用hjkl风格的光标移动方式，而禁止自己使用箭头键，这个映射会派上用场。可在.vimrc中加入如下配置。

```
"禁止箭头键功能，使自己习惯hjkl风格
"map <up> <nop>
"map <down> <nop>
"map <left> <nop>
"map <right> <nop>
```

#### 模式感知的重映射

:map命令和:noremap命令都可用于正常模式、可视模式、选择模式和操作待决模式（operator pending mode）。不过，Vim还支持更细粒度的映射控制方式，可针对每一种模式分别定义映射。

* :nmap和:nnoremap用于正常模式
* :vmap和:vnoremap用于可视和选择模式
* :xmap和:xnoremap用于可视模式
* :smap和:snoremap用于选择模式
* :omap和:onoremap用于操作待决模式。
* :map!和:noremap!用于插入和命令行模式
* :imap和:inoremap用于插入模式
* :cmap和:cnoremap用于命令模式

> Vim通常用感叹号强制执行命令，或为命令添加额外功能，详见帮助文档:help!

如果希望添加一些映射来改变插入模式的行为，可通过如下配置实现

```
"在插入模式下加入一对引号或括号
"inoremap ' ''<esc>i
"inoremap " ""<esc>i
"inoremap ( ()<esc>i
"inoremap { {}<esc>i
"inoremap [ []<esc>i
```

上面例子中，插入模式下的默认按键行为被改变。比如，按左方括号键，会插入一对方括号，然后进入插入模式，并将光标置于这两个括号之间。

### 先导键

leader key，它本质上是用户或插件定义的快捷方式的命名空间。先按先导键，然后按下的任何键都来自于该命名空间。

默认的先导键为反斜线\，但这个键用起来不太方便。在Vim社区中有几个替代方案，其中逗号较流行。为重新绑定先导键，需在.vimrc中加入：

```
"将先导键重新映射为逗号
"let mapleader = ','
```

先导键的设置应该放在.vimrc顶部，因为新定义的先导键只会在定义之后生效。

> 当重新绑定一个键时，该键 原来的默认行为就会被覆盖。比如，逗号本来用于反方向重复执行最后一次t、f、T或F操作。

推荐的先导键是空格键。这个键大，且正常模式下没有什么实际的用途（只不过在功能上模拟了右箭头键）

```
"将先导键映射为空格键
"let mapleader = "\<space>"
```

> 这里\<space>之前的转义符\是必要的，因为mapleader变量中不含特殊字符（如空格符）。对于这个转义来说，只能用双引号，不能用单引号，因为单引号中只能存字面量字符串，而不含转义字符。

然后，就可以在.vimrc中使用先导键\<leader>了,比如：

```
"用leader-w保存文件
"noremap <leader>w :w<cr>
```

有时，会希望用先导键配置插件功能，使插件快捷键更容易记忆，如：

```
"noremap <leader>n :NERDTreeToggle<cr>
```

### 插件的配置

使用一个插件时，查看它提供了哪些命令和选项是一个好习惯。可以将命令绑定到方便记忆的快捷键上。

Vim支持全局变量，其主要用途是配置插件。全局变量通常以g：为前缀。通过：help<插件名>命令查看插件文档，可以找到它提供了哪些配置选项。

如，打开CtrlP插件的帮助文档（运行:help ctrlp命令），并搜索optinons(运行/options命令)

打开ctrlp-options链接（按ctrl+]组合键）可以得到一个选项列表，进一步查看其中一个选项，比如ctrlp_working_path_mode。将光标移动到此链接上，按Ctrl+]组合键打开链接，

ctrlp_working_path_mode选项貌似用于控制CtrlP本地工作目录的设置方式。如果要寻找Git工程的根目录（找不到则返回当前工作目录），则可以将此选项设置为ra，即在.vimrc中加入如下设置：

```
"将CtrlP的工作目录设置为仓库根目录（找不到则回退为当前目录）
"let g:ctrlp_working_path_mode = 'ra'
```

例，CtlP的所有模式都可以仅用两个按键访问。

```
"用先导键重新映射CtrlP的行为
"noremap <leader>p :CtrlP<cr>
"noremap <leader>b :CtrlPBuffer<cr>
"noremap <leader>m :CtrlPMRU<cr>
```

***

***

#### 代码自动补全

代码自动补全是现代集成开发环境（IDE）的重要功能，可避拼写错误，且无须反复输入和记忆冗长复杂的变量名。

#### 内置自动补全

输入一个函数的开始几个字符，然后按Ctrl+n组合键，Vim会弹出一个自动补全列表。然后用Ctrl+n和Ctrl+p组合键可以在这个列表中上下移动。

事实上，Vim有一个**插入——补全模式**，先按Ctrl+x组合键，然后按下列快捷键之一。

* Ctrl+i：用于补全整行
* Ctrl+j：用于补全标签
* Ctrl+f：用于补全文件名
* s：基于拼写建议的补全（要求设置：set spell）

> 其他补全命令参见:help ins-completion，还可查:help 'complete',该选项用于控制Vim补全建议的来源（默认为缓冲区、标签文件和头文件）。

#### YouCompleteMe插件

该插件补全Vim内置自动补全引擎，还增添了大量新功能，下面是其优于内置自动补全的几个重要功能。

* 支持程序语言感知的自动语义补全；YouCompleteMe更能理解代码的语义。
* 补全建议的智能排序和过滤
* 能够显示代码文档、重命名变量、自动格式化代码以及修正某些类别的输入错误（这个功能与具体的语言有关，参见官方GitHub仓库Valloric/YouCompleteMe）

#### 1 安装

首先，确保安装了cmake和llvm，因为YouCompleteMe需要编译代码。

```
$ sudo apt-get install cmake llvm
```

> 对于Windows用户，可以分别下载cmake和llvm。YouCompleteMe要求Vim编译有+python支持，可通过vim --version | grep python来确认。如果看到了-python，则需要重新编译Vim，使它支持python。
>
> win10下载cmake的msi文件运行，llvm安装
>
> 

如果使用vim-plug，则可在.vimrc中加入如下设置。

```
let g:plug_timeout = 300 "为YouCompleteMe增加vim-plug的超时时间
Plug 'Valloric/YouCompleteMe', { 'do': './install.py'}
```

保存.vimrc，然后执行如下命令：

```
:source $MYVIMRC | PlugInstall
```

> 如果遇到错误，比如C++：internal compiler error: Killed (program cclplus)，则很可能是由于机器内存不够用，导致无法完成编译。在Linux中，可通过内存交换操作来增加可用内存空间。
>
> ```
> $ sudo dd if=/dev/zero of=/var/swap.img bs=1024k count=1000
> $ sudo mkswap /var/swap.img
> $ sudo swapon /var/swap.img
> ```

我遇到的具体错误是

```
YCM error. The ycmd server SHUT DOWN (restart wit…the instructions in the documentation
```

解决办法 cd .vim/plugged/YouCompleteMe ，然后执行 `python install.py`

#### 2 使用YouCompleteMe

进入插入模式，开始编辑。自动补全列表会自动弹出来。按Tab键会循环遍历所有建议。

此外，如果YouCompleteMe能够查找函数定义，并且找到的函数支持文档字符串，则会在窗口顶部显示一个预览窗口。

> 预览窗口只在YouCompleteMe使用语义自动补全引擎时显示。语义自动补全引擎可通过在插入模式下输入句点.来启动，或者通过Ctrl+空格组合键来手动启动

对于Python，YouCompleteMe还支持函数定义的跳转。在.vimrc中添加如下快捷键映射。

```
noremap <leader>] :YcmCompleter GoTo<cr>
```

现在只要把光标放在某个函数调用上，然后按住先导键（默认是反斜线\）和]，Vim就会将光标跳转到函数定义。

> YouCompleteMe并不是唯一的自动补全工具，其他的可以在网络上搜索Vim autocomplete来查找。

### 用标签浏览代码库

浏览代码库尝尝需要明确某个方法怎样定义，以及某个方法在哪出现。

Vim自带了在同一文件中浏览变量定义的功能。将光标置于一个单词上，输入gd可以跳转到该变量的定义处。

gd会优先查看局部变量声明，而gD会查看全局声明（从文件开头开始查找，而不仅限于当前作用域）

此功能并没有考虑语法，因为Vim本身并不了解代码的语义结构。不过Vim支持标签功能。标签文件中可存储程序代码的语义和结构关键词，供Vim检索。比如Python常用标签关键词包括类、函数和方法。

#### 1 Exuberant Ctags

用于生成标签文件的外部工具。

> 如果你使用的是Debian系的Linux发行版，可以用命令`sudo apt-get install ctags`或`sudo apt install ctags`来安装Ctags。

Ctags提供了命令行工具ctags，它可以为代码库生成一个标签文件。在命令行中进入一个工程，执行如下命令。

```
$ ctags -R
```

当前目录下会出现一个tags文件。

> 可以在.vimrc中加入如下选项。
>
> ```
> set tags=tags; "在父目录中递归查找tags文件
> ```
>
> 此选项会使Vim在父目录中递归查找tags文件，从而确保整个工程使用同一个标签文件。分号；作用是使Vim持续查找，直到找到一个tags文件。

在Vim中打开animal_farm.py，将光标置于某个语义关键字上，比如第26行的add_animal方法，按Ctrl+]组合键，光标会根据标签跳转到函数的定义（函数的定义位于不同的文件中）。

使用Ctrl+t组合键会跳转返回（使光标返回到之前所在的文件的相应位置）。

> Ctrl+o和Ctrl+i组合键也能实现跳转，但是它们使用的是不同的关键字列表

如果出现同名的标签关键字，则可以用命令:tn（下一个标签，next tag）和:tp（上一个标签，previous tag）在不同选项之间切换。

也可以用命令:ts（表示选择标签，tag select）触发一个标签列表菜单。比如，用Ctrl+]组合键从farm.py文件中的animal.get_kind()调用跳转到get_kind的定义，执行:ts看看效果。

还可以用g]打开一个标签关键字选择窗口来进一步选择，而不会马上跳转。

在打开Vim时可以立即跳转到一个标签位置上。在命令提示符中，执行如下命令行。

```
$ vim -t get_kind
```

Vim会直接跳转到标签关键字get_kind定义处。

#### 2 自动更新标签

用户一般不愿意在每次修改代码后手动执行ctags -R命令，解决这个问题的一种简单方式是在.vimrc种加入如下配置。

```
"保存Python文件时重新生成标签文件
autocmd BufWritePost *.py silent! !ctags -R &
```

上述配置会在每次保存Pyhton文件时运行ctags -R命令。

> 可以将*.py后缀修改为其他文件类型，视工作场景而定。比如，下面的配置针对C++文件。
>
> ```
> autocmd BufWritePost *.cpp *.h silent! !ctags -R &
> ```

### 撤销树和Gundo 貌似只能用python2编译？

大部分现代编辑器都支持撤销栈，其中保存了撤销和重做操作的历史记录。Vim则更进一步，采用了撤销树。如果执行修改操作X，然后撤销并执行另一修改操作Y，X却仍然被保存起来。Vim支持手动浏览撤销树的叶子结点，但还有更好的方式。

Gundo是一种对撤销树进行可视化的插件，可以在GitHub上找到。

> 如果使用vim-plug管理插件，在.vimrc中添加Plug
>
> 'sjl/gundo.vim'。执行:w | so $MYVIMRC | PlugInstall，即可成功安装Gundo。

假设正在用Vim打开animal_farm.py文件，光标位于第15行上，编辑高亮显示的当前行if kind == 'cat'，执行如下操作。

* 将cat换成leopard
* 用撤销命令u撤销前一操作。
* 将cat换成lion

正常情况下，可能会以为cat和leopard都已经丢失，但是因为Vim拥有撤销树，因此所有的修改内容都保留在撤销树中。

执行:GundoToggle将以分割方式打开两个新窗口：左上窗口为树状结构的可视化表达，光标选中了其中一个版本；左下窗口则显示此版本和上一版本之间的差异。

按gg可使光标跳转到缓冲区顶部，可看到，Gundo左上窗口最上面，最后一次修改中，包含cat的行被换成了包含lion的行（以减号-开头的行表示被删除的行，以加号+开头的行表示添加的行）

按j键，光标将在树状结构中向下移动，到达一个未被使用到的分支，你可能会认为这个分支上的操作已经丢失了。可以看到，该操作将cat换成了leopard。按Enter键，Gundo将恢复到这次编辑的版本上。

再次运行:GundoToggle，撤销树窗口将会隐藏。

> 如果经常使用Gundo树状结构，可以将它绑定到某个快捷键上。比如下面的配置将打开Gundo的命令绑定到了F5上。
>
> ```
> noremap <f5> :GundoToggle<cr> "将Gundo映射到<F5>
> ```
>
> 关于撤销树的更多详情，参考:help undo-tree

### 构建、测试和执行

这部分涉及对.vimrc的多次修改，可从GitHub仓库PacktPublishing/Mastering-Vim 中找到相关的设置代码。也可找到Git的安装和配置方法。

#### 版本控制和Git

Git可用于追踪文件修改历史记录，并缓解多个用户同时工作于同一文件时的痛苦。Git是一个分布式版本控制系统，这意味每个开发者在自己系统里都拥有代码库的一个完整副本。

如果使用的是一个Debian系的Linux发行版，可以通过如下命令安装Git。

```
$ sudo apt-get install git
```

其他系统，可在Git官网找到安装包下载地址和安装方法。

在使用前，需要配置用户名和邮件地址。

```
$ git config --global user.name 'Your Name'
$ git config --global user.email 'your@email'
```

查看Git文档的命令如下

```
$ git help
```

#### 1 概念

Git用一系列文件的原子操作来表示文件的修改历史，这些原子操作在Git中称为提交(commit)。提交功能除了可以保存文件的变化，还包含额外添加的描述性消息，这样就可以找到任意时间点的文件变动。

Git中提交的历史并不一定是线性的，也可能是具有多个分支的图状结构，因此多个Git用户就可以独立地修改代码中的不同部分，而不用担心互相影响。

##### 2 新建工程

本例采用官方仓库Chapter05/animal_farm中的代码。创建好一个项目后，可按照下列步骤设置一个Git仓库。

​	1 初始化Git仓库

```
$ cd animal_farm/
$ git init
```

一般默认用master作为初始分支的名称。想修改默认分支名称可以输入：

```
git config --global init.defaultBranch <name>
```

​	一般，master可替换成main 或者trunk 或者development，想修改刚创建的分支名称，可以输入：

```
git branch -m <name>
```

 2 将一个目录中所有文件添加到暂存区（stage），为初始提交做准备。

```
$ git add
```

​	3 创建初始提交

```
$ git commit -m "初始提交"
```

现在，一个Git仓库就创建完成了。

如果想将此仓库备份到其他机器上，可考虑使用在线的Git服务，比如GitHub。

GitHub仓库地址格式为https://github.com/<你的用户名>/animal-farm.git

将GitHub上的远程仓库的地址添加到本地的仓库中去（这里的\<url>换成GitHub仓库的地址）。

```
$ git remote add origin <url>
```

然后，将本地仓库推送（push）到远程仓库。

```
$ git push -u origin master
```

***

***

**注意**

GitHub已不允许密码访问，要用令牌

创建令牌的方式：

* 单击个人资料照片，然后单击Settings
* 左侧栏选Developer settings
* 左侧栏选Personal access tokens（个人访问令牌）
* 单击Generate new token
* 给令牌一个描述性名称
* 选择要授予此令牌的作用域或权限。要使用令牌从命令行访问仓库，选择repo
* 单击Generate token
* 复制令牌，处于安全原因，离开页面后，将无法再次看到令牌。

**警告** ：像对待密码一样对待令牌确保机密性。使用API时，应将令牌用作环境变量，而不是将其硬编码到程序中。

* 在命令行上使用令牌

> 如果有令牌，则可以在通过HTTPS执行Git操作时输入令牌，而不是密码。
>
> 例如，在命令行中输入以下内容：
>
> ```
> $ git clone https://github.com/username/repo.git
> Username:your_username
> Password:your_token
> ```
>
> * 个人令牌只能用于HTTPS Git操作。如果你的仓库使用SSH远程URL，则需要将远程URL从SSH切换到HTTPS。
>
> * 如果没有提示输入用户名和密码，说明你的凭据可能已缓存在计算机上。可以在密钥链中更新你的凭据，用令牌替换你的旧密码。
> * 可以使用Git客户端缓存PAT，而不必为每个HTTPS Git操作手动输入PAT。Git会将你的凭据临时存储在内存中，直到过期为止。还可以将令牌存储在Git可以在每个请求之前读取的纯文本文件中
>
> > ##### 在Git中缓存GitHub凭据
> >
> > 如果使用SSH克隆GitHub仓库，则可使用SSH密钥进行身份验证。开启凭据小助手使Git将你的密码在内存中保存一段时间。默认情况下，Git会缓存密码15分钟。
> >
> > 1 在终端，输入以下命令
> >
> > ```
> > $ git config --global credential.helper cache
> > # set git to use the credential memory cache
> > ```
> >
> > 2 要更改默认的密码缓存时限，输入以下命令：
> >
> > ```
> > $ git config --global credential.helper 'cache --timeout=3600'
> > # Set the cache to timeout after 1 hour (setting is in seconds)
> > ```



为了使两个仓库同步，每提交一次都需要重新推送。

> 上述方法可以连接远程仓库不行，还有如下秘钥方法：
>
> - 设置SSH Key：GitHub上连接已有仓库时的认证，是通过使用了SSH的公开秘钥认证方式进行的。已经创建过秘钥，用现有秘钥进行设置。运行下面的命令创建SSH Key。
>
> ```
> ssh-keygen -t rsa -C "your_email@example.com"
> Generating public/private rsa key pair.
> Enter file in wich to save the key
> (/Users/your_user_directory/.ssh/id_rsa): 按回车
> Enter passphrase (empty for no passphrase): 输入密码
> Enter same passphrase again: 再次输入密码
> ```
>
> 然后会出现以下结果。
>
> ```
> Your identification has been saved in /Users/your_user_directory/.ssh/id_rsa.
> Your public key has been saved in /Users/your_user_directory/.ssh/id_rsa.pub.
> The key fingerprint is:
> fingerprint值 your_email@example.com
> The key's randomart image is:
> +--[ RSA 2048]----+
> |     .+   +      |
> |       = o O .   |
> 略
> ```
>
> id_rsa是私有秘钥，id_rsa.pub是公开秘钥。
>
> ### 添加公开秘钥
>
> 在GitHub中添加公开秘钥，今后就可以用私有秘钥进行认证了。
>
> 点击右上角Account Settings，选择SSH Keys，在Title中输入适当的秘钥名称。Key部分粘贴id_rsa.pub文件里的内容。id_rsa.pub的内容可以用如下方法查看。
>
> ```
> $ cat ~/.ssh/id_rsa.pub
> ssh-rsa 公开秘钥的内容 your_email@example.com
> ```
>
> 
>
> 添加成功后，邮箱会接到一封提示“公共秘钥添加完成”的邮件。
>
> 完成以上设置后，就可以用手中的私人秘钥与GitHub进行认证和通信了。
>
> ```
> $ ssh -T git@github.com
> The authenticity of host 'github.com(207.97.227.239)' can't be established.
> RSA key fingerprint is fingerprint值.
> Are you sure you want to continue connecting (yes/no)? 输入yes
> Hi hirocastest! You've successfully authenticated, but GitHub deos not 
> provide shell access.
> ```



#### 3 克隆一个已有仓库

如果已经在远程仓库（如GitHub）中保存有代码，那么只需要克隆（clone）一份到本地。

找到远程仓库的地址，地址可能是HTTPS协议或SSH协议。执行下列命令将\<url>换成仓库的地址。

```
$ git clone <url>
```

执行此命令后，本地机器上就有了以项目名称命名的目录，其中保存了仓库的内容。

本地仓库和远程仓库是相互独立的，如果想要在远程仓库被修改之后更新本地仓库，可以在曾经复制过的本地仓库中执行`git pull --rebase`

#### 4 使用Git

1 添加文件、提交和推送

将文件animals/lion.py添加到仓库中，文件内容如下:

```python
"""A lion"""
import animal
class Lion(animal.Animal):
    def __init__(self):
        self.kind = 'lion'
        
    def get_kind(self):
        return self.kind
```

接下来在animal_farm.py中调用lion.py

...

from animals import dog

**from animals import lion**

from animals import sheep

...

if kind == 'dog':

​		return dog.Dog()

**if kind == 'lion':**

​		**return lion.Lion()**

if kind =='sheep':

​		return sheep.Sheep()

...

修改完成后，通过如下命令查看文件的状态（查看哪些修改将会被提交）。

```
$ git status
```

可看到显示animal_farm.py被修改，而且添加了新的文件animals/lion.py

若要将文件保存到Git历史记录中，则需要将文件先存放到暂存区。

可以添加单个文件

```
$ git add <filename>
```

也可以添加整个当前目录。

```
$ git add .
```

执行`git status`可以看到暂存区的内容。

将一个或多个文件提交到历史记录中，可执行如下命令：

```
$ git commit -m "<infomative message describing the change>"
```

如果已经创建好远程仓库，则将修改推送到远程仓库的命令如下:

```
$ git push
```

假如有多个人使用同一个远程仓库，则与其他人保持同步的命令如下[多人协作时最好先拉取(pull)再推送(push)]

```
$ git pull --rebase
```

如果查看提交历史，可执行如下命令

```
$ git log
```

log顶部为最新提交，底部为初始提交。

如果在本地工作副本（working copy）拉取某一个特定的提交（如查看初始提交的情况），则可以执行如下命令（这里的\<sha1>为提交ID，它是一个十六进制的字符串，如e1fec4ab2d14ee331498b6e5b2f2cca5b39daec0）

```
$ git checkout <sha1>
```

#### 创建和合并分支

不同的分支通常对应于工作中不同的功能。一旦某个功能成熟了，该分支就可以被合并到主分支（master）。创建一个新分支的命令如下。

```
$ git checkout -b <branch-name>
```

比如，下面的命令将创建一个关于新的动物类型的分支。

```
$ git checkout -b feature-leopard
```

然后，就可以在这个分支上正常操作了。

查看所有分支的命令：

```
$ git branch -a
```

当前分支前面有一个星号(*)

切换到其他分支的命令如下：

```
$ git checkout <branch-name>
```

回到主分支的命令如下

```
$ git checkout master
```

回到主分支后，就可以将之前的新分支合并到主分支，命令如下：

```
$ git merge feature-leopard
```

### Git与Vim的整合(vim-fugitive)

Tim Pope的vim-fugitive插件可以使用户在不离开Vim的情况下进行Git操作。在使用Vim编辑文件时，也可以同时利用版本管理系统记录编辑的位置。

> 如果使用vim-plug安装插件，则可以在.vimrc中添加Plug 'tpope/vim-fugitive'，并执行命令:w | source $MYVIMRC | PlugInstall

vim-fugitive提供的许多命令都是对外部Git命令的镜像，然而，输出通常更具交互性。以git status为例，执行如下命令：

```
:Gstatus
```

​	可以在一个分割窗口中看到熟悉的git status输出结果（显示的是修改但还未提交的情况）

但和git status在命令行中的输出不同，分割窗口是可交互的。可以将光标移动到其中一个文件上（使用Ctrl+n组合键和Ctrl+p组合键），也可尝试：

* -用于将文件移入或移除暂存区。
* cc或:Gcommit用于提交暂存区中的文件。
* dd或:GDiff用于打开一个文件差异对比窗口。
* g?用于显示更多命令的帮助信息。

:Gclog将打开与当前打开文件相关的历史记录，它们被显示在一个快速恢复窗口。

> 使用:copen可打开一个快速恢复窗口，用:cnext和:cprevious命令可以在不同快速恢复窗口之间切换，以查看不同文件的历史版本

命令行:Git blame可以快速提示每一行中修改操作的时间和用户。blame的意思是责怪某个编写了问题代码的开发者。:Git blame在一个垂直分割窗口中交互式显示git blame的输出。

:Git blame在文件的每一行旁边显示了相关提交的ID、作者、日期和时间。

:Git blame窗口中可使用如下命令：

* C、A和D可调整blame窗口到制定的大小（分别对应看到提交、作者和日期为止）
* Enter键用打开所选提交的文件差异
* o用于在分割窗口中打开所选提交的文件差异
* g?用于显示更多命令的帮助信息

:Git blame非常有用，它对于确定出错时间很有帮助。同时，它还提供了其他方便用户使用的包装器。

* ：Gread可直接在缓冲区中预览指定文件的内容。
* ：Ggrep封装了git grep（Git提供了强大的grep命令，可以用于搜索某个文件在任意时刻的状态）
* :Gmove用于移动文件（同时重命名相应的缓冲区）
* ：Gdelete封装了git remove命令

Vim帮助（如：help fugitive）中包含各个插件详细的使用方法

### ==用vimdiff解决冲突==

示例文件可以从官方GitHub仓库PacktPublishing/Mastering-Vim的Chapter05/animal_farm中找到

#### 比较两个文件

比较两个文件的差异

用vimdiff打开多个文件的命令行

```
$ vimdiff animals/cat.py animals/dog.py
```

打开界面中的颜色和.vimrc中的colorscheme设置有关。不同的行用一种颜色高亮显示，不同的字符用另一种颜色高亮显示。

可以用]c和[c分别在多个修改处之间向前和向后跳转。

可以在不同文件之间推送修改。

* do或:diffget（do表示diff obtain，即获得差异的意思）将文件修改应用于当前窗口中的文件。
* dp或:diffput（dp表示diff put，即推送差异的意思）将当前窗口中的文件修改推送给另一个文件。

> 若要将一个文件整体复制到另一个文件中，可以使用:%diffget或:%diffput。

如，若要将cat.py文件中的self.kind = 'cat'推送到dog.py文件，可以用]c和[c将光标移动到cat.py文件中的相应行，并输入do。然后，该行的高亮显示会消失，而dog.py的缓冲区则会应用这个改动。

> 当使用：diffget 和 :diffput在文件之间消除差异时，vimdiff会自动更新高亮显示。但如果手动修改了文件，则需要执行:diffupdate（或缩写:diffu）手动更新高亮显示。

也可以同时比较多个文件的差异，只是不能再使用do和dp快捷键实现这种功能。如，下面的命令行可以比较3个文件的差异

```
$ vimdiff animals/cat.py animals/dog.py animals/sheep.py
```

因为现在有多个缓冲区，所以消除文件差异时就需要指定来源缓冲区或目的缓冲区。:diffget（可简写为:diffg）和:diffput（可简写为:diffp）都需要指定一个参数，此参数既可以是缓冲区编号（用:ls!查看），也可以是缓冲区名称的一部分。

比如，在animals/dog.py窗口中，可以将光标所在行的差异推送到animals/sheep.py中，命令如下:

```
:diffput sheep
```

该行将会被推送到animals/sheep.py缓冲区中

> 如果经常使用vimdiff，可以绑定别名与快捷键

### vimdiff 和 Git

将vimdiff作为Git的合并工具会让人感到困惑，因为Vim会打开4个窗口，还需要记住很多快捷键，却得不到足够的帮助提示。

#### 1 git config

第一件事是将vimdiff设置为Git的合并工具

```
$ git config --global merge.tool vimdiff
$ git config --global merge.conflictstyle diff3
$ git config --global mergetool.prompt false
```

上述命令用于设置Git的默认合并工具，在合并时显示两个分支的共同祖先，并在打开vimdiff时禁止弹窗提示。



#### 2 创建合并冲突

这里仍使用仓库animal_farm。首先，利用以下命令进入仓库

```
$ cd animal_farm/
```

然后，创建一个新分支animal-create，并构造一些冲突（与主分支冲突）。在animal-create分支中，将方法make_animal重命名为create_animal，而在主分支中将其命名为build_animal。下面每一个操作的顺序很重要，要按步骤来进行。

1 创建新分支，并编辑animal_farm.py，命令如下。

```
$ git checkout -b create-animal
$ vim animal_farm.py
```

2 在编辑时，将make_animal方法换成create_animal，代码如下。

```
...
def create_animal (kind):
...
	animal_farm.add_animal(create_animal(animal_kind))
...
```

3 提交修改。

```
$ git add animal_farm.py
$ git commit -m "Rename make_animal to create_animal"
```

4 切换回主分支，并修改文件。

```
$ git checkout master
$ vim animal_farm.py
```

5 在Vim中将make_animal修改为build_animal.

```
...
def build_animal (kind):
...
	animal_farm.add_animal(build_animal(animal_kind))
...
```

6 提交文件

```
$ git add animal_farm.py
$ git commit -m "Rename make_animal to build_animal"
```

7 将create-animal分支合并到主分支(master)

```
$ git merge create-animal
```

此时，合并冲突产生了

#### 3 消除合并冲突

要想消除合并冲突，应该先要打开Git合并工具（之前已经设置为vimdiff了）。

```
$ git mergetool
```

冲突的显示界面并不是很友好，包含了4个窗口以及各种颜色。该界面看起来杂乱无章，其实不然。左上窗口显示本地修改（主分支修改），中间窗口是共同祖先，右上窗口为create-animal分支。底部窗口为合并结果。

从左到右，丛上至下，这4个窗口的含义依次如下。

* LOCAL（本地）：当前分支的一个文件（或待合并的任意文件）
* BASE（共同祖先）：该文件在两个分支中的共同祖先（修改发生之前的状态）
* REMOTE（远程）：另一分支中待合并的文件
* MERGED（合并）：合并结果，即将要保存的输出结果。

在MERGE窗口中，可以看到冲突的标记，即<<<<<<<和>>>>>>>记号

```
<<<<<<<[LOCAL commit/branch]
[LOCAL change]
||||||| merged common ancestors
[BASE - closest common ancestor]
=======
[REMOTE change]
>>>>>>> [REMOTE commit/branch]
```

由于这里只有两个文件，因此只需要运行do(:diffget)或dp(:diffput)命令，而不需要指定参数。

如果想保留REMOTE中的修改（此处为create-animal分支），可以先使光标移至MERGED窗口中（底部的窗口），然后将光标移动到下一个修改处（除常规的快捷键外，还可以使用]c和[c在多个修改处之间跳转。）最后，执行如下命令。

```
:diffget REMOTE
```

者会将REMOTE中的文件修改应用于MERGED窗口中的文件上。上述命令可作如下简化。

* 获得REMOTE中的一个修改：diffg R
* 获得BASE中的一个修改：diffg B
* 获得LOCAL中的一个修改：diffg L

对每一处冲突重复执行上述过程。一旦处理完所有冲突，就可以保存MERGED中的文件，然后退出vimdiff（使用一条命令:wqa即可），这就完成了合并过程（若有多个文件存在冲突，则会跳转到下一个文件中）

> 合并冲突时会在工作目录中留下后缀为.orig的文件，比如animal_farm.py.orig。完成合并过程后，可以将它们删除。

最后要提交合并的结果，即执行命令git commit -m "修改一个合并冲突"

### Tmux、Screen和Vim的终端模式

软件开发除编写代码，还涉及执行二进制程序、运行测试和使用命令行完成某些任务，这时候，会话和窗口就有用了。

现代桌面环境支持同时显示多个窗口，而Vim更关心如何在单个终端会话中管理多个任务。

#### ==Tmux==

这是个终端复用工具，可在一个终端屏幕中管理多个终端窗口。

> 如果使用的是Debian系的发行版，安装方法为sudo apt-get install tmux。如果从源码编译Tmux，参考GitHub仓库tmux/tmux。

Tmux启动方法是在终端中执行tmux命令

#### 1 类似于分割窗口的Tmux面板

Tmux支持多面板（相当于Vim中的窗口）和多窗口（相当于Vim中的标签页）。使用Tmux的功能时，首先要按一个前缀建，然后再按命令键。默认的前缀键为Ctrl+b组合键，需要在配置文件~/.tmux.conf中添加如下设置。

> \# 使用Ctrl+\作为前缀键
>
> unbind-key C-b
>
> set -g prefix 'C-\\'
>
> bind-key 'C-\\' send-prefix 

然后重启Tmux（或先按Ctrl+b组合键，然后执行:source-file ~/.tmux.conf命令），以使配置生效。

垂直分割屏幕的快捷键为Ctrl+b+%

> 对于很多人而言，使用默认快捷键并不方便。比如，短横线-让人更容易联想起垂直分割，因而可以通过下面的配置修改这个行为。
>
> ```
> # 使用-生成垂直分割
> bind - split-window -v
> unbind '%'
> ```
>
> 注:Tmux和Vim对水平分割和垂直分割的定义刚好相反，Tmux中的垂直分割是生成上下两个面板，而Vim的垂直分割是生成左右两个窗口

水平分割屏幕的快捷键为Ctrl+b+"。

> 和垂直分割一样，可以使用管道符|来记忆水平分割，配置如下。
>
> ``` 
> # 使用|生成水平分割
> bind | split-window -h
> unbind '"'
> ```

可以用前缀键（默认Ctrl+b）加方向键在不同面板之间跳转。每个面板都是独立运行的。比如，可以在各个面板中独立地修改当前目录、执行命令以及使用Vim编辑文件（这是最关键的）。

> 如果已经习惯于使用hjkl键，会觉得方向键不方便，这时可以在配置文件~/.tmux.conf中添加对hjkl的支持。
>
> ```
> bind h select-pane -L
> bind j select-pane -D
> bind k select-pane -U
> bind l select-pane -R
> ```
>

关闭面板并退出会话的方法是执行命令行exit或按快捷键Ctrl+d

#### 2 类似于标签页的Tmux窗口

创建Tmux窗口的快捷键是Ctrl+b+c

Tmux窗口默认被命名为当前活跃Tmux窗口中的进程名称，Tmux窗口重命名快捷键为Ctrl+b+,

在Tmux窗口之间跳转的快捷键时Ctrl+b+p和Ctrl+b+n

#### 3 强大的会话功能

如果使用SSH连接远程机器，那么Tmux是一种非常重要的工具，因为Tmux支持长时会话，其寿命比SSH连接还要长。

进入一个Tmux会话之后，可以用Ctrl+b+d组合键退出该会话(detach) ，会回到原来的Shell命令行中，并显示类似于[detached (from session 0)]的消息。

Tmux会话可以一直持续到关机，显示所有Tmux会话的命令如下：

``` 
$ tmux list-sessions
```

其显示结果的格式如下

```
0:2 windows (created Sat Aug 18 19:17:59 2018) [80x23]
```

上面的例子显示，当前只有一个Tmux会话。可以打开这个会话，或者用Tmux的术语来说，叫做附于（attach）此会话。打开会话的命令如下

```
$ tmux attach -t 0
```

不指定任何参数，直接执行tmux命令总是会新建一个会话。

可以创建任意多个会话，这样就能够将不同的工程放到不同的会话中。

用Tmux会话组织工程或任务颇有好处，建议养成这个习惯

在多个Tmux会话间前后跳转的快捷键为Ctrl+b+)和Ctrl+b+（

也可以重命名会话，既可以在启动Tmux时指定名称（tmux new -s name)，也可以在会话中指定(Ctrl+b+$组合键)。

#### 4 Tmux和Vim分割窗口

常常同时使用Tmux面板和Vim面板，它们可以互补。可以在一个Tmux面板中打开Vim，然后在其余的Tmux面板中执行Shell命令。

Tmux和Vim遍历窗口（在Tmux中称为面板）的快捷键不同，最简单的解决方案是使用vim-tmux-navigator插件，GitHub仓库为christoomey/vim-tmux-navigator。vim-tmux-navigator使用的快捷键为Ctrl+h、Ctrl+j、Ctrl+k和Ctrl+l，并且Vim窗口和tmux面板的跳转方式保持一致。

> 为了使用vim-tmux-navigator，Tmux版本不能低于1.8。检查Tmux版本的命令为`$ umux -V`。

安装此插件后，可以在~/.tmux.conf中加入如下快捷键绑定（这段配置文件来自GitHub仓库christoomey/vim-tmux-navigator）。

```
# Smart pane switching with awareness of Vim splits.
# See: https://github.com/christoomey/vim-tmux-navigator
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
| grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n C-h if-shell "$is_vim" "send-keys C-h" "select-pane -L"
bind-key -n C-j if-shell "$is_vim" "send-keys C-j" "select-pane -D"
bind-key -n C-k if-shell "$is_vim" "send-keys C-k" "select-pane -U"
bind-key -n C-l if-shell "$is_vim" "send-keys C-l" "select-pane -R"
bind-key -T copy-mode-vi C-h select-pane -L
bind-key -T copy-mode-vi C-j select-pane -D
bind-key -T copy-mode-vi C-k select-pane -U
bind-key -T copy-mode-vi C-l select-pane -R
```

> 对于Tmux高级用户或有钻研精神的人，可以使用TPM（Tmux插件管理器）来实现快捷键的绑定，而不需要在.tmux.conf中粘贴代码。安装好TPM之后，可以在.tmux.conf中添加如下配置：
>
> ```
> set -g @plugin 'christoomey/vim-tmux-navigator'
> run '~/.tmux/plugins/tpm/tpm'
> ```

### Screen

Screen 是Tmux的思想先驱，可扩展性不如Tmux，若不经过一番配置，Vim和Screen不能和谐共处。

运行Screen时，Esc键在Vim中没有正常注册。为了修复Ese键的行为，需要在~/.screenrc文件中加入如下配置。

```
# 当检测输入的按键序列时，等待时间不超过5ms，这样可以修复Vim中的Ese行为
maptimeout 5
```

Screen还设置了$TERM环境变量，Vim却不能识别这个值，为了修正这项缺陷，可以在.screenrc中添加如下代码。

```
# 将$TERM设置为Vim能够识别的值
term screen-256color
```

同时使用Vim和Screen，还有其他不便之处，比如Home和End键没有注册，Vim Wikia中深入介绍了使Vim和Screen兼容的方法。

### 终端模式

可以使用:!在Vim中运行Shell命令。比如，运行Python程序的命令如下:

```
:!Python3 animal_farm.py cat dog sheep
```

执行命令时，Vim会暂停，并在终端显示程序运行情况。

这种传统方式不太方便，因为用户无法在运行程序的同时编辑文档。不过，终端模式可以解决这个问题。

在8.1版本中，Vim有了终端模式。这是Vim会话中运行的一个终端模拟器。和Tmux不同，终端模式时Vim自带的。当一个程序需要运行很长时间时，终端模式是很有帮助的，因为用户可以同时使用Vim进行其他操作。

终端模式可以用命令：term启动。它会打开一个上下分割的窗口，然后在其中运行默认的Shell解释器。

终端模式窗口与其他Vim窗口相同，也可以改变大小或移动。只不过此窗口初始情况下处于终端工作模式（terminal-job），操作方式类似于插入模式。而且，它还有一些特殊的键盘绑定。

* Ctrl+w,N用于进入终端正常模式(terminal-normal)。切换到插入模式的那些操作（比如i或a键）会让终端正常模式回到终端工作模式。
* Ctrl+w,"用于添加一个寄存器，它会将该寄存器中的内容粘贴到终端中。 比如，用yw复制了一些内容，然后执行Ctrl+w,"会将默认寄存器中的内容复制到终端。
* Ctrl+w,Ctrl+c将中断快捷键（Ctrl+c）发送给终端。

终端模式较突出的一个功能是它可以用一个命令启动终端模式，并把输出保存到一个缓冲区中。

```
:term python3 animal_farm.py cat dog sheep
```

此命令会在Shell中执行一条命令，一旦执行完成，输出结果就会显示在一个缓冲区中。

如果想要在左右分割窗口中使用终端模式，可以执`:vert term`

> 如果习惯用Ctrl+hjkl组合键在Vim窗口之间跳转，可以在终端模式中设置同样的快捷键。.vimrc中的设置如下：
>
> ```
> tnoremap <c-j> <c-w><c-j>
> tnoremap <c-k> <c-w><c-k>
> tnoremap <c-l> <c-w><c-l>
> tnoremap <c-h> <c-w><c-h>
> ```

将Vim终端模式与Tmux结合是最好的，Tmux适合管理会话（每个会话对应于一个任务），而终端模式可用来管理窗口。如，Vim的终端窗口可用于处理项目中的工作，而Tmux窗口（在Vim中叫做标签页）则用于在不同任务之间切换。

### 构建和测试

在处理代码时，常常需要编译（Python不是编译型语言，不在此列），而且往往还需要测试。

Vim支持通过快速恢复列表和位置列表来提示编译和测试错误。

#### 快速恢复列表

为了更方便地跳转到文件的某个部分，Vim采用了一种附加模式，即快速恢复列表。

一些Vim命令利用它在文件的不同位置之间跳转。

如:make产生的编译错误，或:grep及:vimgrep产生的搜索匹配项。诸如Linter（语法检查器）或测试运行器之类的插件也会使用快速恢复列表。

如，用:grep命令从当前目录（.目录）开始，在每一个Python文件(--include="*.py")中递归（-r选项）搜索关键词animal，此命令会用到快速恢复列表。

```
:grep -r --include="*.py" animal .
```

此命令会在当前窗口打开第一个匹配项。如果想在快速恢复窗口中查看所有的匹配项，则可以:copen命令，它会在一个上下分割的窗口中显示结果。

在快速恢复窗口中，可以使用正常的Vim移动光标命令，可以用Ctrl+f组合键和Ctrl+b组合键翻页，也可以用？和/前后搜索。Enter键将在缓冲区中打开当前匹配项所在的文件，而且光标会被置于匹配处。

如果想要打开文件animals/sheep.py中的匹配项import animal，则可以先在快速恢复窗口中用/sheep进行搜索，然后按n键找到相应的行，再按Enter键。这时，在原窗口中将打开sheep.py文件，并将光标置于匹配处。

关闭快速恢复列表的命令是:cclose（如果快速恢复窗口是当前窗口，则可以用:bd命令来删除快速恢复窗口的缓冲区）。

在不打开快速恢复窗口的情况下，可以用如下命令浏览快速恢复列表。

* :cnext或:cn命令用于切换到快速恢复列表中的下一项。
* :cprevious或:cp（或:cN）命令用于切换到快速恢复列表中的上一项

也可以选择只在出现错误时才打开快速恢复窗口（如编译错误），命令为:cwindow或:cw。

#### 位置列表

类似于快速恢复列表，只不过它是当前窗口的局部列表。在一个Vim会话中只有一个快速恢复列表，而位置列表则可以有很多个

要填充位置列表的命令，只需要在大部分快速恢复列表相应命令前加上字母l，比如:lgrep或:lmake

* :lopen用于打开位置列表窗口
* :lclose关闭位置列表窗口

* :lnext切换到位置列表中的下一项
* :lprevious用于切换到位置列表中的上一项
* :lwindow用于在错误出现时才触发位置列表窗口。

一般而言，多个窗口共享同一结果时，需要使用快速恢复列表；而某个结果只与一个窗口相关时，位置列表更合适。

#### 构建代码

Python没有代码需要编译，但可以通过了解代码构建过程来理解Vim是如何执行代码的。

Vim提供了:make命令，它其实封装了UNIX make程序。make是一种非常古老的构建管理方案，它可以按需重新编译部分程序，或完全重新编译。

：make的一些选项。

* ：compiler用于指定其他的编译器插件，它也会修改编译器的输出格式。
* ：set errorformat 可定义一些便于识别的错误格式。
* :set makeprg 用于设置执行:make时使用哪个命令行程序。

> 其他选项参考Vim手册中的文档，执行:help<任意主题>命令

上面的选项可以组合起来与任意编译器一起使用。比如，如果想要编译C程序，则需要调用GCC（标准的

C编译器），使用如下命令进行设置。

```
:compiler gcc
:make
```

:make的重要之处在于，它允许Vim用户实现自己的语法检查器、测试运行器或任意编译器插件（可以引用源码中的位置，从而使用快速恢复列表和位置列表）。

Vim8.1中引入了终端模式，这为在Vim中长时间运行构建过程打下了坚实的基础。因为:term make 将异步调用make，所以与此同时还可以继续编写代码。

#### 插件——vim-dispatch

Tim Pope的插件vim-dispatch改进了:make命令，使它可以异步执行，并且增添了很多语法糖和命令，

但在Vim8.1增加了终端模式之后，vim-dispatch的很大一部分功能显得有些多余。不过如果你有自己偏爱的工作流程，而且需要与不同的终端模拟器整合，则vim-dispatch 还是很有用的。其仓库为tpope/vim-dispatch。

vim-dispatch的一些主要功能：

* :Make在另一个窗口中运行某个任务（只针对Tmux、iTerm或cmd.exe）。
* :Make!在后台运行一个任务（只针对Tmux、Screen、iTerm或cmd.exe）
* :Dispatch将:compiler \<compiler-name>和:make组合起来，比如：Dispatch testrb test/models/user_test.rb
* :Dispatch还可以运行任意命令，比如:Dispatch bundle install

如果经常使用:make命令，那么vim-dispatch还是值得尝试的。

从技术角度讲，完全可以用vim-dispatch运行测试，但是测试一般没有提供标准化输出，vim-dispatch无法自动关联到快速恢复列表或位置列表。

#### 测试代码

测试的输出结果往往没有编译错误那么有规则。

有很多与测试运行器相关的插件。

终端模式使得你可以在编写代码的同时运行测试。

#### 插件——vim-test

一种非常流行的测试运行器，因为它提供了对多种编译器的支持，可以接入许多其他的测试运行器。

还提供了一些易用的键盘映射。对于Python而言，vim-test支持djangotest、django-nose、node、nose2、pytest和PyUnit。在使用vim-test前，应确保已经安装了需要的测试运行器。

vim-test支持如下命令：

* :TestNearest运行离光标最近的测试
* :TestFile运行当前文件中的测试
* :TestSuite运行整个测试集
* :TestLast运行最后那个测试。

vim-test 还允许用户指定测试策略。如make、neomake、MakeGreen以及dispatch（或dispatch_background）会弹出快速恢复窗口。

如果希望使用vim-dispatch运行测试（如在另一个终端窗口中运行一个测试），则可以在.vimrc文件中添加下列代码。

```
let test#strategy = "dispatch"
```

### 用Linter来检查语法

语法检查（又称为Lintering，语法检查器称为Linter）是多人参与的软件项目中的一个核心步骤。

Python代码比其他很多语言都要简单，因为它只遵守单一的标准PEP8。

确保Python代码符合PEP8常用的语法检查器有Pylint、Flake8和autopep8。

在检查语法之前，需要确保上述工具已经安装成功（下面的示例适用于Pylint），因为Vim所做的也不过是调用这些外部工具而已。

> 如果使用的是类Debian的Linux发行版，则可以运行sudo apt-get install pylint3来安装针对Python3版本的Pylint。

#### 1 在Vim中使用Linter

很多常用的语法检查器都带有相关的插件，以避免用户过度纠结于语法检查器的细节。不过，在定制某种语法检查器时，如有必要，可以用Vim弹出一个快速恢复列表。

可以用Vim的:make命令生成一个快速恢复列表。默认情况下，它会运行UNIX的make命令，但可以通过设置makeprg变量改变此默认行为。

快速恢复列表会假设：make只以某种特定的格式输出结果，也可以尝试为其他格式找到适合的语法检查器。但这样容易出错，而且有潜在的兼容性问题（因为所依赖的语法检查器可能会发生变化）。

在.vimrc中加入如下代码,它会在打开Python文件时替换默认的:make行为。

```
autocmd filetype python setlocal makeprg=pylint3\ --reports=n\ --msg-
template=\"{path}:{line}:\ {msg_id}\ {symbol},\ {obj}\ {msg}\"\ %:p
autocmd filetype python setlocal errorformat=%f:%l:\ %m
```

在打开Python文件的情况下，运行:make | copen 命令就可以生成一个快速恢复列表。

> 当不熟悉语法检查器的用法时，可能希望隐藏不关心的那些警告。
>
> 对于Pylint而言，可以在~/.pylintrc中加入代码`disable-invalid-name,missing docstring`来实现这一点，或者在文件尾加上代码`# pylint: disable=invalid-name`。 每一种语法检查器都有自己隐藏警告的方法。

#### 2 插件——Syntastic

Syntastic是语法检查的首选插件，它支持超过100种语言（并且可以通过其他小型语法检查插件来扩展其功能）。

GitHub仓库'vim-syntastic/Syntastic'

Syntastic对新手并不非常友好，需要在.vimrc中加入如下设置。

```
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0

let g:syntastic_python_pylint_exe = 'pylint3'
```

然后，只要在系统中安装一种Python语法检查器（如Pylint），就可以在打开Python文件时看到对不恰当语法问题的警告

有些信息需要注意：

* Syntastic将有语法错误的行用>>标记出来
* 出错的字符也被高亮显示出来。
* 打开了一个位置列表，列出了当前文件中的所有错误。
* 状态栏显示了当前行中的错误信息。

因为这只是一个正常的位置列表，所以可以在此窗口中使用常规命令浏览位置列表（如:lnext或:lprevious）。

如果修改了错误，则语法错误列表会在文件保存时自动更新。

#### 3 插件——ALE

异步语法检查引擎（Asynchronous Lint Engine,ALE）是语法检查领域的新手，不过它的流行程度接近于Syntastic。它的主要优点是可以在输入时同步显示语法检查错误，即它在后台异步运行检查器。

GitHub仓库'W0rp/ale',ALE要求Vim版本在Vim8以上，或者使用Neovim，因为它依赖于Vim的异步调用功能。

ALE是开箱即用的，而且输出与Syntastic类似。

可以用`:ALEToggle`命令切换是否启用ALE，以便在不需要语法检查时随时将它关闭。

启用ALE后（用:lopen打开一个位置窗口），错误行高亮显示，屏幕底部的状态栏显示了相关语法检查信息（linmessage）。

ALE的功能已经超出了语法检查器的范畴，称为一个全功能的语言协议服务器，它支持自动补全、定义跳转。

当然，它还没有YouCompleteMe那么完善和流行，但ALE也有了日益庞大且忠实的用户群体。

对于代码中的引用，可以使用:ALEGoToDefinition命令跳转到定义，并使用:ALEFindReferences命令查找定义的引用之处。为实现自动补全功能，需要在.vimrc中加入代码`let g:ale_completion_enabled = 1`

## 用正则表达式和宏来重构代码

示例可在GitHub仓库PacktPublishing/Mastering-Vim中找到。

### 用正则表达式来搜索和替换

正则表达式（regular expression,regex)是一种很强大的工具，非常值得学习和掌握。Vim有一套独特的正则表达式语法。

#### 搜索和替换

Vim通过:substitute命令实现搜索和替换功能，大部分时候都会将其简写为:s。

默认情况下，:s命令将当前行中的一个子字符串替换为其他字符串，其命令形式如下。

```
:s/<find-this>/<replace-with-this>/<flags>
```

<选项>参数是可选的。打开animal_farm.py文件，体验此命令。跳转到包含cat的行（如用搜索命令/cat），然后执行:s/cat/dog命令。

则当前行中的第一个cat被替换成了dog。



:substitute 的一些选项。

* g表示全局替换，即将匹配到的所有项都替换掉，而不仅仅是第一个

* c表示每次替换前需要确认，即弹出一个界面供用户确认是否替换。

* e表示没有匹配项时不显示错误。
* i表示忽略大小写，即搜索时不关心大小写。

* I表示区分大小写。

这些选项可以组合使用（除了i和I之外）。比如，命令`:s/cat/dog/gi`会将字符串cat.Cat()替换为dog.dog()。

:substitute命令可以作用于一个区间范围，即哪些内容中的匹配项会被替换掉。常用的范围是%，它使:s命令作用于整个文件。

如果要将一个文件中的所有animal替换成creature，则只需要执行:`%s/animal/creature/g`命令。

替换完成后，:substitute会在屏幕底部的状态栏显示有多少个匹配项被替换掉了。这看起来已经是一种简单的代码重构了。

:substitute还支持其他区间范围，常用的有以下几种。

* 数字，表示行号。
* $表示最后一行。
* %表示整个文件（最常用的一种）。
* /search-pattern/，即在下一个搜索结果所在的行操作。
* ？backwards-search-pattern?，与/search-pattern/功能类似，只不过是反向搜索。

此外，这些区间范围可以用；运算符组合起来。如20;$表示从20行到最后一行。

下面是个更复杂的例子，表示从12行开始找到包含dog的行，在这个范围内的所有animal都被替换成creature。

```
:12;/dog/s/animal/creature/g
```

（用:set nu 命令显示行号）

还可以在可视模式中将选中的文字作为默认的区间，这时不指定任何区间，直接执行:s命令，会在选中的文字上执行替换操作。更多内容参见:help cmdline-ranges中关于区间的介绍。

> 如果使用的是Linux风格的路径，或路径中包含/符号，则可以用反斜线\符号进行转义，避免与替换命令的分隔符混淆。当然，也可以修改替换命令的分隔符，比如`:s+path/to/dir+path/to/other/dir+gc`中的命令分隔符被改成了+，它等价于:s/path\/to\/dir/path\/to\/other\/dir/gc。

大部分情况下，可以用下面的命令将整个文件中的所有匹配项替换掉。

```
:%s/find-this/replace-with-this/g
```

在替换文本的时候，有时候可能只想替换那些完整的单词，这时可以用单词界定符\\<和\\>。

比如，在animal_farm.py中，若用/animal搜索（先启用:set hlsearch命令来高亮显示搜索结果），可以搜到所有的animal，但有些却不仅仅是animal单词本身，比如animals。

不过，如果使用/\\<aimal\\>，就能精确匹配单词animal，而将那些包含animal的其他单词排除在外，比如animals。

#### 用参数列表来处理多个文件

参数列表（argument list,arglist）支持在多个文件中执行同一操作，而不需要用户预先加载缓冲区（它会帮用户自动加载）

参数列表支持如下命令。

* :arg用于定义参数列表
* :argdo对参数列表中的所有文件执行一条命令
* :args用于显示参数列表中的文件列表。

如果想递归将每个Python文件中的单词animal替换掉，则可以使用如下命令。

```
:arg **/*.py
:argdo %s/\<animal\>/creature/ge | update
```

这两条命令的含义如下。

* :arg \<pattern>表示在参数列表中添加匹配\<pattern>模式的那些文件名，每个文件名对应有它自己的缓冲区。
* \*\*/\*.py是通配符，表示所有.py文件，它会递归匹配当前目录下的所有.py文件。
* :argdo 对参数列表中的每一项执行同一条命令。
* `%s/\<animal\>/creature/ge`将每个文件中的每一个（选项g的作用）单词animal都替换成creature，如果哪个文件中没找到，也不会报错（选项e的作用）。
* update等价于:write，用于保存每个修改过的缓冲区。

这里的update是必要的，因为Vim在切换缓冲区时推荐保存当前缓冲区。另一种方案是使用:set hidden命令，它会隐藏那些警告，可以在所有替换完成之后用:wa命令保存所有缓冲区。

尝试运行这条命令，可发现相关文件中的每一个匹配到的单词都被替换掉了（可通过git status或git diff来查看Git仓库中的文件修改情况）。参数列表中的文件列表可以通过:args命令来查看。

实际上，参数列表是Vi时代的遗留产物，那时的参数列表与今天缓冲区的使用方式类似。只不过现在的缓冲区涵盖了参数列表：每个参数列表项都在缓冲区列表中，但不每个缓冲区都在参数列表中。

从技术方面而言，可以用:bufdo命令来替代:argdo命令（因为参数列表都在缓冲区中），它会对每个打开的缓冲区执行同一操作。但是，这种行为是不明智的，因为生成参数列表之前可能已经无意中打开了其他缓冲区，使用:bufdo命令会对这一部分文件产生误操作。

#### 正则表达式基础

正则表达式引入了一些特殊模式，每种模式匹配一组字符，比如：

* `\(c\|p\)`arrot同时匹配carrot和parrot，这里的`\(c\|p\)`表示c或p。
* `\warrot\?`同时匹配carrot、parrot，甚至还匹配farro，这里的`\w`表示所有单词字符，而t\?表示t字符是可选的
* `pa.\+ot`匹配parrot、patriot，甚至还匹配pa123ot，这里的`.\+`表示一个或多个任意字符。

> 和其他很多正则表达式不同，Vim中的正则表达式的特殊字符需要\来转义（默认情况下，大部分字符不是正则表达式，只有少数例外，比如.或*。）当然，可以通过魔法模式改变这种行为。

#### 1 正则表达式中的特殊字符

常用的正则表达式符号。

| 符号 | 含义                   |
| ---- | ---------------------- |
| .    | 任意字符，但不包括行尾 |
| ^    | 行首                   |
| $    | 行尾                   |
| \\_  | 任意字符，包含行尾     |
| \\<  | 单词开始               |
| \\>  | 单词结尾               |

> 这类符号的完整列表可参考文档:help ordinary-atom

还有一类正则表达式称为字符类（character class）

| 符号 | 含义                                   |
| ---- | -------------------------------------- |
| \s   | 一个空白符（包括Tab和Space）           |
| \d   | 一个数字                               |
| \w   | 一个单词字符（包括数字、字母或下划线） |
| \l   | 一个小写字符                           |
| \u   | 一个大写字符                           |
| \a   | 一个字母字符                           |

这些字符类的大写版本表示它们的反类，比如\D匹配所有非数字的字符，而\L匹配除小写字母外的所有字符（注意，不仅仅是大写字符）。

> 字符类的完整列表可参考文档:help character-classes。

也可以显示地指定一个字符集合，供匹配时选择，语法是使用一对方括号[]。

比如，[A-Z0-9]匹配所有大写字母和数字，而[,4abc]只会匹配逗号、数字4和字母a、b、c

在字符集合中，可以用短横线-来指定一个范围。

[0-9A-Za-z_] 表示匹配字母、数字和下划线。

读取一个字符集合的补集，只需要在字符集合前面加上脱字符\^即可。如果要匹配所有非字符数字的符号，则可以使用字符集合[\^0-9A-Za-z]

#### 2 交替和分组

| 符号   | 含义 |
| ------ | ---- |
| \\|    | 交替 |
| \\(\\) | 分组 |

交替（alternation）操作起到“或”的作用，比如carrot\\|parrot同时匹配carrot和parrot

分组（grouping）用于将多个字符放在一个组里，好处是：

1 分组可以与其他正则表达式组合使用，比如`\(c\|p\)arrot`是一种同时匹配carrot和parrot的更精准的方式。

2 分组匹配到的字符串还可以在后面的替换中重用。如果希望将cat hunting mice替换成 mice hunting cat,则可以用替换命令`:s/\(cat\) hunting \(mice\)/\2 hungting \1/`。

分组是有助于代码重构的，比如，可用分组功能对函数的参数进行重排序。

#### 3 量词和重数

每个字符（无论是字面字符，还是特殊字符）或字符区间后面都可以接一个量词(quantifier)，在Vim中称为重数（multi）。

比如，`\w\+`匹配一个或多个单词字符，而`a\{2,4}`匹配2~4个连续的字符a（如aaa）。

一些常用的量词。

| 符号 | 含义    |
| ---- | ------- |
| *    | 0或多个 |
|      |         |
|      |         |
|      |         |
|      |         |
|      |         |

差不多到这行了

还不行，vim的markdown preview不更新了

还是用neovim好

## Neovim

安装方法可在GitHub仓库neovim/neovim中找到。

对于Debian系Linux，可

```
sudo apt-get install neovim  "安装
Python3 -m pip install neovim "实现Neovim对Python3的支持
```

Neovim启动命令为`nvim`

Neovim的配置文件格式和vim相同，不过.vimrc在Neovim中不会自动加载。

Neovim的配置文件遵守XDG基本目录结构，即所有的配置文件都放在\~/.config目录中。Neovim配置文件被保存在~/.config/nvim中。

* ~/.config/nvim/init.vim 对应于 ~/.vimrc
* ~/.config/nvim/对应与 ~/.vim

可以直接将Neovim的配置文件链接到Vim的配置文件。

```
$ mkdir -p $HOME/.config
$ ln -s $HOME/.vim $HOME/.config/nvim
$ ln -s $HOME/.vimrc $HOME/.config/nvim/init.vim
```

做好上述软连接后，Neovim就 可以加载原来的.vimrc文件了。

Windows系统中，Neovim的配置文件一般位于C:\Users\%USERNAME%\AppData\Local\nvim目录中。也可以像Linux中那样建立软连接。

```
$ mklink /D %USERPROFILE%\AppData\Local\nvim %USERPROFILE%\vimfiles
$ mklink %USERPROFILE%\AppData\Local\nvim\init.vim %USERPROFILE%\_vimrc
```

### 检查健康状态

Neovim欢迎界面中，会提示用户运行:checkhealth命令。

健康检查会汇报当前Neovim设置中的所有错误，并给出修复建议。

相比于Vim，Neovim有一个 非常好的特性，即启用某个功能或选项时不需要重新编译。

### 合理的默认选项

Neovim的默认选项与Vim有很大的不同。在现代的计算机世界里，文本编辑器的默认值需要对用户比较友好。默认情况下的Vim的.vimrc文件并不包含任何默认设置，而Neovim默认已经设置好语法高亮、合理的缩进设置、wildmenu、高亮显示搜索结果和增量搜索(insearch)等。

可通过查看:help nvim-defaults了解Neovim的默认选项。

如果想要同步Vim和Neovim的配置文件，最好在~/.vimrc中加入如下设置（然后再将其连接为~/.config/nvim/init.vim）

```
if !has('nvim')
  set nocompatible                             "与Vi不兼容
  filetype plugin indent on                    "对现在的插件是必须的
  syntax on                                    "语法高亮
  set autoindent                               "沿用上一行缩进
  set autoread                                 "从磁盘自动重载文件
  set backspace=indent,eol,start               "现代编辑器的退格键行为
  set belloff=all                              "禁用错误报警声
  set cscopeverbose                            "详细输出cscope结果
  set complete-=i                               "补全时，不要对当前被包含的文件进行扫描
  set display=lastline,msgsep                  "显示更多消息文本
  set encoding=utf-8                           "设置默认编码
  set fillchars=vert:|,fold:                   "分隔字符
  set formatoptions=tcqj                       "更直观的自动格式化
  set fsync                                    "调用fsync()实现更健壮的文件保存
  set history=10000                            "最大的历史记录数
  set hlsearch                                 "搜索结果高亮显示
  set incsearch                                "搜索时边输入边搜索、并移动光标
  set langnoremap                              "避免出现映射崩溃的情况
  set laststatus=2                             "总是显示状态栏
  set listchars=tab:>\,trail:-,nbsp:+          ":list时一些特殊字符的显示
  set nrformats=bin,hex                        "对<c-a>和<c-x>的支持
  set ruler                                    "在状态栏角落里显示当前行位置信息
  set sessionoptions-=options                  "不同会话不共享选项
  set shortmess=F                              "文件信息少显示一些
  set showcmd                                  "在状态栏中显示最后一条命令
  set sidescroll=1                             "更平滑的侧边滚动条
  set smarttab                                 "更只能的<Tab>键响应方式
  set tabpagemax=50                            "-p选项能够打开的最大数目的标签页
  set tags=./tags;,tags                        "用于搜索标签的那些文件名
  set ttimeoutlen=50                           "按键序列中等待下一个的时间，单位为毫秒
  set ttyfast                                  "要求实现快速的终端连接
  set viminfo+=!                               "为多个会话保存全局变量
  set wildmenu                                 "增强命令行补全功能
endif
  
```

set fillchars=vert:|,fold:  这个设置在nvim里有问题

以上注释较简短，更多信息查看响应:help

#### Oni

Oni是基于Neovim实现的跨平台图形用户界面编辑器，GitHub仓库为onivim/oni。

该编辑器为Neovim增加了集成开发环境（IDE）的功能，包括

* 内置浏览器
* 原生支持的自动补全和模糊搜索
* 一个命令菜单
* 多种教程
* 明显提升用户体验的其他功能
* 沿用Neovim的配置文件和键盘绑定

内置浏览器界面打开方式：  用`Ctrl+Shift+p`组合键打开命令菜单，再输入Browser

Oni完美继承了Vim的精髓，可以完全实现无鼠标操作（按`Ctrl+g`组合键再输入屏幕上提示的按键，可以访问页面上的任意元素）

#### Neovim高亮显示插件

Neovim大体上和Vim后向兼容，并且支持很多Vim插件（Powerline不支持）

不过，Neovim原生支持异步插件，并且提供了一些对开发者友好的功能，所以有很多插件只能在Neovim中运行。

下列插件（除了NyaoVim）可以移植到Vim中

* Dein(GitHub仓库Shougo/dein.vim)是一个异步插件管理器，类似于vim-plug。
* Denite（GitHub仓库为Shougo/denite.nvim）是一个模糊搜索插件，搜索范围广，包括缓冲区、当前文件中的行，甚至还可以是色调（基本是一个无所不能的CtrlP）。比如，可在当前文件中搜索关键字所在行号。
* NyaoVim（GitHub仓库rhysd/NyaoVim）是Neovim的一个跨平台的基于网络组件的图形界面插件。它的主要优点是可以将易于扩展和添加新的用户界面插件作为网络组件。
* Neomake（GitHub仓库neomake/neomake）是一个异步的语法检查器和编译器，它提供了一个针对不同文件类型的异步命令:Neomake。
* Neoterm（GitHub仓库kassio/neoterm）扩展了Vim/Neovim的终端功能,让它们更容易地在已有终端中运行命令。
* NCM2（GitHub仓库ncm2/ncm2）是Vim/Neovim的一个强大且可扩展的代码补全框架。
* gen_tags（GitHub仓库jsfaint/gen_tags.vim）是一个异步ctags/gtags生成器。gtags比ctags稍微强大一些，但支持的语言种类较少。

## 为什么用Neovim

* 使Vim代码库更容易管理
* 使用户和开发者更容易地添加功能和编写插件
* 鼓励外部应用程序与Neovim的整合













