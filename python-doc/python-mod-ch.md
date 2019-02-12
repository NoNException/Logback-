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
	let g:pymode_run + 1

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

Rope项目文件

*:PymodeRopeNewProject* [<path>] -- 在给定的目录下打开Rope项目
*:PymodeRopeRegenerate* -- 重新生成项目缓存

Rope使用一个名为`.ropeproject`的文件夹来保存项目的配置信息和数据。建议不要讲.repeproject文件
夹添加到版本控制中

当前这个文件主要使用在下面几个方面

* `config.py`文件主要包含了项目中的配置。查看config.py(默认当文件不存在时自动创建)文件了解更多信息。
* 自动保存项目历史，因此当下次打开项目时也能撤销以前的更改
* 用于保存信息和引用
* 用于保存全区命名缓存，可以用来实现自动导入
	
默认情况下，如果当前路径下没有`.ropeproject`文件夹，rope会递归在索索当前路径的父目录

警告：如果rope在父目录下找到`.ropeproject`文件夹，它自动应用于该父目录下的所有子路径，这将
导致扫描的过程变慢(因为这个过程包含了很多无关的文件)

设置在父目录下查询|.ropeproject|
>
    let g:pymode_rope_lookup_project = 0

你也可以手动设置rope项目的路径。如果没有特别指定，rope将使用当前文件路径
>
    let g:pymode_rope_project_root = ""

如果你希望修改`.ropeproject`的默认路径到项目之外。rope将会将这个新的路径作为功能的资源文件，
因此这个路径会总是关联到项目的跟你那路径。你可以使用`..`来将.ropeproject文件放置到项目根目录
之外。
>
    let g:pymode_rope_ropefolder='.ropeproject'

展示光标下元素的文档

展示光标元素下的文档，如果为空则禁用快捷键功能
>
    let g:pymode_rope_show_doc_bind = '<C-c>d'

当保存时(如果文件被修改过)，重新生成项目缓存
>
    let g:pymode_rope_regenerate_on_write = 1

---

4.1 自动完成

默认情况下，可以使用<Ctrl-Space>来完成自动填充。第一个选项会被自动选择并被插入到代码装中，如果按下<Return>，同样的功能快捷键<C-X><C-O> and <C-P>/<C-N>也一样。

默认情况下,自动完成功能在插入模式下，等待一段时间也可以被唤起。

打开自动完成功能
>
    let g:pymode_rope_completion = 1

打开在插入模式下输入的自动完成更能
>
    let g:pymode_rope_complete_on_dot = 1

绑定自动完成的快捷键
>
    let g:pymode_rope_completion_bind = '<C-Space>'

导入的自动完成(rope可以自动导入)
>
    let g:pymode_rope_autoimport = 0

默认情况下的自动导入
>
    let g:pymode_rope_autoimport_modules = ['os', 'shutil', 'datetime']

在在自动完成之后提示未解决的导入
>
    let g:pymode_rope_autoimport_import_after_complete = 0

---

4.2 跳转到定义

默认情况下使用*<C-C>g跳转到定义，设置为空可以禁用该功能
>
    let g:pymode_rope_goto_definition_bind = '<C-c>g'

设置当跳转定义时运行的命令
可选的内容为(`e`, `new`, `vnew`)
>
    let g:pymode_rope_goto_definition_cmd = 'new'
	
---

4.3 重构

重命名光标下的方法/函数/类/变量
>
    let g:pymode_rope_rename_bind = '<C-c>rr'

重命名当前的模块/包

*:PymodeRopeRenameModule* -- 重命名当前的模块 

绑定当前功能快捷键
>
    let g:pymode_rope_rename_module_bind = '<C-c>r1r'

导入

*:PymodeRopeAutoImport* -- 解决光标下元素的导入

组织导入的顺序问题，根据PEP8，未使用的导入将被删除

>
    let g:pymode_rope_organize_imports_bind = '<C-c>ro'

插入当前光标下的导入快捷键
通过|'g:pymode_rope_autoimport'|配置项启用
>
    let g:pymode_rope_autoimport_bind = '<C-c>ra'

将模块转换成包
*:PymodeRopeModuleToPackage* -- 将模块转换成包

快捷键绑定
>
    let g:pymode_rope_module_to_package_bind = '<C-c>r1p'

提取方法/变量

提取选定的行的方法/变量
>
    let g:pymode_rope_extract_method_bind = '<C-c>rm'
    let g:pymode_rope_extract_variable_bind = '<C-c>rl'

查询方法的使用

这个功能将查询到方法被使用的地方，并且？更改代用它的代码
>
    let g:pymode_rope_use_function_bind = '<C-c>ru'

移动方法/变量

当你在重构一个类的方法时，这个类会重新构建出一个方法，老的方法将会调用新的方法。如果你想
修改所有老方法出现的地方，你可以以后进行修改
>
    let g:pymode_rope_move_bind = '<C-c>rv'

修改方法签名
>
    let g:pymode_rope_change_signature_bind = '<C-c>rs'

---

4.4 撤回/重做变更

命令:
*:PymodeRopeUndo* -- 撤销项目中最近的变更
*:PymodeRopeRedo* -- 重做项目中最近的变更

---

5. 语法

开启语法检查
>
    let g:pymode_syntax = 1

同步的语法价检查会降低响应的时间，在配置较低的硬件平台上应该考虑禁用该功能。
>
    let g:pymode_syntax_slow_sync = 1

启用所有的python
>
    let g:pymode_syntax_all = 1

高亮'print'
>
		let g:pymode_syntax_print_as_function = 0

高亮异步/等待的代码
>
    let g:pymode_syntax_highlight_async_await = g:pymode_syntax_all

高亮'='
>
    let g:pymode_syntax_highlight_equal_operator = g:pymode_syntax_all

高亮"*"
>
    let g:pymode_syntax_highlight_stars_operator = g:pymode_syntax_all

高亮'self'关键字
>
    let g:pymode_syntax_highlight_self = g:pymode_syntax_all

高亮缩进错误
>
    let g:pymode_syntax_indent_errors = g:pymode_syntax_all

高亮空格错误
>
    let g:pymode_syntax_space_errors = g:pymode_syntax_all

高亮格式化字符串
>
    let g:pymode_syntax_string_formatting = g:pymode_syntax_all
    let g:pymode_syntax_string_format = g:pymode_syntax_all
    let g:pymode_syntax_string_templates = g:pymode_syntax_all
    let g:pymode_syntax_doctests = g:pymode_syntax_all

高亮内建关键词(True,False...)
>
    let g:pymode_syntax_builtin_objs = g:pymode_syntax_all

高亮内建对象
>
    let g:pymode_syntax_builtin_types = g:pymode_syntax_all

高亮异常
>
    let g:pymode_syntax_highlight_exceptions = g:pymode_syntax_all

高亮文档字符串
>
    let g:pymode_syntax_docstrings = g:pymode_syntax_all

---

6. FAQ

1. Python-mode不能正常工作
---

记住要选用最近的源码，并且更新更新项目的导入

清除所有的python的缓存/编译代码(`*.pyc`和`__pycache__`目录下的所有文件).
在Linux/Unix/MacOS的系统下你可以下运行下面的命令

`find . -type f -name '*.pyc' -delete && find . -type d -name '__pycache__' -delete`

然后使用下面的命令启用Python-mode
`vim -i NONE -u <path_to_pymode>/debugvimrc.vim`
你可以重新构建含有错误信息的文件,你可以通过`:message`查看到类似的信息:
`pymode debug msg 1: Starting debug on: 2017-11-18 16:44:13 with file /tmp/pymode_debug_file.txt`请提交这个文件下的所有内容

2. Rope自动完成更能很缓慢
---

Rope将在|.ropeproject|下创建项目级别的服务路径

如果在当前目录下没有发现`.ropeproject`，rope将会向上上级目录寻找`.ropeproject`文件夹，他将会设置
所有的子目录，因此扫描的过程将会变得缓慢

解决方式:
- 删除父目录下的`.ropeproject`文件夹，并且在当前文件夹下创建`.ropeproject`
- 运行``:PymodeRopeNewProject``命令来创建行的`ropeproject`文件
- 设置选项|g:pymode_rope_lookup_project|为0来取消想上查询的功能

也考虑设置|’g:pymode_rope_lookup_project'|来人工指定Rope的路径

3. Pylint检查非常缓慢
---

在一些项目中pylint可能表现的很慢，它可能会扫描所有可能的导入模块，尝试使用其他的代码检查模块:
查看|'g:pymode_lint_checkers'|

你也可以在|vimrc|中设置|exrc|和|secure|来自动以当前项目的代码检查?

4. OSX系统下不能导入urandom
---

查看 https://groups.google.com/forum/?fromgroups=#!topic/vim_dev/2NXKF6kDONo

下面的命令可以修复这个功能
>
    brew unlink python
    brew unlink macvim
    brew remove macvim
    brew install -v --force macvim
    brew link macvim
    brew link python

5. 代码折叠缓慢
---

Python-mod可以折叠定义和多行的文档代码.这在大文件中将会消耗一定量的计算。可以考虑
在.vimrc中设置
>
	let g:pymode_folding = 1
来禁用这个功能

考虑到在多个窗口下进行编辑时，Python-mode将会计算每一次编辑的折叠。因此如下的编辑可能会有一定的
改善
>
    augroup unset_folding_in_insert_mode
        autocmd!
        autocmd InsertEnter *.py setlocal foldmethod=marker
        autocmd InsertLeave *.py setlocal foldmethod=expr
    augroup END


---

7.开发指导

本章简短的介绍了python-mode的开发指导

1. 本帮助文件使用|help-writing|中定义的vim内容进行编写?
2. 在文档中应该是应该使用‘python-mode'描述插件,除非在句子的第一行才使用Python-mode
3. 所有的函数定义应该应该使用vim的定义并且以’Pymode‘开头
4. 项目开发中可以使用`XXX`和`TODO`这样的特殊表，他们可以为开发者提供方便的查询
5. 提交的代码请求应该包含覆盖了bug/feature中内容的测试.查看`test/test.sh`(1)文件了解更新的信息
建议的文件结构如下:添加代码测试到`test/test_bash`(2)，vim脚本添加到`test/test_procedures_vimscript`(3)。尽量尝试使用已经定义在`test/test_python_sample_code`。文件(1)需要能够触发最新添加的文件(2)。并且能够调用文件(3)。文件(3)然后需要读取文件(4)作为第一步断言，然后继续执行后续所有的断言.

---

8.认证

8. Credits ~
                                                                 *pymode-credits*
    Kirill Klenov
        http://klen.github.com/
        http://github.com/klen/

    Rope
        Copyright (C) 2006-2010 Ali Gholami Rudi
        Copyright (C) 2009-2010 Anton Gritsay

    Pylint
        Copyright (C) 2003-2011 LOGILAB S.A. (Paris, FRANCE).
        http://www.logilab.fr/

    Pyflakes:
        Copyright (c) 2005-2011 Divmod, Inc.
        Copyright (c) 2013-2014 Florent Xicluna
        https://github.com/PyCQA/pyflakes

    PEP8:
        Copyright (c) 2006 Johann C. Rocholl <johann@rocholl.net>
        http://github.com/jcrocholl/pep8

    autopep8:
        Copyright (c) 2012 hhatto <hhatto.jp@gmail.com>
        https://github.com/hhatto/autopep8

    Python syntax for vim:
        Copyright (c) 2010 Dmitry Vasiliev
        http://www.hlabs.spb.ru/vim/python.vim

    PEP8 VIM indentation
        Copyright (c) 2012 Hynek Schlawack <hs@ox.cx>
        http://github.com/hynek/vim-python-pep8-indent

		

===============================================================================
9. License ~
                                                                 *pymode-license*
Python-mod基于GUN软件开协议
查看: http://www.gnu.org/copyleft/lesser.html

如果你喜欢这个插件，非常有幸收到你的明信片

我的地址是: "Russia, 143500, MO, Istra, pos. Severny 8-3" to "Kirill
Klenov".感谢你的资瓷



