---
title: 'Vimrc配置文件'
date: 2017-11-10 14:39:00
tags: 
	- Vim
categories:
	- Linux
---
# 我的vimrc配置文件


``` vimrc 
"目录
"第一章:基本全局设定{{{
"1.1 语法高亮
"1.2 配色方案设置
"1.3 显示行号设置
"1.4 不兼容vi
"1.5 使用系统剪切板
"1.6 设置当前行高亮 
"1.7 设置状态栏 
"1.8 vimrc的折叠设定 
"1.9 搜索高亮设置  
"1.10 制表符设置 
"1.11 自动换行
"1.12 文件类型检测
"1.13 鼠标支持
"1.14 编码支持
"}}} 
"第二章：映射{{{
"2.1 leader setting and local-leader setting
"2.2 correct the bad habbits.
"2.3 set the abbreviation
"2.4 workman layout
"2.5 make the select-area surround by the symbol you need.
"2.6 快速设置vimrc
"}}}
"第三章：自动匹配和自动补全{{{
"3.1 {}匹配
"3.2 ()匹配
"3.3 ""匹配
"3.4 ''匹配
"3.5 <>匹配
"3.6 <tab>自动补全
"}}}
"第四章：插件{{{
"}}}
"第五章：Java语言设置{{{
"5.1 Java anotationn
"5.2 Java 编译设置
"5.3 Java 运行设置
"5.4 Java 输出缩写
"5.5 Java 主函数缩写
"}}}

"1.1-1.4 合并设置{{{
"1.1语法高亮
syntax on

"1.2配色方案
colorscheme desert

"1.3显示行号
set nu

"1.4设置不兼容vi
:set nocompatible
"}}}
"1.5 用于处理复制粘贴问题{{{
"需要先查看是否支持系统剪贴板
"普通模式映射sp作为粘贴命令
nmap sp "+p
"可视模式下映射sy作为复制命令
vmap sy "+y
"}}}
"1.6 当前行高亮{{{
set cursorline

"当前行高亮的配色:透明带
hi CursorLine   cterm=NONE ctermbg=237 ctermfg=NONE guibg=darkred guifg=white
"}}}
"1.7 设置状态栏{{{
:set laststatus=2
:set statusline=%4*\ %F\ %* 
:set statusline+=%=
:set statusline+=\ %l
:set statusline+=/
:set statusline+=%L
"}}}
"1.8 vimrc折叠设定---------------{{{
set foldmethod=indent
augroup filetype_vim
	autocmd!
	autocmd FileType vim setlocal foldmethod=marker
augroup END
nnoremap <space> za
"}}}
"1.9 搜索设置{{{
set hlsearch
"}}}
"1.10 设置制表符与空格转化{{{
set tabstop=4
"}}}
"1.11 基本自动对齐{{{
set autoindent
set smartindent
set shiftwidth=4
set showmatch
"}}}
"1.12 文件类型检测{{{
filetype plugin indent on
"}}}
"1.13 鼠标支持{{{
set mouse=a
"}}}
"1.14 编码支持{{{
set fileencodings=utf-8,gbk,gb18030,utf-16,big5
"}}}

"2.1 设置前缀{{{
"设置前缀leader
let mapleader = ";"

"设置本地前缀localleader
let maplocalleader = "," 
"}}}
"2.2 改正坏习惯{{{
"设置插入模式快速返回普通模式,并取消esc
inoremap hh <esc>
" inoremap <esc> <nop>

"取消上下左右键
map <left> <nop>
map <right> <nop>
map <up> <nop>
map <down> <nop>
"}}}
"2.3 设置abbreviation{{{
iabbrev @@ 1143515591@qq.com
iabbrev hmc 何铭春
"}}}
"2.4 设置workman键盘布局{{{
" nnoremap j k
" nnoremap k j
"}}}
"2.5 设置选中区域加引号{{{
vnoremap hh <esc>a"<esc>`<i"<esc>
"}}}
"2.6 vimrc快速设置{{{

"快速编辑vimrc
nnoremap <leader>ev :vsplit $MYVIMRC<cr>

"快速重载vimrc
:nnoremap <leader>sv :source $MYVIMRC<cr>
"}}}

"3.1 {}匹配{{{
inoremap { {}<esc>i<cr><esc>ko
"}}}
"3.2 ()匹配{{{
inoremap ( ()<esc>i

function SkipParen()
	let temp=getline('.')[col('.')-1]
	if temp==")"
		return "\<esc>la"
	else
		return ")"
	endif
endfunction

inoremap ) <c-r>=SkipParen()<cr>
"}}}
"3.3 ""匹配{{{
function SkipQuotation()
	let temp=getline('.')[col('.')-1]
	if temp=='"'
		return "\<esc>la"
	else
		return '""'."\<esc>i"
	endif
endfunction

inoremap " <c-r>=SkipQuotation()<cr>
"}}}
"3.4 ''匹配{{{
function SkipOtherQuotation()
	let temp=getline('.')[col('.')-1]
	if temp=="'"
		return "\<esc>la"
	else
		return "''\<esc>i"
	endif
endfunction

inoremap ' <c-r>=SkipOtherQuotation()<cr>
"}}}
"3.5 <>匹配{{{
inoremap < <><esc>i

function SkipBook()
	let temp=getline('.')[col('.')-1]
	if temp==">"
		return "\<esc>la"
	else
		return ">"
	endif
endfunction

inoremap > <c-r>=SkipBook()<cr>
"}}}
"3.6 <tab>自动补全{{{
set omnifunc=omni
inoremap <tab> <c-n>
"}}}

"vimrc取消部分匹配
	autocmd FileType vim inoremap { {

"5.1 Java 注释{{{
function JavaAnnotation()
let temp=getline(".")
let aTemp=temp[0].temp[1]
if aTemp ==# "//"
normal ^xx
else
normal I//
endif
endfunction

:autocmd FileType java nnoremap <buffer><leader>a :call JavaAnnotation()<cr>
"}}}
"5.2 Java 编译设置{{{
set makeprg=javac\ @%
:autocmd FileType java nnoremap <buffer><F4> :!javac %<cr>
"}}}
"5.3 运行Java{{{
:autocmd FileType java nnoremap <buffer><F5> :!java %:r<cr>
"}}}
"5.4 输出配置{{{
autocmd FileType java iab pl System.out.println("
autocmd FileType java iab pp System.out.print("
"}}}
"5.5 main函数设置{{{
:autocmd FileType java iab main public static void main(String[] args){
"}}}
```
