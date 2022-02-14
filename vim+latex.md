## vim+latex

测试LaTeX语法的网站 https://latex.codecogs.com/eqneditor/editor.php

一份（不太）简短的LaTeX 介绍
https://github.com/CTeX-org/lshort-zh-cn

画图用Inkscape

屏幕布局：左边vim，右边pdf viewer（Zathura）

Zathura也支持Vim-like keybindings

使用Ubuntu+bspwm 作为窗口管理器

LaTeX插件用vimtex，用vim-plug安装

```
Plug 'lervag/vimtex'
let g:tex_flavor='latex'
let g:vimtex_view_method='zathura'
let g:vimtex_quickfix_mode=0
set conceallevel=1
let g:tex_conceal='abdmg'
```

最后两行是隐藏配置，意思是当光标不在代码所在行时，代码不展现或展现为其他。

例如`\[`，`\]`，$将变为不可见。`\bigcap`将变为∩，`\in`变成∈。（需要在.tex之类文件中才能生效）

> 安装vimtex插件，在vim里适用：PlugInstall时报错` OpenSSL SSL_connect: Connection reset by peer in connection to github.com:443`
>
> 解决办法：
>
> 我开了VPN，出错是因为代理问题，设置下proxy就可以了，
>
> 查看自己的VPN端口号，我的端口号是1080，在git bash命令行中输入一下命令即可
>
> ```
> $ git config --global http.proxy 127.0.0.1:1080
> $ git config --global https.proxy 127.0.0.1:1080
> ```
>
> 到此即可
>
> 题外话：
>
> 如果之前git中设置过上述配置，则适用如下命令取消后再进行配置即可：
>
> ```
> git config --global --unset http.proxy
> git config --global --unset https.proxy
> ```
>
> 下面是几个常用的git配置查看命令：
>
> ```
> git config --global http.proxy #查看git的http代理配置
> git config --global https.proxy #查看git的https代理配置
> git config --global -1 #查看git的所有配置
> ```

#### Snippets

短小可复用的文本，输入后按Tab转成其他预设文本。

snippets也可以是动态的，如：输入today按Tab，转为当前日期，输入box按Tab转为一个长度随录入增长的文本框。

snippets可以递归

#### 使用Using UltiSnips to create snippets

使用插件UltiSnips

```
Plug 'sirver/ultisnips'
let g:UltiSnipsExpandTrigger = '<tab>'
let g:UltiSnipsJumpForwardTrigger = '<tab>'
let g:UltiSnipsJumpBackwardTrigger = '<s-tab>'
```

例如，设置sign的snippet

```
snippet sign "Signature"
Yours sincerely,

Gilles Castel
endsnippet
```

对于动态snippets，把代码放在\`\`（注意是反引号）之间，比如这里用bash来生产当前的日期：`date + %F`

```
snippet today "Date"
`date +%F`
endsnippet
```

也可以用python来生成snippets，把代码放在`!p...`中，比如box的snippet

```
snippet box "Box"
`!p snip.rv = '┌' + '─' * (len(t[1]) + 2) + '┐'`
│ $1 │
`!p snip.rv = '└' + '─' * (len(t[1]) + 2) + '┘'`
$0
endsnippet
```

这个Python代码段将会替换成`snip.rv`的值。

`t[1]`包含了第一个tab stop，`fn`包含当前文件名。

#### LaTeX snippets

用snippets将大大加快latex书写速度

#### Environments

插入一个环境，只需要输入`beg`，然后录入环境名，`\end{}`里的环境名会随之同时生成。按Tab将把光标放置在新生产的环境里。代码如下:

```
snippet beg "begin{} / end{}" bA
\begin{$1}
	$0
\end{$1}
endsnippet
```

`b`表示只会在一行开始的地方进行扩展。

`A`表示自动扩展，即不用按Tab。

Tab stops是指你按`Tab`和`Shift`+`Tab`能跳转到的地方，用`$1`，`$2`表示，最后一个用`$0`表示。

#### Inline and display math

`mk`和`dm`是最常用的snippets.前者代表行内数学，后者displayed math。

行内数学snippet是'smart'的，它知道啥时该在\$后加空格，你在\$后输入一个word，就会自动出现空格，输入非word字符（如-）就不会出现空格。如`$p$-value`

代码如下

```
snippet mk "Math" wA
$${1}$`!p
if t[2] and t[2][0] not in [',', '.', '?', '-', ' ']:
    snip.rv = ' '
else:
    snip.rv = ''
`$2
endsnippet
```

第一行结尾w表示snippet将在word边界展开，所以`hellomk`不会展开，`hello mk`会展开。

displayed math，代码如下

```
snippet dm "Math" wA
\[
$1
.\] $0
endsnippet
```

#### Sub-and superscripts

subscripts snippet 把`a1`变`a_1`，把`a_12`变`a_{12}`

它是用正则表达式实现的。这个snippet在两种情况下展开：一个字符后跟一个数字，wich encoded by `[A-Za-z]\d`，或者一个字符后跟_和两个数字，`[A-Za-z]_\d\d`

```
snippet '([A-Za-z])(\d)' "auto subscript" wrA
`!p snip.rv = match.group(1)`_`!p snip.rv = match.group(2)`
endsnippet

snippet '([A-Za-z])_(\d\d)' "auto subscript2" wrA
`!p snip.rv = match.group(1)`_{`!p snip.rv = match.group(2)`}
endsnippet
```

When you wrap parts of a regular expression in a group using parenthesis,e.g.`(\d\d)`,you can use them in the expansion of the snippet via `match.group(i)` in Python.

上标，使用`td`，转换成`^{}`，squared用`sr`，cubed用`cb`，complement用`comp`

```
snippet sr "^2" iA
^2
endsnippet

snippet cb "^3" iA
^3
endsnippet

snippet compl "complement" iA
^{c}
endsnippet

snippet td "superscript" iA
^{$1}$0
endsnippet
```

#### 分数

```
//	→	\frac{}{}
3/	→	\frac{3}{}
4\pi^2/	→	\frac{4\pi^2}{}
(1 + 2 + 3)/	→	\frac{1 + 2 + 3}{}
(1+(2+3)/)	→	(1 + \frac{2+3}{})
(1 + (2+3))/	→	\frac{1 + (2+3)}{}
```

The code for the first one is easy:

```snip
snippet // "Fraction" iA
\\frac{$1}{$2}$0
endsnippet
```

The sec­ond and third ex­am­ples are made pos­si­ble using reg­u­lar ex­pres­sions to match for ex­pres­sions like `3/`, `4ac/`, `6\pi^2/`, `a_2/`, etc.

```snip
snippet '((\d+)|(\d*)(\\)?([A-Za-z]+)((\^|_)(\{\d+\}|\d))*)/' "Fraction" wrA
\\frac{`!p snip.rv = match.group(1)`}{$1}$0
endsnippet
```

![image-20210916165754774](https://raw.githubusercontent.com/lunnche/picgo-image/main/image-20210916165754774.png)

第4，第5个例子中，尝试去找出匹配的括号，用UItiSnips的正则表达式无法做到，所以使用Python:

```python
priority 1000
snippet '^.*\)/' "() Fraction" wrA
`!p
stripped = match.string[:-1]
depth = 0
i = len(stripped) - 1
while True:
	if stripped[i] == ')': depth += 1
	if stripped[i] == '(': depth -= 1
	if depth == 0: break;
	i -= 1
snip.rv = stripped[0:i] + "\\frac{" + stripped[i+1:-1] + "}"
`{$1}$0
endsnippet
```

最后这个和分数有关的snippet，使用你的选择来创建分数。选择一段文本，按`Tab`，输入`/`，然后再按`Tab`

代码使用了`${VISUAL}变量来表示你的选择

```snip
snippet / "Fraction" iA
\\frac{${VISUAL}}{$1}$0
endsnippet
```

#### Sympy and Mathematica

另一个cool但用的相对少的snippet 是使用sympy来表示数学表达式，例如：`sympy``Tab`扩展为`sympy | sympy`，`sympy 1 + 1 sympy``Tab`扩展为`2`

```snip
snippet sympy "sympy block " w
sympy $1 sympy$0
endsnippet

priority 10000
snippet 'sympy(.*)sympy' "evaluate sympy" wr
`!p
from sympy import *
x, y, z, t = symbols('x y z t')
k, m, n = symbols('k m n', integer=True)
f, g, h = symbols('f g h', cls=Function)
init_printing()
snip.rv = eval('latex(' + match.group(1).replace('\\', '') \
    .replace('^', '**') \
    .replace('{', '(') \
    .replace('}', ')') + ')')
`
endsnippet
```

对于Mathematica的使用者来说，可以做同样的事:

```snip
priority 1000
snippet math "mathematica block" w
math $1 math$0
endsnippet

priority 10000
snippet 'math(.*)math' "evaluate mathematica" wr
`!p
import subprocess
code = 'ToString[' + match.group(1) + ', TeXForm]'
snip.rv = subprocess.check_output(['wolframscript', '-code', code])
`
endsnippet
```

#### Postfix snippets

例如：`phat`->`\hat{p}`and`zbar`->`\overline{z}`.

类似的snippet是postfix vector，如`v,.`->`\vec{v}`

`v.,`->`\vec{v}` 

 ，和.的顺序不重要。所以可以同时按下它们。



注意我依然能使用`bar`和`hat`前缀，因为给它们设置了较低的优先级。

```snip
priority 10
snippet "bar" "bar" riA
\overline{$1}$0
endsnippet

priority 100
snippet "([a-zA-Z])bar" "bar" riA
\overline{`!p snip.rv=match.group(1)`}
endsnippet
```

```snip
priority 10
snippet "hat" "hat" riA
\hat{$1}$0
endsnippet

priority 100
snippet "([a-zA-Z])hat" "hat" riA
\hat{`!p snip.rv=match.group(1)`}
endsnippet
```

```snip
snippet "(\\?\w+)(,\.|\.,)" "Vector postfix" riA
\vec{`!p snip.rv=match.group(1)`}
endsnippet 
```

#### 其它snippet

其它常用的snippet在这里：https://github.com/gillescastel/latex-snippets ，它们大多非常简单，例如，`!>`变为`\mapsto`,`->`变为`\to`

`fun`变为`f: \R \to \R :`,`!>`->`\mapsto`,`->`变为`\to`,`cc`->`\subset`

`lim`变为`\lim_{n \to \infty}`,`sum`变为`\sum_{n = 1}^{\infty}`,`ooo`变为`\infty`

#### Course specific snippets

除了常用snippets，还有course specific snippets.想要加载它们需要在.vimrc中添加：

```vim
set rtp+=~/current_course
```

`current_course`是一个symlink，连接到我当前激活的course，在那个文件夹中，我有个文件`~/current_course/UltiSnips/tex.snippets`其中包含了course specific snippets。比如，对于量子物理，有适用于bra/ket记法的snippets.

```
<a|	→	\bra{a}
<q|	→	\bra{\psi}
|a>	→	\ket{a}
|q>	→	\ket{\psi}
<a|b>	→	\braket{a}{b}
```

由于`\psi`在量子物理里使用很多，把所有带尖括号的q转化为`\psi`

```snip
snippet "\<(.*?)\|" "bra" riA
\bra{`!p snip.rv = match.group(1).replace('q', f'\psi').replace('f', f'\phi')`}
endsnippet

snippet "\|(.*?)\>" "ket" riA
\ket{`!p snip.rv = match.group(1).replace('q', f'\psi').replace('f', f'\phi')`}
endsnippet

snippet "(.*)\\bra{(.*?)}([^\|]*?)\>" "braket" riA
`!p snip.rv = match.group(1)`\braket{`!p snip.rv = match.group(2)`}{`!p snip.rv = match.group(3).replace('q', f'\psi').replace('f', f'\phi')`}
endsnippet
```

#### Context

单词disregard中的sr不该被转换为`^2`,从而输`dis^2egard`

解决方法是为snippets添加context，利用Vim的语法高亮，它能决定依据对象是数学还是文本，UltiSnips是否该转换snippet，把下面这段代码加到你snippets文件的最前面：

```python
global !p
def math():
    return vim.eval('vimtex#syntax#in_mathzone()') == '1'

def comment(): 
    return vim.eval('vimtex#syntax#in_comment()') == '1'

def env(name):
    [x,y] = vim.eval("vimtex#env#is_inside('" + name + "')") 
    return x != '0' and x != '0'

endglobal
```

现在你可以把`context "math()"`加入snippets, 你只想在数学上下文环境中转换`sr`为`^2`

```snip
context "math()"
snippet sr "^2" iA
^2
endsnippet
```

mathematical context是个微妙的东西。Some­times you add some text in­side a math en­vi­ron­ment by using `\text{...}`. In that case, you do not want snip­pets to ex­pand. How­ev­er, in the fol­low­ing case: `\[ \text{$...$} \]`, they *should* ex­pand.

Sim­i­lar­ly, you can add `context "env('itemize')"` to snip­pets that only should ex­pand in an `itemize` en­vi­ron­ment or `context "comment()"` for snip­pets in com­ments.

#### Correcting spelling mistakes on the fly

更正拼写错误而不打断工作流。打字时按`Ctrl+L`，之前的拼写错误就会被自动更正

```vim
setlocal spell
set spelllang=nl,en_gb
inoremap <C-l> <c-g>u<Esc>[s1z=`]a<c-g>u
```

It ba­si­cal­ly jumps to the pre­vi­ous spelling mis­take `[s`, then picks the first sug­ges­tion `1z=`, and then jumps back ``]a`. The `<c-g>u` in the mid­dle make it pos­si­ble to undo the spelling cor­rec­tion quick­ly.

