---
description: vim 小结 熟能生巧吧
---

# VIMTUTOR

### 第一讲小结

1. 光标在屏幕文本中的移动既可以用箭头键，也可以使用 hjkl 字母键。 **h \(左移\)** **j \(下行\)** **k \(上行\)** **l \(右移\)**
2. 欲进入 Vim 编辑器\(从命令行提示符\)，请输入：vim 文件名 &lt;回车&gt;
3. 欲退出 Vim 编辑器，请输入 **:q!** 放弃所有改动。或者输入 **:wq** 保存改动。
4. 在正常模式下删除光标所在位置的字符，请按： **x**
5. 欲插入或添加文本，请输入：

   **i** 在光标前插入文本

   **a** 在光标后插入文本

   **A** 在一行后添加文本

### 第二讲小结

1. 欲从当前光标删除至下一个单词，请输入：**dw**
2. 欲从当前光标删除至当前行末尾，请输入：**d$**
3. 欲删除整行，请输入：**dd**
4. 欲重复一个动作，请在它前面加上一个数字：2w
5. 在正常模式下修改命令的格式是：

   > **operator \[number\] motion**

   * **operator**  操作符，代表要做的事情，比如 d 代表删除，c 代表更改
   * **number**  可以附加的数字，代表动作重复的次数
   * **motion**  动作，代表在所操作的文本上的移动，例如 w 代表单词\(word\)，$ 代表行末等等

6. 欲移动光标到行首，请按数字0键：**0**
7. 欲撤消以前的操作，请输入：**u \(小写的u\)**

   欲撤消在一行中所做的改动，请输入：**U \(大写的U\)**

   欲撤消以前的撤消命令，恢复以前的操作结果，请输入：**CTRL-R**

### 第三讲小结

1. **p** 将最后一次删除的内容置入光标后面
2. **r** key 替换光标所在位置的字符
3. **ce** 删除一个单词并进入插入模式
4. **c** **\[number\]** **motion**
   * **c** 操作符‘change’，更改的意思

### 第四讲小结

1. **CTRL + g** 显示当前编辑文件状态信息，光标位置、行号等
2. **G** 跳转到文件最后一行
3. **gg** 跳转到文件最后一样
4. **行号 + G** 跳转到该行
5. **/**  或者  **?** 进行搜索，区别是 **?** 进行逆向搜索，**n**\(**N**\)向下（上）查找，**CTRL + o** 或者 **CTRL + i** 回到搜索前的位置
6. **%** 可以搜索括号\( \) \[ \] { }，在程序调试时查找不匹配的括号很有用

### 第五讲小结

1. **:!** 可以执行外部的 shell 命令，如：ls
2. **:w FILENAME** 创建保存文件
3. **v** 进入可视模式进行选取
4. **:r FILENAME** 读取文件内容，并从光标位置处置入

### 第六讲小结

1. **o**\(**O**\) 在光标下方（上方）新开一行并进入插入模式
2. **a** 在光标后插入文本
3. **R** 可连续替换多个字符
4. **v** 可视模式选取 **y** 复制 **p** 粘贴
5. **:set ic** 设置搜索是忽略大小写\(**noic**\)

   **:set hls** 设置搜索时高亮\(**nohlsearch**\)

   **:set is** 设置键入是就搜索\(**nois**\)

### 第七讲小结

1. **:help** 打开帮助窗口
2. 创建一个 .vimrc 保存偏好设置
3. 插入模式下**CTRL + d** 或 /TAB/ 可以进行命令自动补全

一些基本的 vim 配置

```vim
" --- ~/.vimrc ---
set number
set ignorecase smartcase
set smartindent
set tabstop=2
set expandtab
set shiftwidth=2

nnoremap ,p "0p
nnoremap ,P "0P
nnoremap J gT
nnoremap K gt
```

### 其他

* **H** 窗口头部 **M** 窗口中部 **L** 窗口尾部
* **J** 将当前行与下一行链接在一起 **K** 查询光标下单词的手册页解释
* **w** 下一个单词的开头 **e** 下一个单词的结尾 **b** 上一个单词的开头
* 可视模式 `o` 指令

  ```text
   v         逐字符可视模式
   V         逐行可视模式
   Ctrl-v    逐块可视模式
  ```

* **Text Objects**

  ```text
   w     一个单词
   p     一个段落
   s     一个句子
   (或)  一对()
   {或}  一对{}
   [或]  一对[]
   <或>  一对<>
   t     XML标签
   "     一对""
   '     一对''
   `     一对``
  ```

* 也可以使用 NeoVim

  一些常用的 NeoVim 配置

   ```vim
   " --- ~/.config/nvim/init.vim ---
   " Vim-Plug
   call plug#begin()
   " Editor theme
   Plug 'folke/tokyonight.nvim', { 'branch': 'main' }
   " Highlight difference modes
   Plug 'itchyny/lightline.vim'
   " Go to definition
   Plug 'neoclide/coc.nvim', {'branch': 'release'}
   call plug#end()

   " Vim Script
   colorscheme tokyonight

   " COC key map
   nmap <silent> gd <Plug>(coc-definition)
   nmap <silent> gD <Plug>(coc-implementation)
   nmap <silent> gr <Plug>(coc-references)

   " Use config from .vimrc
   set runtimepath^=~/.vim runtimepath+=~/.vim/after
   let &packpath = &runtimepath
   source ~/.vimrc
   ```
