*pymode.txt*     For Vim Version 8.0      Last change: 2017 November 20

     ____  _  _  ____  _   _  _____  _  _     __  __  _____  ____  ____      ~
    (  _ \( \/ )(_  _)( )_( )(  _  )( \( )___(  \/  )(  _  )(  _ \( ___)     ~
     )___/ \  /   )(   ) _ (  )(_)(  )  ((___))    (  )(_)(  )(_) ))__)      ~
    (__)   (__)  (__) (_) (_)(_____)(_)\_)   (_/\/\_)(_____)(____/(____)     ~


                          Version: 0.9.4

1. 介绍 

重要提示：在2017年11月18日之后，python-mode经历了一次重要的重构，因此一些功能的表现
不如预期，所以当遇到了文档中前后矛盾/错误的地方，请保持耐心并且上报给我们。上报的同
时，请留意已经提交的bug和issuse，以免重复提交。

Python-mode 是一个vim插件，它允许你在vim中使用类似pylint，rope，pydoc库的特性来进行代码
检查，重构，以及其他类似的事情。

这个插件允许你在不安装pylint或者rope这样的库的同时，通过非常轻松的方式编写python代码的同时。

Python-mode 包含你在vim中编写python应用所需要的所有功能

特性:
- 支持python2.6+到3.2+版本
- 语法高亮 
- 虚拟环境支持
- 运行Python代码 (``<leader>r``)
- 添加/移除端点(``<leader>r``)
- 支持Python锁紧 
- Python 代码折叠
- VIM风格的代码移动 (``]]``, ``3[[``, ``]]M``, ``vaC``, ``viM``,
  ``daC``, ``ciM``, ...)
- 同步代码检查
- 自动纠正非PEP8风格代码 (``:PymodeLintAuto``)
- Python文档检索(``K``)
- 代码重构 <rope refactoring library> (rope_)
- 代码自动填充 (rope_)
- 定义跳转(``<C-c>g`` for `:RopeGotoDefinition`)
- 更多功能 

2.基础功能

脚本可以提供下面的选项来定制化Python-mod,你可以载.vimrc文件中设置。

下面是配置的默认选项

打开Python-mode的所有插件
>
	let g:pymode = 1

关闭插件的警告
>
	let g:pymod_warning = 1

添加路径到`sys.path`中
在[]中添加路径的列表
>
	let g:pymode_paths = []

保存文件时自动删除无用的空格:
>
	let g:pymode_trim_whitespaces = 1

配置默认的python选项
>
	let g:pymode_options = 1

如果当前选项设置为1，Python-mode会自动对缓冲区中的Python代码应用以下配置项：
 >
	setlocal complete+=t
	setlocal formatoptions-=t
	if v:version > 702 && !&relativanumber
		setlocal number
	endif
	setlocal nowrap
	setlocal textwidth=79
	setlocal commentstring=#%s
	setlocal define=^\s*\\(def\\\\|class\\)

设置最大行宽
>
	let g:pymode_options_max_line_length = 79

在最大行宽出设置有色列
>
	let g:pymode_options_colorcolumn = 1

设置Python-mode|自动填充|窗口
>
	let g:pymode_quckfix_minheight = 3
	let g:pymode_quckfix_maxheight = 6

设置Python-mode|预览|窗口高度
预览窗口用于展示文档和|pymode-run|的输出内容
>
	let g:pymode_preview_height = &previewheight
	
设置|预览|窗口的展示位置:
>
	let g:pymode_preview_position = 'botright'

本条的设置内容会影响通过`:new`命令创建出来的窗口，例如. `botright`。

---
2.1 Python版本

Python-mod默认使用你的VIM中支持的python版本，你可选择使用更早的版本，但是
该配置的选项会在加载的时候被测试
>
	let g:pymode_python = 'python'

该配置项的取值为`python`，`python3`，`disable`三个值，如果设置为`disable`
大多数Python-mode的特性将不可用

将值设置为`python3`，如果你的项目中使用python3，那么可以将可以使用|exrc|

---

2.2 Python缩进

Python-mod支持PEP8风格的缩进
设置Python-mode缩进可用
>
	let g:pymode_indent = 1
	
---

2.3 Python代码折叠:

设置Python代码可折叠
>
	let g:pymode_folding = 0

当前折叠功能是实验性质的功能，针对这个功能当前还存在几个issues

---

2.4 Vim风格的移动

支持Vim风格的Pyhon对象移动（例如：函数，类，方法）

`C` - 代表类
`M` - 代表方法

按键	命令

[[		跳转到之前的类或者函数(普通模式、视图模式、命令模式)

]]		跳转到下一个类或者函数(普通模式、视图模式、命令模式)

[M		跳转到之前的类或者方法(普通模式、视图模式、命令模式)

]M		跳转到下一个类或者方法(普通模式、视图模式、命令模式)

aC		选择一个类对象。例如:`vaC,daC,yaC,caC`(普通模式、视图模式、命令模式)

iC		选择类之间的内容。例如：`viC,diC,yiC,ciC`(普通模式、视图模式、命令模式)

aM		选择一个函数或者方法。例如：`vaM,daM,yaM,caM`(普通模式、视图模式、命令模式)

iM		选择函数或者方法之间的内容。例如:`viM,diM,yiM,ciM`(普通模式、视图模式、命令模式)

设置Python-mode的移动有效：
>
	let g:pymode_motion = 1

---




