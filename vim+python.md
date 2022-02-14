# 用Vim写Python

《程序员修炼之道》：最好是精通一种编辑器，并将其用于所有编辑任务。如果不坚持使用一种编辑器，可能会面临现代的巴别塔大混乱。

## 1 一键执行
这不是插件，而是一个自定义的vim配置
将下面的配置放到.vimrc文件即可：
```

""""""""""""""""""""""
    "Quickly Run
""""""""""""""""""""""
    map <F5> :call CompileRunGcc()<CR>
    func! CompileRunGcc()
        exec "w"
if &filetype == 'c'
            exec "!g++ % -o %<"
            exec "!time ./%<"
elseif &filetype == 'cpp'
            exec "!g++ % -o %<"
            exec "!time ./%<"
elseif &filetype == 'java'
            exec "!javac %"
            exec "!time java %<"
elseif &filetype == 'sh'
            :!time bash %
elseif &filetype == 'python'
            exec "!time python2.7 %"
elseif &filetype == 'html'
            exec "!firefox % &"
elseif &filetype == 'go'
    "        exec "!go build %<"
            exec "!time go run %"
elseif &filetype == 'mkd'
            exec "!~/.vim/markdown.pl % > %.html &"
            exec "!firefox %.html &"
endif
    endfunc
```
## 2 代码补全
用Ultisnips就行

## 3 语法检查(Syntastic)
snytastic插件，当你保存源文件时，它就会执行，并提示哪些代码存在语法错误，哪些代码风格不符合规范，并给出具体的提示。
例如，python代码风格默认设置为PEP8，即使你不知道PEP8风格，只要你使用syntastic插件，并根据它给出的提示修改，你就能写出完全符合PEP8风格的代码。

## 4 编程提示(jedi-vim)
jedi-vim是基于jedi的自动补全插件，比Syntastic智能，更贴切的称呼是“编程提示”，而不是代码补全。

如图所示：

![img](https://raw.githubusercontent.com/lunnche/picgo-image/main/202109251028872.jpeg)
这个插件是写vim的标配，真正让vim写python变成一件轻松愉快的事情。
注意：安装jedi-vim插件，需要在电脑中安装jedi，根据jedi-vim给出的提示，正常安装即可。
可能的问题：在虚拟机里安装以后不起作用，用下面命令更新vim即可
```
sudo aptitude install vim-gnome vim vim-common vim-tiny  
```
## 调试
用ipdb

