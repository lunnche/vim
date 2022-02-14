# Vim+Markdown

## 好笔记

* 录入文档、公式和课堂上在黑板上写板书一样快，不能接受延迟
* 画图也一样快
* 管理笔记，搜索知识点简单快捷

## Markdown插件

### 1 Vim-markdown

Markdown语法高亮插件，提供段落折叠，查看目录，段间跳转等功能

安装与命令

```vim
"安装插件
Plug 'godlygeek/tabular' "必要插件，安装在vim-markdown前面
Plug 'plasticboy/vim-markdown'
"查看所有配置建议
:help vim-markdwon
[[ "跳转上一个标题
]] "跳转下一个标题
]c "跳转到当前标题
]u "跳转到副标题
zr "打开下一级折叠
zR "打开所有折叠
zm "折叠当前段落
zM "折叠所有段落
:Toc "显示目录
```

由于还会使用到Latex数学公式，所以可以在配置文件中加上

```vim
let g:vim_markdown_math = 1
```

### 2 Vim-markdown-toc

自动在当前光标生成目录的插件

安装与命令

```vim
"安装插件
Plug 'mzlogin/vim-markdown-toc'
"在当前光标后生成目录
:GenTocMarked
"更新目录
:UpdateToc
"取消储存时自动更新目录
let g:vmt_auto_update_on_save = 0
```

自动生成的目录层级太多，可在在配置文件中写了一方程，自动找到较低的目录层级，然后删除之：

```vim
function RToc()
    exe "/-toc .* -->"
    let lstart=line('.')
    exe "/-toc -->"
    let lnum=line('.')
    execute lstart.",".lnum."g/           /d"
endfunction
```

### 3 Markdown-preview.nvim/markdown-preview.vim

[markdonw-preview.nvim](https://link.zhihu.com/?target=https%3A//github.com/iamcco/markdown-preview.nvim)（Neovim）和 [markdown-preview.vim](https://link.zhihu.com/?target=https%3A//github.com/iamcco/markdown-preview.vim) (Vim) 可以通过浏览器实时预览Markdown 文件。并可以借助浏览器的打印功能导出PDF文档。两个插件为同一个作者的作品，但是Neovim版本较新，更加完善，附带了Latex预览，Mermaid甘特图，Plantuml UML图等等一系列功能。Vim版本较旧，且已不再维护。所以推荐大家使用Neovim而非Vim。

```text
注：如果使用Vim并且想预览数学公式，还需要安装 mathjax-support-for-mkdp。
详情见 github.com/iamcco/markdown-preview.vim
```

安装与命令

```vim
" 安装插件
Plug 'iamcco/markdown-preview.nvim'
" 打开/关闭预览
:MarkdownPreviewToggel
" 指定浏览器路径
let g:mkdp_path_to_chrome = "path/of/chrome"
" 指定预览主题，默认Github
let g:mkdp_markdown_css=''
```

```
    MarkdownPreview
    " 在打开 markdown 文件后，使用该命令可以打开预览窗口

    MarkdownPreviewStop
    " 关闭 markdown 预览窗口，并停止开启的服务进程
```

**默认配置：**

```
 let g:mkdp_path_to_chrome = ""
    " 设置 chrome 浏览器的路径（或是启动 chrome（或其他现代浏览器）的命令）
    " 如果设置了该参数, g:mkdp_browserfunc 将被忽略

    let g:mkdp_browserfunc = 'MKDP_browserfunc_default'
    " vim 回调函数, 参数为要打开的 url

    let g:mkdp_auto_start = 0
    " 设置为 1 可以在打开 markdown 文件的时候自动打开浏览器预览，只在打开
    " markdown 文件的时候打开一次

    let g:mkdp_auto_open = 0
    " 设置为 1 在编辑 markdown 的时候检查预览窗口是否已经打开，否则自动打开预
    " 览窗口

    let g:mkdp_auto_close = 1
    " 在切换 buffer 的时候自动关闭预览窗口，设置为 0 则在切换 buffer 的时候不
    " 自动关闭预览窗口

    let g:mkdp_refresh_slow = 0
    " 设置为 1 则只有在保存文件，或退出插入模式的时候更新预览，默认为 0，实时
    " 更新预览

    let g:mkdp_command_for_global = 0
    " 设置为 1 则所有文件都可以使用 MarkdownPreview 进行预览，默认只有 markdown
    " 文件可以使用改命令

    let g:mkdp_open_to_the_world = 0
    " 设置为 1, 在使用的网络中的其他计算机也能访问预览页面
    " 默认只监听本地（127.0.0.1），其他计算机不能访问
```

**键位绑定：**

默认情况下，插件没有进行任何的按键绑定，如果想绑定按键去预览 markdown 文件，可以绑定 按键到`<Plug>MarkdownPreview`来打开预览窗口，绑定按键到`<Plug>StopMarkdownPreview`来 关闭预览窗口。

按键绑定例子（`F8`打开预览窗口，`F9`关闭预览窗口）：

```
nmap <silent> <F8> <Plug>MarkdownPreview        " 普通模式
imap <silent> <F8> <Plug>MarkdownPreview        " 插入模式
nmap <silent> <F9> <Plug>StopMarkdownPreview    " 普通模式
imap <silent> <F9> <Plug>StopMarkdownPreview    " 插入模式
```

让vim 识别 latex

```

Plug 'lervag/vimtex'

let g:tex_flavor= 'latex'

let g:vimtex_view_method= 'zathura'

let g:vimtex_quickfix_mode=0

set conceallevel=1

let g:tex_conceal= 'abdmg'
```



### Ultisnips片段

一个非常实用的Vim插件。**Snippets**译为片段，在这里表示可被某一段关键词触发并替换的一段短小的，可重复利用的文本块。

```text
注意！！！：ultisnips 需要python支持。

1. 如果使用的是vim，则需要在电脑安装相同编码，指定版本的python。 (2021年9月12是python3.7)
如果Vim是32位的，那么一定安装32位的python，否则vim无法识别。此插件极其依赖Python特定版本，一旦本地python版本有一丁点变动，整个vim的使用都会完全受阻

2. 如果使用的是Neovim，那么只要确保安装了python，然后在控制台使用

>> pip install neovim

就可以使用了。
```

安装与命令

```vim
"安装插件
Plug 'SirVer/ultisnips'

"设置tab键为触发键
let g:UltiSnipsExpandTrigger = '<tab>'
"设置向后跳转键
let g:UltiSnipsJumpForwardTrigger = '<tab>' 
"设置向前跳转键
let g:UltiSnipsJumpBackwardTrigger = '<S-tab>' 
"设置文件目录
let g:UltiSnipsSnippetDirectories=["path/of/snippetDirectories"]
"设置打开配置文件时为垂直打开
let g:UltiSnipsEditSplit="vertical"
```

### .snippets 文件

在使用 **vim-plug** 安装完 **ultisnips** 之后，可以在插件安装路径下找到文件夹 **ultisnips**。

按教材内容在ultisnips文件夹内新建Ultisnips文件夹,然后把markdown.snippets建在这里就能用，但我的机器不能用，我的机器智能识别ultisnips/snippets/中的markdown.snippets，但这样会导致如果在snippets中用了Python(开头引入global !p)就会报错，Ultisnips插件开发者在GitHub中issues中生命，不建议把markdown.snippets放在snippets文件夹中，因为这是为snipMate设置的引用方式，会出错。总之不推荐的方法我的电脑能行但不能在snippets中用python，推荐的方法我的电脑识别不了。转去尝试在windows上用Neovim，

发现neovim下也是不能识别

最后发现错误在于在.vimrc里设置了错误的`UltiSnipsSnippetDirectories`

成功的设置是这样的

```
Plug 'SirVer/ultisnips'
   let g:UltiSnipsSnippetDirectories=['Ultisnips']
```

==在.vim下新建了Ultisnips文件夹，把snippets直接放在这里==，而不是在.vim/plugged/下面新疆Ultisnips或snippets文件夹，前者导致识别不了，后者导致`global !p``<!DOCTYPE html>`之类报错，因它是snipMate的默认搜索文件夹不是ultisnips的会冲突。

是否必须直接放在.vim下尚未可知，网上有篇文章说：

***

***

***

这个插件很简单，给你看我的设置好了：

```
NeoBundle 'SirVer/ultisnips'
let g:UltiSnipsSnippetDirectories=['UltiSnips']
let g:UltiSnipsSnippetsDir = '~/.vim/UltiSnips'
let g:UltiSnipsExpandTrigger = '<Tab>'
let g:UltiSnipsListSnippets = '<C-Tab>'
let g:UltiSnipsJumpForwardTrigger = '<Tab>'
let g:UltiSnipsJumpBackwardTrigger = '<S-Tab>'
```

哦，对了，我想起来一件事情。`g:UltiSnipsSnippetDirectories` 选项的值必须是文件夹名称（可以是多个），并且这个（些）文件夹必须存在于 `runtimepath` 中的某一项之下。比如说 `~/.vim` 就是 `runtimepath` 中的一项。默认的文件夹 *UltiSnips* 会自动创建，如果你换了，那你必须保证这个文件夹是存在的。我看你换成了 `snippets`，如果你事先安装过 SnipMate，那么 `snippets` 才会存在，否则你得自行创建。`g:UltiSnipsSnippetDirectories` 选项的作用是指定 UltiSnips 的搜索路径，你找不到 snippets 的原因大概就是这样。

解释为什么错了真的很累，换个角度告诉你，如果你做对了是什么样子的：

1. `let g:UltiSnipsSnippetsDir = '~/.vim/UltiSnips'` 这个设置会确保 *snippets* 都在指定的文件夹内（你自己编辑的也会保存在这里，如果你用了第三方的并且还要进一步编辑，你得确保都复制到了这里）

![img](https://raw.githubusercontent.com/lunnche/picgo-image/main/202109202247132.png)

`let g:UltiSnipsSnippetDirectories = ['UltiSnips']` 这个设置会告诉 UltiSnips 去哪儿找 snippets，可以是多个地方，所以如果你用第三方的 snippets，和上一个设置不在一起的话，你得把它们的路径放这里。要注意的是，这个数组里的每一项都必须在 `runtimepath` 其中的一项之下，所以不确定的话，先看看 `runtimepath` 的值。

![17652d9fe152ed74388c89ce1ec967f3-1.png](https://raw.githubusercontent.com/lunnche/picgo-image/main/202109202249526.png)

若上述两点都做对了，那么在任意类型文件打开的前提下输入 `:UltiSnipsEdit` 都会打开对应类型的 snippets，能不能用，哪些能用，一看便知。

***

***

***

一个片段的格式一般为：

```text
snippet triggerWord "Comment" iAwrb
your snippets
endsnippet
```

其中 **triggerWord** 为关键词，**your snippets** 为自动输出的文本片段。

**iAwrb** 为snippet的选项。

\- **i** 表示片段可在句中被触发。默认是只有在前面有多个空格或者在行首时才会被触发。

\- **A** 表示片段会被自动触发

\- **w** 表示片段只会在关键词为单独单词的情况下被触发。若关键词为 **mk**, 那么只有在 **html mk** 时会被触发，**htmlmk** 不会被触发。

\- **r** 表示关键词使用正则表达式。正则表达式必须用 两个引号' '包围。比如 **\\'([1-9])day\'**。

\- **b** 表示只有在一行的开头才会被触发。

### Python 支持

使用 **ultisnips** 时最为方便的就是可以在片段中加入python脚本。用 \`!p \` 包围脚本，并使用 **snip.rv** 返回计算结果。加入了python可以使得输入更加智能便捷，并实现一些骚操作。例如

```text
# 在数学模式下自动加下标
context "math()"
snippet '([A-Za-z])(\d)' "auto subscript" wrA
`!p snip.rv = match.group(1)`_`!p snip.rv = match.group(2)`
endsnippet
```

或者

```text
# 根据用户输入新建一个Markdown格式的表格
snippet '(?<!\\)([0-9])([0-9])tb' "new table" r
$1`!p 
x=match.group(1)
y=match.group(2)
row1=""
row2="" 
for i in range(int(x)):
    row1+="| "
    row2+="|:-:"
row1+="|\n"
row2+="|\n"
out=row1+row2+int(y)*row1
snip.rv=out
`$0
endsnippet
```



### Latex数学公式

### **♣** Inline & Display

在Markdown中，Latex数学公式可分为 inline 模式和 Display 模式。inline 模式关键词为 \$\$, Display 模式关键词为一头\$一尾\$。因此，沿袭 [Gilles Castel](https://link.zhihu.com/?target=https%3A//castel.dev/) 大佬的习惯，有如下snippets：

```text
# mk 表示 make Ketax
snippet mk "Math" wA
$${1}$`!p
if t[2] and t[2][0] not in [',', '.', '?', '-', ' ']:
    snip.rv = ' '
else:
    snip.rv = ''
`$2
endsnippet

# dm 表示 Display math
snippet dm "Math" wA
$$
${1:${VISUAL}}
$$ $0
endsnippet
```

### **♣** 分数，希腊字母

Latex语法下的分数为 **\frac{}{}** , 输入较为费劲。使用 snippets 能使这个过程简便很多。使用正则表达式与python可以很好的识别出是否为数字，是否被括号包裹等等。类似的应用还有诸如希腊字母的快速输入，快速输入上下标，快速输入积分、级数等等。例如：

```text
# 若输入 ‘/’，则检查符号前的字符是否为数字或者字母，
# 将数字或字母作为分子扩展为Latex分数形式然后在分母部分等待输入
context "math()"
priority 1000
snippet '((\d+)|(\d*)(\\)?([A-Za-z]+)((\^|_)(\{\d+\}|\d))*)/' "Fraction" wrA
\\frac{`!p snip.rv = match.group(1)`}{$1}$0
endsnippet
```

熟练使用Ultisnips可以大大加快输入速度，在输入Latex时可以缩减输入时间，让人更加专注于数学公式，而不是去思考Latex这个语言。

更多的Snippet代码  见markdown.snippets文件

```
###############
##Python Util##
###############
global !p
texMathZones = ['texMathZone'+x for x in ['A', 'AS', 'B', 'BS', 'C', 'CS', 'D', 'DS', 'E', 'ES', 'F', 'FS', 'G', 'GS', 'H', 'HS', 'I', 'IS', 'J', 'JS', 'K', 'KS', 'L', 'LS', 'DS', 'V', 'W', 'X', 'Y', 'Z', 'XX']]
texIgnoreMathZones = ['texMathText']
texMathZoneIds = vim.eval('map('+str(texMathZones)+", 'hlID(v:val)')")
texIgnoreMathZoneIds = vim.eval('map('+str(texIgnoreMathZones)+", 'hlID(v:val)')")
ignore = texIgnoreMathZoneIds[0]
def math():
	synstackids = vim.eval("synstack(line('.'), col('.') - (col('.')>=2 ? 1 : 0))")
	try:
		first = next(i for i in reversed(synstackids) if i in texIgnoreMathZoneIds or i in texMathZoneIds)
		return first != ignore
	except StopIteration:
		return False
		
def nomath():
	synstackids = vim.eval("synstack(line('.'), col('.') - (col('.')>=2 ? 1 : 0))")
	try:
		first = next(i for i in reversed(synstackids) if i in texIgnoreMathZoneIds or i in texMathZoneIds)
		return first == ignore
	except StopIteration:
		return True
endglobal


#############
### Math ####
#############
snippet mk "Math" wA
$${1}$`!p
if t[2] and t[2][0] not in [',', '.', '?', '-', ' ']:
	snip.rv = ' '
else:
	snip.rv = ''
`$2
endsnippet

snippet dm "Math" wA
$$
${1:${VISUAL}}
$$ $0
endsnippet

context "math()"
snippet => "implies" Ai
\implies
endsnippet

context "math()"
snippet =< "implied by" Ai
\impliedby
endsnippet

context "math()"
snippet iff "iff" Ai
\iff
endsnippet

context "math()"
snippet ali "Align" bA
\begin{aligned}
	${1:${VISUAL}}
\end{aligned}
endsnippet

context "math()"
snippet // "Fraction" iA	
\\frac{$1}{$2}$0
endsnippet

context "math()"
snippet / "Fraction" iA
\\frac{${VISUAL}}{$1}$0
endsnippet

context "math()"
priority 1000
snippet '((\d+)|(\d*)(\\)?([A-Za-z]+)((\^|_)(\{\d+\}|\d))*)/' "Fraction" wrA
\\frac{`!p snip.rv = match.group(1)`}{$1}$0
endsnippet

context "math()"
snippet '^.*\)/' "() frac" wrA
`!p
stripped = match.string[:-1]
depth = 0
i = len(stripped) - 1
while True:
	if stripped[i] == ')': depth += 1
	if stripped[i] == '(': depth -= 1
	if depth == 0: break;
	i-=1
snip.rv = stripped[0:i] + "\\frac{" + stripped[i+1:-1] + "}"
`{$1}$0
endsnippet

context "math()"
snippet '([A-Za-z])(\d)' "auto subscript" wrA
`!p snip.rv = match.group(1)`_`!p snip.rv = match.group(2)`
endsnippet

context "math()"
snippet '([A-Za-z])_(\d\d)' "auto subscript2" wrA
`!p snip.rv = match.group(1)`_{`!p snip.rv = match.group(2)`}
endsnippet

context "math()"
snippet == "equals"
&= $1 \\\\
endsnippet

context "math()"
snippet sr "^2" 
^2
endsnippet

context "math()"
snippet cb "^3" 
^3
endsnippet

context "math()"
snippet td "to the ... power"  Ai
^{$1}$0
endsnippet

context "math()"
snippet rd "to the ... power" 
^{$1}$0
endsnippet

context "math()"
snippet __ "subscript" Ai
_{$1}$0
endsnippet

priority 100
context "math()"
snippet ^^ "upper" Ai
^{$1}$0
endsnippet

context "math()"
snippet ooo "\infty" 
\infty
endsnippet

context "math()"
snippet rij "mrij" i
(${1:x}_${2:n})_{${3:$2}\\in${4:\\N}}$0
endsnippet

context "math()"
snippet <= "leq" 
\le 
endsnippet

context "math()"
snippet >= "geq" 
\ge 
endsnippet

context "math()"
snippet EE "geq" 
\exists 
endsnippet

context "math()"
snippet AA "forall" 
\forall 
endsnippet

context "math()"
snippet xnn "xn" 
x_{n}
endsnippet

context "math()"
snippet ynn "yn" 
y_{n}
endsnippet


context "math()"
snippet xii "xi" 
x_{i}
endsnippet

context "math()"
snippet yii "yi" 
y_{i}
endsnippet

context "math()"
snippet xjj "xj" 
x_{j}
endsnippet

context "math()"
snippet yjj "yj" 
y_{j}
endsnippet

context "math()"
snippet xp1 "x" 
x_{n+1}
endsnippet

context "math()"
snippet xmm "x" 
x_{m}
endsnippet


context "math()"
snippet mcal "mathcal" A
\mathcal{$1}$0
endsnippet

snippet lll "l"
\ell
endsnippet

context "math()"
snippet nabl "nabla" 
\nabla 
endsnippet

context "math()"
snippet xx "cross" 
\times 
endsnippet

context "math()"
snippet norm "norm"
\|$1\|$0
endsnippet

priority 100
context "math()"
snippet '(?<!\\)(cdot|sin|cos|arccot|cot|csc|ln|log|exp|star|perp)' "ln" rwA
\\`!p snip.rv = match.group(1)` 
endsnippet

priority 300
context "math()"
snippet dint "integral" wA
\int_{${1:-\infty}}^{${2:\infty}} ${3:${VISUAL}} $0
endsnippet

priority 200
context "math()"
snippet '(?<!\\)(arcsin|arccos|arctan|arccot|arccsc|arcsec|pi|zeta|int)' "ln" rwA
\\`!p snip.rv = match.group(1)`
endsnippet


priority 100
context "math()"
snippet -> "to" iA
\to 
endsnippet

priority 200
context "math()"
snippet <-> "leftrightarrow" 
\leftrightarrow
endsnippet

context "math()"
snippet !> "mapsto" 
\mapsto 
endsnippet

context "math()"
snippet invs "inverse" 
^{-1}
endsnippet

context "math()"
snippet compl "complement" 
^{c}
endsnippet

context "math()"
snippet \\\ "setminus" 
\setminus
endsnippet

context "math()"
snippet >> ">>" w
\gg
endsnippet

context "math()"
snippet << "<<" w
\ll
endsnippet

context "math()"
snippet set "set" wA
\\{$1\\} $0
endsnippet

context "math()"
snippet || "mid" 
 \mid 
endsnippet

context "math()"
snippet cc "subset" Ai
\subset 
endsnippet

snippet notin "not in " 
\not\in 
endsnippet

context "math()"
snippet inn "in " 
\in 
endsnippet

context "math()"
snippet NN "n" 
\N
endsnippet

context "math()"
snippet Nn "cap" 
\cap 
endsnippet

context "math()"
snippet UU "cup" 
\cup 
endsnippet

context "math()"
snippet uuu "bigcup" 
\bigcup_{${1:i \in ${2: I}}} $0
endsnippet

context "math()"
snippet nnn "bigcap" 
\bigcap_{${1:i \in ${2: I}}} $0
endsnippet

context "math()"
snippet OO "emptyset" 
\O
endsnippet

context "math()"
snippet RR "real" 
\R
endsnippet

context "math()"
snippet QQ "Q" 
\Q
endsnippet

context "math()"
snippet ZZ "Z" 
\Z
endsnippet

context "math()"
snippet <! "normal" 
\trngleleft 
endsnippet

context "math()"
snippet <> "hokje" 
\dmond 
endsnippet


context "math()"
snippet '(?<!i)sts' "text subscript" irA
_\text{$1} $0
endsnippet

context "math()"
snippet tt "text" 
\text{$1}$0
endsnippet

context "math()"
snippet case "cases" wA
\begin{cases}
	$1
\end{cases}
endsnippet

context "math()"
snippet SI "SI" 
\SI{$1}{$2}
endsnippet

snippet cvec "column vector" 
\begin{pmatrix} ${1:x}_${2:1}\\\\ \vdots\\\\ $1_${2:n} \end{pmatrix}
endsnippet

priority 10
context "math()"
snippet "bar" "bar" r
\overline{$1}$0
endsnippet

priority 100
context "math()"
snippet "([a-zA-Z])bar" "bar" r
\overline{`!p snip.rv=match.group(1)`}
endsnippet

priority 10
context "math()"
snippet "hat" "hat" r
\hat{$1}$0
endsnippet

priority 100
context "math()"
snippet "([a-zA-Z])hat" "hat" r
\hat{`!p snip.rv=match.group(1)`}
endsnippet

context "math()"
snippet HH "H" 
\mathbb{H}
endsnippet

context "math()"
snippet DD "D" 
\mathbb{D}
endsnippet

context "math()"
snippet != "unequals" 
\neq 
endsnippet

context "math()"
snippet ceil "ceil" 
\left\lceil $1 \right\rceil $0
endsnippet

context "math()"
snippet floor "floor" 
\left\lfloor $1 \right\rfloor$0
endsnippet

snippet pmat "pmat" 
\begin{pmatrix} $1 \end{pmatrix} $0
endsnippet

context "math()"
snippet bmat "bmat" 
\begin{bmatrix} $1 \end{bmatrix} $0
endsnippet

context "math()"
snippet () "left( right)" 
\left( ${1:${VISUAL}} \right) $0
endsnippet


snippet lra "leftangle rightangle" 
\left<${1:${VISUAL}} \right>$0
endsnippet

context "math()"
snippet conj "conjugate" 
\overline{$1}$0
endsnippet

context "math()"
snippet sum "sum" w
\sum_{n=${1:1}}^{${2:\infty}} ${3:a_n z^n}
endsnippet

context "math()"
snippet taylor "taylor" w
\sum_{${1:k}=${2:0}}^{${3:\infty}} ${4:c_$1} (x-a)^$1 $0
endsnippet

context "math()"
snippet lim "limit" w
\lim_{${1:n} \to ${2:\infty}} 
endsnippet

context "math()"
snippet limsup "limsup" w
\limsup_{${1:n} \to ${2:\infty}} 
endsnippet

context "math()"
snippet prod "product" w
\prod_{${1:n=${2:1}}}^{${3:\infty}} ${4:${VISUAL}} $0
endsnippet

context "math()"
snippet part "d/dx" w
\frac{\partl ${1:V}}{\partl ${2:x}} $0
endsnippet

context "math()"
snippet sq "\sqrt{}" 
\sqrt{${1:${VISUAL}}} $0
endsnippet

context "math()"
snippet ~ "~"
\sim 
endsnippet

context "math()"
snippet ~~ "approx"
\approx
endsnippet

context "math()"
snippet != "nq" 
\neq
endsnippet

context "math()"
snippet +- "pm" wA
\pm
endsnippet

priority 100
context "math()"
snippet '(?<!\\)(alpha|beta|gamma|delta|Delta|epsilon|varepsilon|zeta|eta|theta|Theta|vartheta|mu|nu|xi|pi|rho|sigma|tau|phi|Phi|varphi|omega|Omega|chi)' "greece" rwA
\\`!p snip.rv = match.group(1)`
endsnippet


priority 100
context "math()"
snippet '(?<!\\)(vec)' "latexmath" rwA
\\`!p snip.rv = match.group(1)`{$0}
endsnippet

############
## Others ##
############
snippet date "YYYY-MM-DD" w
`!v strftime("%Y-%m-%d")`
endsnippet

snippet ddate "Month DD, YYYY" w
`!v strftime("%b %d, %Y")`
endsnippet

snippet diso "ISO format datetime" w
`!v strftime("%Y-%m-%d %H:%M:%S%z")`
endsnippet

snippet time "hh:mm" w
`!v strftime("%H:%M")`
endsnippet

snippet datetime "YYYY-MM-DD hh:mm" w
`!v strftime("%Y-%m-%d %H:%M")`
endsnippet

##############
## Markdown ##
##############
snippet title "title"
<head><style>div.solid {border-style:solid;}</style></head>
<b><font size="7">$1</font></b>

------

$0
endsnippet

snippet newpage "New Page"
<div style="page-break-after: always;"></div>
$0
endsnippet

snippet sp
&emsp;
endsnippet

context "nomath()"
snippet { "{}" A
{$1}$0
endsnippet

context "nomath()"
snippet [ "[]" A
[$1]$0
endsnippet

context "nomath()"
snippet ( "()" A
($1)$0
endsnippet

priority 80
context "nomath()"
snippet + "*" Ai
*$1*$0
endsnippet

priority 80
context "nomath()" 
snippet **a "+" i
+
endsnippet

snippet fig "Figure environment" bw
![$1](pic/$1.png)
$0
endsnippet

snippet figt "Figure with title" b
<center>
    <img style="border-radius: 0.2125em;" src="pic/$1.png";size="50">
    <div style="
    display: outline;
    color: #666;
    padding: 2px;">$2 </div>
</center>
$0
endsnippet

snippet table "table" 
|$1|$2|
|:-|:-:|
|$3|$0|
endsnippet

snippet msp "4 space" w
&emsp; &emsp; &emsp; &emsp;
endsnippet

snippet deg "degree" w
&deg;$C$
endsnippet

snippet dia "diamond" w
&diams;
endsnippet

snippet link  "hlink" w
[$1]($2)$0 
endsnippet

snippet club "spade"
$\clubsuit$
endsnippet

snippet lrn "left( right)" iAw
($1)$0
endsnippet

snippet lrb "left\{ right\}" iAw
{$1}$0
endsnippet

snippet lre "left[ right]" iAw
[$1]$0
endsnippet

snippet '(?<!\\)([0-9])([0-9])tb' "definele" r
$1`!p 
x=match.group(1)
y=match.group(2)
row1=""
row2="" 
for i in range(int(x)):
	row1+="| "
	row2+="|:-:"
row1+="|\n"
row2+="|\n"
out=row1+row2+int(y)*row1
snip.rv=out
`$0
endsnippet

snippet box "box" w
<div class="solid">
$1
</div>
$0
endsnippet

# vim:ft=snippets
```



markdown 图床 用picgo (windows下设置)

picgo设置

* 打开更新助手 开
* 开机自启 开
* 时间戳重命名 开
* 上传后自动复制URL 开
* 选择显示的图床 GitHub图床

然后markdown里设置

图像

插入图片时

上传图片

对本地位置的图片应用上述规则  √

对网络位置的图片应用上述规则 √

插入时自动转义图片URL √

上传服务 PicGo(app)

PicGo路径 。。



![image-20210912202318883](https://raw.githubusercontent.com/lunnche/picgo-image/main/202109122023934.png)



#### ubuntu20.04 + picgo + markdown

在linux环境下配置markdown，使得贴入图片自动将图片上传到github，使得换计算机打开这个md文件照样能看到图片

#### 安装nodejs、npm、picgo

```
$ sudo apt-get install nodejs npm
$ sudo npm install picgo -g
```

然后 输入`which picgo`可确认picgo的安装路径。



#### 选择图床

github

#### 生成秘密令牌

github进入设置，选择开发者设置

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

#### 配置picgo

```
$ picgo set uploader
? Choose a(n) uploader (Use arrow keys)
> smms
  tcyun
  github
  qiniu
  imgur
  aliyun
  upyun
```

选择github，

```
"repo": "lunnche/picgo-image",
"branch": "main",
"token": "你的token",
"path": "",           空着
"customUrl": ""       空着
```

选了github后，config.json里图床还是smms没有变，手动去改下~/.picgo/config.json

#### 配置Typora

picgo.AppImage里设置（这一步好像不做也行，用的picgo-core）

* 打开更新助手 开
* 开机自启 开
* 时间戳重命名 开
* 上传后自动复制URL 开
* 选择显示的图床 GitHub图床

然后markdown里设置

图像

插入图片时

上传图片

对本地位置的图片应用上述规则  √

对网络位置的图片应用上述规则 √

插入时自动转义图片URL √

上传服务 (Image Uploader)选择"Custom Command"

command 输入 `[your node path][your picgo-core path ] upload`  可用命令`which picgo`查前面的路径，我的是`/usr/local/bin/picgo upload` ,如果你的“node”和“picgo”都在系统路径里，那么command可直接输入“picgo upload”



## 在linux下结合picgo-core和typora自动上传图片至github

## 下载 PicGo-Core

```
$ npm install picgo -g
# or
$ yarn global add picgo
```

## 获取路径

- node安装路径（`which node`）：`/usr/local/bin/node`
- picgo安装路径（`which picgo`）：`/usr/local/bin/picgo`

### Imgae Upldoad Setting

> 打开 Typora -> 偏好设置 -> 图像：

上传服务选择“Custom Command”，自定义命令格式是 “[your node path] [your picgo-core path] upload”，比如可能是 `/usr/local/bin/node /usr/local/bin/picgo upload`



![img](https://raw.githubusercontent.com/lunnche/picgo-image/main/171638ed41ea7fb9%7Etplv-t2oaga2asx-watermark.awebp)

picgo 的默认配置文件为`~/.picgo/config.json`。其中`~`为用户目录。不同系统的用户目录不太一样。

linux 和 macOS 均为`~/.picgo/config.json`。

windows 则为`C:\Users\你的用户名/.picgo\config.json`。

配置文件需要至少有如下的配置项：

```
{
  "picBed": {
    "current": "github",
    "github": {
      "repo": "lunnche/picgo-image",
      "branch": "main",
      "token": "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",
      "path": "",
      "customUrl": ""
    }
  },
  "picgoPlugins": {}
}
```

## 验证

点击**验证图片上传选项** 按钮



![image-20200410180446932](https://raw.githubusercontent.com/lunnche/picgo-image/main/171638ed423f33b8%7Etplv-t2oaga2asx-watermark.awebp)

