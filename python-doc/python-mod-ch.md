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

2.5 文档展示

Python-mod 可以通过`pydoc`来展示当前词汇的python文档。

命令：`:PymodeDoc` <args> - 展示文档 

打开Python-mod文档功能
>
	let g:pymode_doc = 1

绑定快捷键方式展示Python文档
>
	let g:pymode_doc_bind = 'K'

---

2.6 支持虚拟环境

命令模式下输入:`:PymodeVirtualenv` <path> -- 启用虚拟环境(路径可以为绝对路径或者当前路径的相对路径)

启用虚拟环境自动化检测:
>
	let g:pymode_virtualenv = 1

手动设置虚拟环境的路径
>
    let g:pymode_virtualenv_path = $VIRTUAL_ENV

---

2.7 运行代码

命令模式下输入:`:PymodeRun` -- 运行当前缓冲区或者选择的代码

开启运行代码功能:
>
	let g:pymode_run - 1

绑定快捷键运行Python代码:
>
	let g:pymode_run_bind = '<leader>r'

---

2.8 断点功能

Python-mode可以体用有效的Debugger工具(例如 pdb,ipdb,pudb)并且用户可以一键设置/取消
断点。

设置改功能有效:
>
    let g:pymode_breakpoint = 1
	
绑定快捷键：
>
    let g:pymode_breakpoint_bind = '<leader>b'

人工设置断点（设置为空时，标识自动标识断点)
>
    let g:pymode_breakpoint_cmd = ''


---

3.代码检测

Python-mod 支持`pylint`,`pep257`,`pep8`,`pyflakes`,`maccabe`的代码检查，可以同时运行几个这样的检查:

`Python-mod使用Pylama库用于代码检查.在pylama.ini文件中可以定义例如跳过文件，错误等功能。可以查看Pylama的文档了解更多功能`

`Pylint的相关配置被定义在$HOME/pylint.rc文件中，查看pylint了解更多细节`

命令模式下输入:

*:PymodeLint* -- 检查当前缓冲区中的代码
*:PymodeLintToggle* -- ？
*:PymodeLintAuto* -- 修复当前缓冲区中不符合PEP8的代码

开启代码检测:
>
	let g:pymode_lint = 1

在每次保存代码时进行检测(如果当前文件被修改的前提下)
>
    let g:pymode_lint_on_write = 1

当进行文件编辑时修改代码:
>
    let g:pymode_lint_on_fly = 0

当鼠标防止在错误的一行时显示错误的详情
>
    let g:pymode_lint_message = 1
	
设置默认的代码检测器(你可以同时设置多个)
>
    let g:pymode_lint_checkers = ['pyflakes', 'pep8', 'mccabe']

该配置有一下可选项 `pylint`, `pep8`, `mccabe`, `pep257`, `pyflakes`

跳过错误和警告

`W`, `E2` 其中E2用来跳过所有的错误和警告
>
    let g:pymode_lint_ignore = ["E501", "W",]

选择一些错误或者警告:

例如，你可以禁用所有'W'标识的警告的同时查看`W0011`或者`W430`
>
    let g:pymode_lint_select = ["E501", "W0011", "W430"]

对错误进行排序
如果非空，错误会按照预先订单的顺序进行排列，例如E.g. let g:pymode_lint_sort = ['E','C','I'] ？
>
    let g:pymode_lint_sort = []

当发现错误时自动打开(quickfix)窗口
>
    let g:pymode_lint_cwindow = 1

开启放置标志
>
    let g:pymode_lint_signs = 1

标志的放置的定义如下:
>
    let g:pymode_lint_todo_symbol = 'WW'
    let g:pymode_lint_comment_symbol = 'CC'
    let g:pymode_lint_visual_symbol = 'RR'
    let g:pymode_lint_error_symbol = 'EE'
    let g:pymode_lint_info_symbol = 'II'
    let g:pymode_lint_pyflakes_symbol = 'FF'

---

3.1 设置代码检查选项

Python-mode有能力通过几个选项来设置代码检查规范的选项

设置PEP8的选项
>
    let g:pymode_lint_options_pep8 =
        \ {'max_line_length': g:pymode_options_max_line_length}

查看链接 https://pep8.readthedocs.org/en/1.4.6/intro.html#configuration 了解更多信息.

设置Pyflakes选项
>
    let g:pymode_lint_options_pyflakes = { 'builtins': '_' }

设置maccabe选项
>
    let g:pymode_lint_options_mccabe = { 'complexity': 12 }

设置pep257选项
>
    let g:pymode_lint_options_pep257 = {}

设置pylint选项
>
    let g:pymode_lint_options_pylint =
        \ {'max-line-length': g:pymode_options_max_line_length}

查看 http://docs.pylint.org/features.html#options 获取更多信息 

---

4. Rope支持

Python-mode支持基于Rope库的代码重构选项、代码自动补充和帮助
命令:
|:PymodeRopeAutoImport| -- 解决鼠标选中元素的导入功能
|:PymodeRopeModuleToPackage| -- 将当前模块转化成包 
|:PymodeRopeNewProject| -- 在当前工作目录下打开新的Rope项目
|:PymodeRopeRedo| -- 撤回上次重构的更改
|:PymodeRopeRegenerate| --  重新产生工程的缓存
|:PymodeRopeRenameModule| -- 重新命名当前模块
|:PymodeRopeUndo| -- 取消上次重构的更能该

打开Rope脚本的重构更能
>
    let g:pymode_rope = 1

Rope项目的折叠


